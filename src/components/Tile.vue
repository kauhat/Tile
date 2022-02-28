<template>
  <div class="tile">
    <svg viewBox="0 0 10 10">
      <g>
        <polygon
          class="shape"
          :class="{ 'shape--hidden': shape.hidden }"
          v-for="(shape, index) in shapes"
          :key="index"
          :x="shape.x"
          :y="shape.y"
          :points="shape.points"
          :transform="shape.translate"
          width="1"
          height="this.hexHeight"
        />
      </g>
    </svg>
  </div>
</template>

<script>
export default {
  // props: {
  //   shapeScale: { type: Number },
  // },
  data: function () {
    const width = 10;
    const height = 10;

    const rt3 = Math.sqrt(3);

    let points =
      // x1 y1
      "1,0" +
      // x2 y2
      " 0.5," +
      rt3 / 2 +
      // x3 y3
      " -0.5," +
      rt3 / 2 +
      // x4 y4
      " -1,0" +
      // x5 y5
      "-0.5,-" +
      rt3 / 2 +
      // x6 y6
      "0.5,-" +
      rt3 / 2;

    let shapes = [];

    for (let i = 0; i < width * height; i++) {
      const x = (i % width) * 3 + (Math.floor(i / width) % 2) * 1.5;
      const y = (Math.floor(i / width) * rt3) / 2;

      const translate = "translate(" + x + "," + y + ")";

      shapes.push({
        hidden: Math.random() < 0.3,
        points: points,
        x: x,
        y: y,
        translate: translate,
      });
    }

    console.log(shapes);

    return {
      // points: "1,0 0.5,0.866 -0.5,0.866 -1,0 -0.5,-0.866 0.5,-0.866",
      width: width,
      shapes: shapes,
      height: rt3 / 2,
    };
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.tile svg {
  flex: 1;
  width: 100%;
  height: 100%;
}
.shape {
  fill: palevioletred;
  opacity: 0.5;
}
.shape--hidden {
  opacity: 1;
}
</style>
