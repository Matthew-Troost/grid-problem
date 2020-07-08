<template>
  <div>
    <vue-slider v-model="gridWidth" />
    <verte v-model="blockColor" model="hex"></verte>
    <verte v-model="blockHoverColor" model="hex"></verte>

    <button @click="suffleGrid">Shuffle Grid</button>
    <div v-for="(row, row_index) in grid" :key="row.id">
      <div
        v-for="(col, col_index) in row"
        :key="col.id"
        :style="selectedColors"
        :class="[
          {
            'block--colored': col == 1,
            'block--revealConnections':
              hovering && col == 1 && alreadyTraversed([row_index, col_index]),
          },
          'block',
        ]"
        @click="onBlockClick(row_index, col_index)"
        @mouseover="onBlockHover(row_index, col_index)"
      >
        <div class="center-vertical">
          {{
            selectedBlock[0] == row_index && selectedBlock[1] == col_index
              ? connections.length
              : ""
          }}
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import VueSlider from "vue-slider-component";
import "vue-slider-component/theme/antd.css";
import Verte from "verte";
import "verte/dist/verte.css";

export default {
  components: {
    VueSlider,
    Verte,
  },
  data() {
    return {
      grid: [],
      gridWidth: 5,
      selectedBlock: [],
      connections: [],
      hovering: false,
      blockColor: "#7f6ffd",
      blockHoverColor: "#ffffff",
    };
  },
  computed: {
    selectedColors: function() {
      return {
        "--color": this.blockColor,
        "--color-hover": this.blockHoverColor,
      };
    },
  },
  created() {
    this.suffleGrid();
  },
  methods: {
    getConnections(row_index, col_index) {
      this.recursiveTraverse(row_index, col_index);
    },
    recursiveTraverse(row_index, col_index) {
      let iterationConnections = [];

      //left traverse
      let leftIndex = col_index - 1;
      if (
        leftIndex >= 0 &&
        !this.alreadyTraversed([row_index, leftIndex]) &&
        this.grid[row_index][leftIndex] == 1
      )
        iterationConnections.push([row_index, leftIndex]);

      //right traverse
      let rightIndex = col_index + 1;
      if (
        rightIndex < this.grid[0].length &&
        !this.alreadyTraversed([row_index, rightIndex]) &&
        this.grid[row_index][rightIndex] == 1
      )
        iterationConnections.push([row_index, rightIndex]);

      //up traverse
      let upIndex = row_index - 1;
      if (
        upIndex >= 0 &&
        !this.alreadyTraversed([upIndex, col_index]) &&
        this.grid[upIndex][col_index] == 1
      )
        iterationConnections.push([upIndex, col_index]);

      //down traverse
      let downIndex = row_index + 1;
      if (
        downIndex < this.grid.length &&
        !this.alreadyTraversed([downIndex, col_index]) &&
        this.grid[downIndex][col_index] == 1
      )
        iterationConnections.push([downIndex, col_index]);

      this.connections = this.connections.concat(iterationConnections);

      iterationConnections.forEach((block) =>
        this.recursiveTraverse(block[0], block[1])
      );
    },
    alreadyTraversed(block) {
      return this.connections.some((ele) => {
        return JSON.stringify(ele) === JSON.stringify(block);
      });
    },
    onBlockHover(row_index, col_index) {
      if (this.grid[row_index][col_index] == 0) return;

      this.connections = [[row_index, col_index]];
      this.hovering = true;

      this.getConnections(row_index, col_index);

      setTimeout(() => {
        this.hovering = false;
      }, 2000);
    },
    onBlockClick(row_index, col_index) {
      this.hovering = false;
      this.selectedBlock = [row_index, col_index];
    },
    suffleGrid() {
      this.selectedBlock = [];
      this.grid = [];
      for (let row = 0; row < this.gridWidth; row++) {
        this.grid.push([]);
        for (let col = 0; col < this.gridWidth; col++) {
          this.grid[row][col] = Math.round(Math.random());
        }
      }
    },
  },
};
</script>
<style>
.block {
  display: inline-block;
  width: 100px;
  height: 100px;
  border: 1px solid grey;
  cursor: pointer;
  position: relative;
  border-radius: 10px;
  margin: 0px 4px;
  transition: all 0.3s;
}
.block--colored {
  background-color: var(--color);
  border-color: var(--color);
}

.block--revealConnections {
  background-color: var(--color-hover);
  border-color: var(--color-hover);
}
.center-vertical {
  top: 50%;
  height: 100;
  position: absolute;
  left: 50%;
  width: 50%;
  margin: -10% 0 0 -20%;
}
</style>
