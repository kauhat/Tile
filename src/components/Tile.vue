<template>
  <div class="tile" @click="reveal" @mouseleave="hide">
    <svg :viewBox="viewBox">
      <image :href="imgPath" height="100%" width="100%" />
      <g>
        <polygon
          class="shape"
          v-for="(shape, index) in shapes"
          :key="index"
          :points="shape.points"
          :transform="shape.translate + this.scale"
          v-bind:style="shape.delay"
          :fill="colour"
          :stroke="colour"
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

  const points = `1,0 0.5,${rt3o2} -0.5,${rt3o2} -1,0 -0.5,${-rt3o2} 0.5,${-rt3o2}`;

  const computeShape = function (i, gridwidth) {
    const x = (i % gridWidth) * 3 + (Math.floor(i / gridWidth) % 2) * 1.5;
    const y = Math.floor(i / gridWidth) * rt3o2;
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
}

export default {
  props: {
    width: { type: Number, default: 20 },
    height: { type: Number, default: 20 },
    colour: { type: String, default: "#ffd1dc" },
    shapeFn: { type: Function, default: hexagon },
    imgPath: { type: String, default: "http://placekitten.com/200/200" },
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

      const { gridWidth, gridHeight, points, computeShape } = shapeFn(
        width,
        height
      );

      const amount = gridWidth * gridHeight;

      return [...Array(amount)].map((value, index) => {
        return {
          points: points,
          ...computeShape(index, gridWidth),
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
  stroke-width: 0.027;
}
</style>
