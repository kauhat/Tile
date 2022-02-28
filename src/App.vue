<template>
  <!-- :width="this.width" :height="this.height" :shapeFn="square" -->
  <Tile />
</template>

<script>
import TileVue from "./components/Tile.vue";

export default {
  name: "App",
  components: {
    Tile: TileVue,
  },
  data: function () {
    return {
      width: 20,
      height: 20,
      square: function (width, height) {
        const delayScale = 1;

        const gridWidth = width;
        const gridHeight = height;

        let points =
          // x1 y1
          "0,0" +
          // x2 y2
          " 1,0" +
          // x3 y3
          " 1,1" +
          // x4 y4
          " 0,1";

        const computeShape = function (i, gridwidth) {
          const x = i % gridWidth;
          const y = Math.floor(i / gridWidth);
          const translate = `translate(${x},${y})`;
          const delay =
            "transition-delay: " +
            (delayScale * (x + y)) / (gridWidth + gridHeight) +
            "s";
          return { translate: translate, delay: delay };
        };

        return {
          gridWidth: gridWidth,
          gridHeight: gridHeight,
          points: points,
          computeShape: computeShape,
        };
      },
    };
  },
};
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
