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
          :fill="colour"
        />
      </g>
    </svg>
  </div>
</template>

<script>
function hexagon(width, height) {
  const delayScale = 1;

  const gridWidth = Math.floor(width / 3) + 1;
  const gridHeight = Math.floor((4 * height) / 3) + 1;

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

  const xfunc = function (i, gridWidth) {
    return (i % gridWidth) * 3 + (Math.floor(i / gridWidth) % 2) * 1.5;
  };

  const yfunc = function (i, gridwidth) {
    return Math.floor(i / gridWidth) * rt3o2;
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
}

export default {
  props: {
    width: { type: Number, default: 20 },
    height: { type: Number, default: 20 },
    colour: { type: String, default: "#ffd1dc" },
    shapeFn: { type: Function, default: hexagon },
  },
  data: function (props) {
    return {
      scale: "",
    };
  },
  computed: {
    viewBox() {
      return `0 0 ${this.width} ${this.height}`;
    },
    shapes() {
      const { shapeFn, width, height } = this;

      const {
        gridWidth,
        gridHeight,
        points,
        xfunc,
        yfunc,
        translate,
        delay,
      } = shapeFn(width, height);

      const amount = gridWidth * gridHeight;

      return [...Array(amount)].map((value, index) => {
        const x = xfunc(index, gridWidth);
        const y = yfunc(index, gridWidth);

        return {
          points: points,
          translate: translate(x, y),
          delay: delay(x, y),
        };
      });
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
  transition: transform 1s ease-in-out;
}
</style>
