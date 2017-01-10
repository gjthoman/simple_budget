<template>
  <li class="category">
    <h3 @click="editing = true" v-show="!editing">{{ category.title }}: <currency v-bind:number="items_total"></currency></h3>
    <form v-show="editing" v-on:submit.prevent="doneEdit">
      <input v-model="category.title"
        @keyup.enter="doneEdit">
      <button type="submit">Save</button>
    </form>
    <h5>Left to spend: <currency v-bind:number="remaining_to_spend"></currency></h5>
      <transition-group name="item" tag="ul">
        <Item v-for="item in category.items" :item="item" v-bind:key="item"></Item>
      </transition-group>
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
  </li>
</template>

<script>
import Item from './Item'
import Currency from './Currency'
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
    Item,
    Currency
  }
}
</script>

<style scoped>
.item-item {
  display: inline-block;
  margin-right: 10px;
}
.item-enter-active{
  transition: all .5s;
}
.item-leave-active {
  transition: all .3s;
}
.item-enter, .item-leave-to, .item-leave-active {
  opacity: 0;
  transform: translateX(30px);
}


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
  position: relative;
  border: 1px solid black;
  padding: 1em;
  margin: 2rem 1rem;
  background-color: #fff3db;
}

ul {
  list-style-type: none;
  padding: 0;
}

a {
  color: #42b983;
}

button {
  position: absolute;
  top: 0;
  right: 0;
}
</style>
