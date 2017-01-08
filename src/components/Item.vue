<template>
  <li class="item">
    <div v-show="!editing">
      <label><input type="checkbox" v-model="spent"></label>
      <h4 @click="editing = true">{{ item.title }}</h4>
      <span @click="editing = true">${{ item.price | currency }}</span>
      <button @click="removeItem">X</button>
    </div>
    <form v-show="editing" v-on:submit.prevent="doneEdit">
      <input v-model="item.title"
        @keyup.enter="doneEdit">
      <input v-model="item.price"
        @keyup.enter="doneEdit">
      <button type="submit">Save</button>
    </form>
  </li>
</template>

<script>
export default {
  name: 'item',
  props: ['item'],
  data () {
    return {
      editing: false,
      spent: this.item.spent
    }
  },
  methods: {
    doneEdit (e) {
      const item = this.item
      const title = this.item.title
      const price = this.item.price

      if (this.editing) {
        this.$store.commit('editItem', {item, title, price})
        this.editing = false
      }
    },
    removeItem () {
      const item = this.item
      this.$store.commit('removeItem', {item})
    }
  },
  watch: {
    spent () {
      const item = this.item
      const spent = this.spent
      this.$store.commit('spentItem', {item, spent})
    }
  },
  filters: {
    currency (num) {
      return Number(num).toFixed(2).replace(/(\d)(?=(\d{3})+\.)/g, '$1,')
    }
  }
}
</script>

<style scoped>
h4 {
  margin: 0;
  padding: 0;
}

li.item {
  display: block;
  margin-bottom: 10px;
  padding: 1em;
  border: 1px solid black;
}

.item div {
  display: flex;
  justify-content: space-between;
}

.item div label,
.item div h4,
.item div span,
.item div button {
  flex: 0 1 auto;
}

a {
  color: #42b983;
}
</style>
