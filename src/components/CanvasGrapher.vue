<template>
  <canvas id="graph" width="500" height="500"></canvas>
</template>

<script>
import SpellComputer from "@/components/SpellComputer.vue";
export default {
  name: "CanvasGrapher",
  data() {
    return {
      options: []
    }
  },
  methods: {
    update(nbElements, values, options) {
      this.options = options;

      this.ctx = document.getElementById("graph").getContext("2d");
      this.ctx.clearRect(0,0,500,500);
      const points = SpellComputer.methods.computePoints(nbElements);
      const pairs = SpellComputer.methods.computePairs(nbElements, values);

      this.graph( points, pairs);
    },
    setLine(color, width) {
      this.ctx.fillStyle = color;
      this.ctx.strokeStyle = color;
      this.ctx.lineWidth = width;
    },
    graph(points, pairs) {
      this.ctx.lineCap = "round";
      if (this.options.drawCircle) {
        this.setLine(this.options.circleColor ?? 'black', this.options.circleWidth ?? 3)
        this.ctx.beginPath();
        this.ctx.arc(250, 250, 200, 0, 2 * Math.PI);
        this.ctx.stroke();
      }
      if (this.options.drawPoints) {
        this.setLine(this.options.pointsColor ?? 'black', 1)
        points.forEach((point) => {
          this.ctx.beginPath();
          this.ctx.arc(point.x, point.y, this.options.pointsWidth ?? 5, 0, 2 * Math.PI);
          this.ctx.fill();
        })
      }
      if (this.options.drawLines) {
        this.setLine(this.options.linesColor ?? 'black', this.options.linesWidth ?? 3)
        pairs.forEach((pair) => {
          const point1 = points[pair[0]];
          const point2 = points[pair[1]];
          this.ctx.beginPath();
          this.ctx.moveTo(point1.x, point1.y);
          this.ctx.lineTo(point2.x, point2.y)
          this.ctx.stroke();
        })
      }
    }
  }
}
</script>

<style scoped>

</style>0