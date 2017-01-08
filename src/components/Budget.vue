<template>
  <div class="budget">
    <h1>{{ title }}</h1>
    <div v-show="!editing">
      <span @click="editing = true">Total Income: ${{income | currency}}</span>
    </div>
    
    <form v-show="editing" v-on:submit.prevent="doneEdit">
      <input v-model="new_income"
        @keyup.enter="doneEdit">
      <button type="submit">Save</button>
    </form>

    <span>Remaining to Budget: ${{ remaining_to_budget | currency }}</span><br>
    <span>Remaining to Spend: ${{ remaining_to_spend | currency }}</span>
    <Category v-for="category in categories" :category="category"></Category>
    <form v-on:submit.prevent="addCategory">
      <button type="submit">Add Category</button>
      <input class="new-category"
          v-model="new_category_title"
          autocomplete="off"
          placeholder="title">
    </form>
  </div>
</template>

<script>
import Category from './Category'
var _ = require('lodash/core')

export default {
  name: 'budget',
  data () {
    return {
      new_category_title: '',
      editing: false,
      new_income: this.$store.state.income
    }
  },
  computed: {
    title () {
      return this.$store.state.title
    },
    income () {
      return this.$store.state.income
    },
    categories () {
      return this.$store.state.categories
    },
    remaining_to_budget () {
      var remaining = 0

      _.each(this.$store.state.categories, function (cat) {
        _.each(cat.items, function (item) {
          remaining += 1 * item.price
        })
      })

      return remaining
    },
    remaining_to_spend () {
      var remaining = 0

      _.each(this.$store.state.categories, function (cat) {
        _.each(cat.items, function (item) {
          if (!item.spent) {
            remaining += 1 * item.price
          }
        })
      })

      return remaining
    }
  },
  filters: {
    currency (num) {
      return Number(num).toFixed(2).replace(/(\d)(?=(\d{3})+\.)/g, '$1,')
    }
  },
  methods: {
    addCategory () {
      const title = this.new_category_title
      if (title.trim()) {
        this.$store.commit('addCategory', {title})
      }
      this.new_category_title = ''
    },
    doneEdit () {
      const income = this.new_income
      this.$store.commit('setIncome', {income})
      this.editing = false
    }
  },
  components: {
    Category
  }
}
</script>

<style scoped>
h1, h2 {
  font-weight: normal;
}

a {
  color: #42b983;
}
</style>
