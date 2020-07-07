<template>
  <div>
    <div v-for="row in grid" :key="row.id">
      <div
        v-for="(col, index) in row"
        :key="col.id"
        :class="[{ 'block--red': col == 1 }, 'block']"
        @click="getConnections(row, index)"
      ></div>
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
        [1, 1, 0, 1, 1],
        [0, 0, 0, 0, 0],
        [1, 1, 1, 0, 0],
      ],
    };
  },
  methods: {
    getConnections(row, row_index) {
      console.log(row, row_index);
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
}
.block--red {
  background-color: red;
}
</style>
