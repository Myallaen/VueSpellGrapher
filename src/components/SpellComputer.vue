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
      console.log(encoder);
      let element = 0;

      let pairs = [];
      for (let key = 1; key <= values.length; key++) {
        let value = values[key- 1] + encoder;

        for (let i = 1; i <= 32; i++) {
          let a = value << 32 - i;
          let b = a >> 31;
          if (b) {
            console.log(i, value, key, this.modulo(i, nbPoints), this.modulo(i + key, nbPoints))
            pairs.push([
              this.modulo(i - key, nbPoints),
              this.modulo((i + key), nbPoints) - key
            ]);
          }
        }
        element++;
      }

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