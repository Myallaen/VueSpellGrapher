<template>
  <form @change.prevent="updateGraph">
    <select v-model="level"><option v-for="object in Levels" :value="object.value">{{ object.name }}</option></select>
    <select v-model="school"><option v-for="object in Schools" :value="object.value">{{ object.name }}</option></select>
    <select v-model="damage"><option v-for="object in Damages" :value="object.value">{{ object.name }}</option></select>
    <select v-model="target"><option v-for="object in Targets" :value="object.value">{{ object.name }}</option></select>
    <select v-model="range"><option v-for="object in Ranges" :value="object.value">{{ object.name }}</option></select>
    <br/>
    <label>Draw Circle :</label>
    <input type="checkbox" v-model="drawCircle" :true-value="true" :false-value="false">
    <label>Color :</label>
    <input v-model="circleColor">
    <label>Width :</label>
    <input type="number" min="1" max="10" v-model="circleWidth">
    <br/>
    <label>Draw Points :</label>
    <input type="checkbox" v-model="drawPoints" :true-value="true" :false-value="false">
    <label>Color :</label>
    <input v-model="pointsColor">
    <label>Width :</label>
    <input type="number" min="1" max="10" v-model="pointsWidth">
    <br/>
    <label>Draw Lines :</label>
    <input type="checkbox" v-model="drawLines" :true-value="true" :false-value="false">
    <label>Color :</label>
    <input v-model="linesColor">
    <label>Width :</label>
    <input type="number" min="1" max="10" v-model="linesWidth">
  </form>
  <CanvasGrapher />
</template>

<script>
import CanvasGrapher from "@/components/CanvasGrapher.vue";
export default {
  name: 'SpellGrapher',
  components: {CanvasGrapher},
  data() {
    return {
      nbElements: 5,
      level:0,
      school:0,
      damage: 0,
      target: 0,
      range: 0,
      drawCircle: true,
      circleColor: 'white',
      circleWidth: 3,
      drawPoints: false,
      pointsColor: 'white',
      pointsWidth: 5,
      drawLines: true,
      linesColor: 'white',
      linesWidth: 3,
      Levels: [
        {'name':  'Cantrip', 'value': 0}, {'name':  'Level 1', 'value': 1}, {'name':  'Level 2', 'value': 2},
        {'name':  'Level 3', 'value': 3}, {'name':  'Level 4', 'value': 4}, {'name':  'Level 5', 'value': 5},
        {'name':  'Level 6', 'value': 6}, {'name':  'Level 7', 'value': 7}, {'name':  'Level 8', 'value': 8},
        {'name':  'Level 9', 'value': 9}
      ],
      Schools: [
        {'name':  'Abjuration', 'value': 0},
        {'name':  'Conjuration', 'value': 1},
        {'name':  'Divination', 'value': 2},
        {'name':  'Enchantment', 'value': 3},
        {'name':  'Evocation', 'value': 4},
        {'name':  'Illusion', 'value': 5},
        {'name':  'Necromancy', 'value': 6},
        {'name':  'Transmutation', 'value': 7}
      ],
      Damages: [
        {'name':  'None', 'value': 0}, {'name':  'Acid', 'value': 1}, {'name':  'Bludgeoning', 'value': 2},
        {'name':  'Cold', 'value': 3}, {'name':  'Fire', 'value': 4}, {'name':  'Force', 'value': 5},
        {'name':  'Lightning', 'value': 6}, {'name':  'Necrotic', 'value': 7}, {'name':  'Piercing', 'value': 8},
        {'name':  'Poison', 'value': 9}, {'name':  'Psychic', 'value': 10}, {'name':  'Radiant', 'value': 11},
        {'name':  'Slashing', 'value': 12}, {'name':  'Thunder', 'value': 13}
      ],
      Targets: [
        {'name':  'Single Target', 'value': 0}, {'name':  'Multiple Targets', 'value': 1},
        {'name':  'Wall 30ft long 10 ft high', 'value': 2}, {'name':  '15 ft radius Sphere', 'value': 3},
        {'name':  '15 ft radius Cylinder', 'value': 4}, {'name':  '30 ft Cone', 'value': 5},
        {'name':  '60 ft Line', 'value': 6}, {'name':  '15 ft Cube', 'value': 7},
        {'name':  '30 ft Square', 'value': 8}, {'name':  '15 ft Circle', 'value': 9}
      ],
      Ranges: [
        {'name':  'Touch', 'value': 0}, {'name':  '30 ft', 'value': 1}, {'name':  '60 ft', 'value': 2},
        {'name':  '90 ft', 'value': 3}, {'name':  '120 ft', 'value': 4}, {'name':  '300 ft', 'value': 5}
      ]
    }
  },
  methods: {
    updateGraph() {
      CanvasGrapher.methods.update(
          this.nbElements,
          [this.level, this.school, this.damage, this.target, this.range],
          {
            drawCircle: this.drawCircle, circleColor: this.circleColor, circleWidth: this.circleWidth,
            drawPoints: this.drawPoints, pointsColor: this.pointsColor, pointsWidth: this.pointsWidth,
            drawLines: this.drawLines, linesColor: this.linesColor, linesWidth: this.linesWidth,
          }
      );
    }
  },
  mounted() {
    this.updateGraph();
  }
}

</script>

<style scoped>

</style>