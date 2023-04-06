<template>

</template>

<script>
export default {
  name: "SpellComputer",
  methods: {
    computePoints(nbElements) {
      let nbPoints = nbElements * 2 + 1;
      const incr = 360/nbPoints;

      let points = [];
      let angle = -0.5*incr+90;

      for (let i = 0; i < nbPoints; i++) {
        let x = 250 + 200 * Math.cos(-angle*Math.PI/180);
        let y = 250 + 200 * Math.sin(-angle*Math.PI/180);

        points.push({'x': x, 'y': y});
        angle += incr;
      }

      return points;
    },
    computePairs(nbElements, values) {
      const nbPoints = nbElements * 2 + 1;
      const encoder = 1 << (nbPoints - 1);
      let element = 1;

      let pairs = [];
      values.map((value) => {
        value += encoder;
        for (let i = 1; i <= 32; i++) {
          let a = value << 32 - i;
          let b = a >> 31;
          if (b) {
            pairs.push([
              this.modulo(i - element, nbPoints),
              this.modulo((i + element), nbPoints) - element
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