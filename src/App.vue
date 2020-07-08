<template>
  <div id="app">
    <h1>Grid Problem</h1>
    <vue-slider v-model="gridWidth" :min="2" :max="25" />
    <div>
      <div class="color-picker">
        <verte v-model="blockColor" model="hex"></verte><span>Block Color</span>
      </div>
      <div class="color-picker">
        <verte v-model="blockHoverColor" model="hex"></verte
        ><span>Block Hover Color</span>
      </div>
    </div>

    <button @click="shuffle">Shuffle Grid</button>

    <grid
      ref="_grid"
      :gridWidth="gridWidth"
      :blockColor="blockColor"
      :blockHoverColor="blockHoverColor"
    />
  </div>
</template>

<script>
import grid from "./components/grid";
import "./assets/styles/index.css";
import VueSlider from "vue-slider-component";
import "vue-slider-component/theme/antd.css";
import Verte from "verte";
import "verte/dist/verte.css";

export default {
  name: "App",
  components: {
    VueSlider,
    grid,
    Verte,
  },
  data() {
    return { gridWidth: 10, blockColor: "#7f6ffd", blockHoverColor: "#ffffff" };
  },
  methods: {
    shuffle() {
      this.$refs._grid.shuffle();
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  margin: 60px;
}
.color-picker {
  display: inline-block;
  margin: 1em;
}
</style>
