<template>
  <div id="app">
    <budget></budget>
  </div>
</template>

<script src="http://cdnjs.cloudflare.com/ajax/libs/jquery/2.0.3/jquery.js"></script>
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

button {
  font-family: Arial;
  color: #ffffff;
  font-size: 18px;
  background: #3498db;
  padding: 6px;
  border: solid #1e80bd 2px;
  text-decoration: none;
}

button:hover {
  background: #3cb0fd;
  text-decoration: none;
}
</style>
