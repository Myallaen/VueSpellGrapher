<template>
  <canvas id="graph" width="500" height="500"></canvas>
</template>

<script>
import SpellComputer from "@/components/SpellComputer.vue";
export default {
  name: "CanvasGrapher",
  methods: {
    update(nbElements, values) {
      const ctx = document.getElementById("graph").getContext("2d");
      ctx.clearRect(0,0,500,500);
      const points = SpellComputer.methods.computePoints(nbElements);
      const pairs = SpellComputer.methods.computePairs(nbElements, values);
      console.log(points);
      console.log(pairs);
      this.graph(ctx, 'white', 5, points, pairs);
      this.graph(ctx, 'black', 3, points, pairs);
    },
    graph(ctx, color, lineWidth, points, pairs) {
      ctx.lineWidth = lineWidth;
      ctx.fillStyle = color;
      ctx.strokeStyle = color;
      points.forEach((point) => {
        ctx.beginPath();
        ctx.arc(point.x, point.y, 3, 0, 2 * Math.PI);
        ctx.fill();
      })
      pairs.forEach((pair) => {
        const point1 = points[pair[0]];
        const point2 = points[pair[1]];
        ctx.beginPath();
        ctx.moveTo(point1.x, point1.y);
        ctx.lineTo(point2.x, point2.y)
        ctx.stroke();
      })
    }
  }
}
</script>

<style scoped>

</style>