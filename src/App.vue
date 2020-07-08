<template>
  <div id="app">
    <h1>Grid Problem</h1>
    <grid />
  </div>
</template>

<script>
import grid from "./components/grid";

export default {
  name: "App",
  components: {
    grid,
  },
};
</script>

<style>
body {
  background-color: #242453;
  color: white;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  margin-top: 60px;
}
</style>
