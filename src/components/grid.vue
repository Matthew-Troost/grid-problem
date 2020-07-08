<template>
  <div>
    <div v-for="(row, row_index) in grid" :key="row.id">
      <div
        v-for="(col, col_index) in row"
        :key="col.id"
        :style="[selectedColors, blockHeight]"
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
        @mouseleave="hovering = false"
      >
        <div class="center-vertical">
          {{
            selectedBlock[0] == row_index && selectedBlock[1] == col_index
              ? connectionCount
              : ""
          }}
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    gridWidth: {
      type: Number,
      default: 10,
    },
    blockColor: {
      type: String,
      default: "#7f6ffd",
    },
    blockHoverColor: {
      type: String,
      default: "#ffffff",
    },
  },
  data() {
    return {
      grid: [],
      selectedBlock: [],
      connections: [],
      connectionCount: 0,
      hovering: false,
    };
  },
  computed: {
    selectedColors: function() {
      return {
        "--color": this.blockColor,
        "--color-hover": this.blockHoverColor,
      };
    },
    blockHeight: function() {
      return {
        height: `calc(100vw / ${this.gridWidth * 2} )`,
        width: `calc(100vw / ${this.gridWidth * 2})`,
      };
    },
  },
  created() {
    this.shuffle();
  },
  watch: {
    gridWidth: function() {
      this.shuffle();
    },
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
    },
    onBlockClick(row_index, col_index) {
      if (this.grid[row_index][col_index] == 0) return;

      this.hovering = false;
      this.selectedBlock = [row_index, col_index];
      this.connectionCount = this.connections.length;
    },
    shuffle() {
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
  border: 1px solid grey;
  cursor: pointer;
  position: relative;
  border-radius: 10px;
  margin: 0px 4px;
  transition: all 0.3s;
  max-width: 100px;
  max-height: 100px;
  color: #242453;
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
