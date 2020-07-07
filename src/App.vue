<template>
  <div id="app">
    <grid />
  </div>
</template>

<script>
import grid from './components/grid'

export default {
  name: 'App',
  components: {
    grid
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
