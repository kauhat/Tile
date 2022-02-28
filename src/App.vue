<template>
  <Tile :width="this.width" :height="this.height" :shapeFn="square" />
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
      width: 10,
      height: 10,
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

        const xfunc = function (i, gridWidth) {
          return i % gridWidth;
        };

        const yfunc = function (i, gridwidth) {
          return Math.floor(i / gridWidth);
        };

        const translate = (x, y) => `translate(${x},${y})`;

        const delay = (x, y) => {
          return (
            "transition-delay: " +
            (delayScale * (x + y)) / (gridWidth + gridHeight) +
            "s"
          );
        };
        return {
          gridWidth: gridWidth,
          gridHeight: gridHeight,
          points: points,
          xfunc: xfunc,
          yfunc: yfunc,
          translate,
          delay,
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
