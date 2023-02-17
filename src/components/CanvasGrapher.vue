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
    setColor(color) {
      this.ctx.fillStyle = color;
      this.ctx.strokeStyle = color;
    },
    graph(points, pairs) {
      this.ctx.lineWidth = this.options.lineWidth ?? 3;

      if (this.options.drawCircle) {
        this.setColor(this.options.circleColor ?? 'black')
        this.ctx.beginPath();
        this.ctx.arc(250, 250, 200, 0, 2 * Math.PI);
        this.ctx.stroke();
      }
      if (this.options.drawPoints) {
        this.setColor(this.options.pointsColor ?? 'black')
        points.forEach((point) => {
          this.ctx.beginPath();
          this.ctx.arc(point.x, point.y, 3, 0, 2 * Math.PI);
          this.ctx.fill();
        })
      }
      if (this.options.drawLines) {
        this.setColor(this.options.linesColor ?? 'black')
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