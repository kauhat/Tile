<template>
  <div class="tile" @click="reveal" @mouseleave="hide">
    <svg :viewBox="viewBox">
      <g>
        <polygon
          class="shape"
          v-for="(shape, index) in shapes"
          :key="index"
          :points="shape.points"
          :transform="shape.translate + this.scale"
          v-bind:style="shape.delay"
        />
      </g>
    </svg>
  </div>
</template>

<script>
export default {
  props: {
    width: { type: Number, default: 10 },
    height: { type: Number, default: 10 },
    colour: { type: Number, default: 0xffd1dc },
  },
  data: function (props) {
    const { width, height } = props;

    const gridWidth = Math.floor(width / 3) + 1;
    const gridHeight = Math.floor((4 * height) / 3) + 1;

    const delayScale = 1;

    const rt3o2 = Math.sqrt(3) / 2;

    let points =
      // x1 y1
      "1,0" +
      // x2 y2
      " 0.5," +
      rt3o2 +
      // x3 y3
      " -0.5," +
      rt3o2 +
      // x4 y4
      " -1,0" +
      // x5 y5
      "-0.5,-" +
      rt3o2 +
      // x6 y6
      "0.5,-" +
      rt3o2;

    let shapes = [];

    for (let i = 0; i < gridWidth * gridHeight; i++) {
      const x = (i % gridWidth) * 3 + (Math.floor(i / gridWidth) % 2) * 1.5;
      const y = Math.floor(i / gridWidth) * rt3o2;

      const translate = "translate(" + x + "," + y + ")";

      const delay =
        "transition-delay: " +
        (delayScale * (x + y)) / (gridWidth + gridHeight) +
        "s";

      shapes.push({
        hidden: false,
        points: points,
        translate: translate,
        delay: delay,
      });
    }

    return {
      gridWidth: gridWidth,
      shapes: shapes,
      scale: "",
    };
  },
  computed: {
    viewBox() {
      return `0 0 ${this.width} ${this.height}`;
    },
    fill() {
      return this.colour;
    },
  },
  methods: {
    reveal: function () {
      this.scale = " scale(0)";
    },
    hide: function () {
      this.scale = "";
    },
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
  fill: fill;

  transition: transform 1s ease-in-out;
}
</style>
