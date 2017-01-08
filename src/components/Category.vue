<template>
  <div class="category">
    <h3 @click="editing = true" v-show="!editing">{{ category.title }}: ${{ items_total | currency }}</h3>
    <form v-show="editing" v-on:submit.prevent="doneEdit">
      <input v-model="category.title"
        @keyup.enter="doneEdit">
      <button type="submit">Save</button>
    </form>
    <h5>Left to spend: ${{ remaining_to_spend | currency }}</h5>
    <ul>
      <Item v-for="item in category.items" :item="item"></Item>
    </ul>
    <form v-on:submit.prevent="addItem">
      <button type="submit">Add Item</button>
      <input v-model="item_title"
        @keyup.enter="addItem"
        placeholder="title">
      <input v-model="item_price"
        @keyup.enter="addItem"
        placeholder="price">
    </form>
    <button @click="removeCategory">Remove Category</button>
  </div>
</template>

<script>
import Item from './Item'
var _ = require('lodash/core')

export default {
  name: 'category',
  props: ['category'],
  data () {
    return {
      editing: false,
      item_title: '',
      item_price: ''
    }
  },
  computed: {
    items () {
      return this.category.items
    },
    items_total () {
      var totalPrice = 0

      _.each(this.category.items, function (item) {
        totalPrice += 1 * item.price
      })
      return totalPrice
    },
    remaining_to_spend () {
      var remaining = 0

      _.each(this.category.items, function (item) {
        if (!item.spent) {
          remaining += 1 * item.price
        }
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
    addItem (e) {
      const title = this.item_title
      const price = this.item_price
      const category = this.category
      if (title.trim()) {
        this.$store.commit('addItem', {category, title, price})
      }
      this.item_title = this.item_price = ''
    },
    doneEdit () {
      if (this.editing) {
        const category = this.category
        const title = this.category.title
        this.$store.commit('editCategory', {category, title})
        this.editing = false
      }
    },
    removeCategory () {
      const category = this.category
      this.$store.commit('removeCategory', {category})
    }
  },
  components: {
    Item
  }
}
</script>

<style scoped>
h3, h5, form {
  text-align: center;
}

h3 {
  margin-bottom: 0;
}

h5 {
  margin-top: 0;
}

.category {
  border: 1px solid black;
  padding: 1em;
}

ul {
  list-style-type: none;
  padding: 0;
}

a {
  color: #42b983;
}
</style>
