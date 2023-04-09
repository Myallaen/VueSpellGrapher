<template>
  <canvas id="graph" width="500" height="500"></canvas>
</template>

<script>
import SpellComputer from "@/components/SpellComputer.vue";
export default {
  name: "CanvasGrapher",
  data() {
    return {
      options: [],
      ctx: {}
    }
  },
  methods: {
    initialize() {
      this.ctx = document.getElementById("graph").getContext("2d");
      this.ctx.translate(250, 250);
      this.ctx.lineCap = "round";
    },
    update(nbElements, values, options) {
      this.options = options;
      this.ctx.clearRect(-250, -250, 500, 500);
      const points = SpellComputer.methods.computePoints(nbElements, this.options.baseType);
      const pairs = SpellComputer.methods.computePairs(nbElements, values);
      console.log(pairs);

      this.graph( points, pairs);
    },
    setLine(color, width) {
      this.ctx.fillStyle = color;
      this.ctx.strokeStyle = color;
      this.ctx.lineWidth = width;
    },
    graph(points, pairs) {
      if (this.options.baseDraw) {
        this.setLine(this.options.baseColor ?? 'black', this.options.baseWidth ?? 3)
        this.ctx.beginPath();
        this.ctx.arc(0, 0, 200, 0, 2 * Math.PI);
        this.ctx.stroke();
      }
      if (this.options.pointsDraw) {
        this.setLine(this.options.pointsColor ?? 'black', 1)
        points.forEach((point) => {
          this.ctx.beginPath();
          this.ctx.arc(point.x, point.y, this.options.pointsWidth ?? 5, 0, 2 * Math.PI);
          this.ctx.fill();
        })
      }
      if (this.options.linesDraw) {
        pairs.forEach((pair) => {
          const point1 = points[pair[0]];
          const point2 = points[pair[1]];
          const mod = (pair[1] - pair[0] + 11) % 11 - 1;
          this.setLine(this.options.linesColor[mod] ?? 'black', this.options.linesWidth ?? 3);

          switch (this.options.lineType) {
            case 'center':
              this.graphCenterLines(point1, point2);
              break;
            case 'non-center':
              this.graphNonCenterLines(point1, point2, this.options.linesColor[mod]);
              break;
            case 'straight':
            default:
              this.graphStraitLines(point1, point2);
          }
        })
      }
    },
    graphStraitLines(point1, point2) {
      this.ctx.beginPath();
      this.ctx.moveTo(point1.x, point1.y);
      this.ctx.lineTo(point2.x, point2.y)
      this.ctx.stroke();
    },
    graphCenterLines(point1, point2) {
      const a = (point1.x + point2.x) / 2;
      const b = (point1.y + point2.y) / 2;
      const r = Math.sqrt(Math.pow(point1.x - a, 2) + Math.pow(point1.y - b, 2));
      const T0 = Math.atan((point1.y - b)/(point1.x - a));
      const T1 = Math.atan((point2.y - b)/(point2.x - a));

      let condition = false;
      switch (this.options.parametricCondition) {
        default:
        case 'x':
          condition = point1.x <= point2.x;
          break;
        case 'y':
          condition = point1.y <= point2.y;
          break;
        case 'reverse-x':
          condition = point1.x >= point2.x;
          break;
        case 'reverse-y':
          condition = point1.y >= point2.y;
          break;
        case 'force-positive':
          condition = true;
          break;
        case 'force-negative':
          condition = false;
          break;
      }
      this.ctx.beginPath();
      if (condition) {
        this.ctx.arc(a, b, r, T1 + Math.PI, T0);
      } else {
        this.ctx.arc(a, b, r, T0, T1 + Math.PI);
      }
      this.ctx.stroke();
    },
    graphNonCenterLines(point1, point2, color) {
      const x = (point1.x + point2.x) / 2;
      const y = (point1.y + point2.y) / 2;
      const p = this.options.offset;
      const a = x * p;
      const b = y * p;

      const r = Math.sqrt(Math.pow(point1.x - a, 2) + Math.pow(point1.y - b, 2));
      const T0 = Math.atan((point1.y - b)/(point1.x - a));
      const T1 = Math.atan((point2.y - b)/(point2.x - a));

      this.setLine( 'white', 5);

      this.ctx.beginPath();
      this.ctx.arc(a, b, 5, 0, 2 * Math.PI);
      this.ctx.fill();

      this.setLine('blue', this.options.linesWidth ?? 3);
      this.ctx.beginPath();
      this.ctx.arc(a, b, r, T0, T0);
      this.ctx.stroke();
      this.setLine('green', this.options.linesWidth ?? 3);
      this.ctx.beginPath();
      this.ctx.arc(a, b, r, T1 + Math.PI, T1 + Math.PI);
      this.ctx.stroke();

      // this.ctx.beginPath();
      // if (point1.x <= point2.x) {
      //   this.ctx.arc(a, b, r, T1 + Math.PI, T0);
      // } else {
      //   this.ctx.arc(a, b, r, T0, T1 + Math.PI);
      // }
      // this.ctx.stroke();
    },
  }
}
</script>

<style scoped>

</style>0