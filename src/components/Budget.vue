<template>
  <div class="budget">
    <h1>{{ title }}</h1>
    <div v-show="!editing">
      <span @click="editing = true">Total Income: <currency v-bind:number="income"></currency></span>
    </div>
    
    <form v-show="editing" v-on:submit.prevent="doneEdit">
      <input v-model="new_income"
        @keyup.enter="doneEdit">
      <button type="submit">Save</button>
    </form>

    <span>Remaining to Budget: <currency v-bind:number="remaining_to_budget"></currency></span><br>
    <span>Remaining to Spend: <currency v-bind:number="remaining_to_spend"></currency></span>
    <transition-group name="category" tag="ul">
      <Category v-for="category in categories" :category="category" v-bind:key="category"></Category>
    </transition-group>
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
import Currency from './Currency'
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

      return this.$store.state.income - remaining
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
    Category,
    Currency
  }
}
</script>

<style scoped>

.category-list-item {
  display: inline-block;
  margin-right: 10px;
}
.category-list-enter-active{
  transition: all .5s;
}
.category-list-leave-active {
  transition: all .3s;
}
.category-list-enter, .category-list-leave-to, .category-list-leave-active {
  opacity: 0;
  transform: translateX(30px);
}
h1, h2 {
  font-weight: normal;
}

a {
  color: #42b983;
}

ul {
  list-style: none;
  padding: 0;
  margin: 0;
}
</style>
