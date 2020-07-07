<template>
  <div>
    <button @click="suffleGrid">Shuffle Grid</button>
    <div v-for="(row, row_index) in grid" :key="row.id">
      <div
        v-for="(col, col_index) in row"
        :key="col.id"
        :class="[{ 'block--red': col == 1 }, 'block']"
        @click="col == 1 ? getConnections(row_index, col_index) : null"
      >
        {{
          selectedBlock[0] == row_index && selectedBlock[1] == col_index
            ? connections.length
            : ""
        }}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      grid: [
        [0, 0, 0, 0, 1],
        [1, 1, 0, 0, 0],
        [1, 0, 1, 1, 1],
        [0, 0, 0, 0, 0],
        [1, 1, 1, 1, 0],
      ],
      selectedBlock: [],
      connections: [],
    };
  },
  methods: {
    getConnections(row_index, col_index) {
      this.selectedBlock = [row_index, col_index];
      this.connections = [this.selectedBlock];
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
    suffleGrid() {
      this.selectedBlock = [];
      this.grid = [];
      for (let row = 0; row < 5; row++) {
        this.grid.push([]);
        for (let col = 0; col < 5; col++) {
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
}
.block--red {
  background-color: red;
}
</style>
