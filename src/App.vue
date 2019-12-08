<template>
  <div id="app">
    <VueTable msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
import VueTable from './components/VueTable.vue'

export default {
  name: 'app',
  components: {
    VueTable
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
