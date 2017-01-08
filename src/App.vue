<template>
  <div id="app">
    <budget></budget>
  </div>
</template>

<script>
import Budget from './components/Budget'

export default {
  name: 'app',
  components: {
    Budget
  }
}
</script>

<style>

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}
</style>
