<template>

</template>

<script>
export default {
  name: "SpellComputer",
  methods: {
    computePoints(nbElements, baseType) {
      const nbPoints = nbElements * 2 + 1;

      switch (baseType.value) {
        case 'line1':
          return this.computePointsLinear(nbPoints, {'x': -200, 'y': 0}, {'x': 200, 'y': 0});
        case 'line2':
          return this.computePointsLinear(nbPoints, {'x': 0, 'y': -200}, {'x': 0, 'y': 200});
        case 'diag1':
          return this.computePointsLinear(nbPoints, {'x': -200, 'y': 200}, {'x': 200, 'y': -200});
        case 'diag2':
          return this.computePointsLinear(nbPoints, {'x': -200, 'y': -200}, {'x': 200, 'y': 200});
        case 'halfCircle1':
          return this.computePointsPartialCircular(nbPoints, {'x': 0, 'y': 100}, 200, 180, 0);
        case 'halfCircle2':
          return this.computePointsPartialCircular(nbPoints, {'x': 100, 'y': 0}, 200, 180, 90);
        case 'halfCircle3':
          return this.computePointsPartialCircular(nbPoints, {'x': 0, 'y': -100}, 200, 180, 180);
        case 'halfCircle4':
          return this.computePointsPartialCircular(nbPoints, {'x': -100, 'y': 0}, 200, 180, 270);
        case 'quartCircle1':
          return this.computePointsPartialCircular(nbPoints, {'x': -150, 'y': 150}, 300, 90, 0);
        case 'quartCircle2':
          return this.computePointsPartialCircular(nbPoints, {'x': 150, 'y': 150}, 300, 90, 90);
        case 'quartCircle3':
          return this.computePointsPartialCircular(nbPoints, {'x': 150, 'y': -150}, 300, 90, 180);
        case 'quartCircle4':
          return this.computePointsPartialCircular(nbPoints, {'x': -150, 'y': -150}, 300, 90, 270);
        case 'circle':
        default:
          return this.computePointsFullCircular(nbPoints);
      }
    },
    computePointsFullCircular(nbPoints) {
      const incr = 360/nbPoints;

      let points = [];
      let angle = -1.5*incr+90;

      for (let i = 0; i < nbPoints; i++) {
        let x = 200 * Math.cos(-angle*Math.PI/180);
        let y = 200 * Math.sin(-angle*Math.PI/180);

        points.push({'x': x, 'y': y});
        angle += incr;
      }

      return points;
    },
    computePointsPartialCircular(nbPoints, center, radius, range, offset) {
      const incr = range/(nbPoints-1);

      let points = [];
      let angle = offset;

      for (let i = 0; i < nbPoints; i++) {
        let x = center.x + (radius * Math.cos(-angle*Math.PI/180));
        let y = center.y + (radius * Math.sin(-angle*Math.PI/180));

        points.push({'x': x, 'y': y});
        angle += incr;
      }

      return points;
    },
    computePointsLinear(nbPoints, point1, point2) {
      let points = [];
      for (let i = 0; i < nbPoints; i++) {
        let x = point1.x + (i * ((point2.x - point1.x)/nbPoints));
        let y = point1.y + (i * ((point2.y - point1.y)/nbPoints));

        points.push({'x': x, 'y': y});
      }

      return points;
    },

    computePairs(nbElements, values) {
      const nbPoints = nbElements * 2 + 1;
      let element = 1;

      let pairs = [];
      values.map((value) => {
        value = value + 1024;
        for (let i = 1; i <= 32; i++) {
          let a = value << 32 - i;
          let b = a >> 31;
          if (b) {
            pairs.push([
              this.modulo(i - element, nbPoints),
              this.modulo(i, nbPoints)
            ]);
          }
        }
        element++;
      })
      return pairs;

    },
    modulo(a, b) {
      return (a + b) % b;
    }
  }
}
</script>

<style scoped>

</style>