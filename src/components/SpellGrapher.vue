<template>
  <form @change.prevent="updateGraph">
    <select v-model="level"><option v-for="object in Levels" :value="object.value">{{ object.name }}</option></select>
    <select v-model="school"><option v-for="object in Schools" :value="object.value">{{ object.name }}</option></select>
    <select v-model="damage"><option v-for="object in Damages" :value="object.value">{{ object.name }}</option></select>
    <select v-model="area"><option v-for="object in Area" :value="object.value">{{ object.name }}</option></select>
    <select v-model="range"><option v-for="object in Ranges" :value="object.value">{{ object.name }}</option></select>
    <br/>
    <input v-model="levelColor">
    <input v-model="schoolColor">
    <input v-model="damageColor">
    <input v-model="areaColor">
    <input v-model="rangeColor">
    <br/>
    <label>Base Draw :</label>
    <input type="checkbox" v-model="baseDraw" :true-value="true" :false-value="false">
    <label>Type :</label>
    <select v-model="baseType"><option v-for="(object, i) in baseTypes" :value="i">{{ object.name }}</option></select>
    <label>Color :</label>
    <input v-model="baseColor">
    <label>Width :</label>
    <input type="number" min="1" max="10" v-model="baseWidth">
    <br/>
    <label>Points Draw :</label>
    <input type="checkbox" v-model="pointsDraw" :true-value="true" :false-value="false">
    <label>Color :</label>
    <input v-model="pointsColor">
    <label>Width :</label>
    <input type="number" min="1" max="10" v-model="pointsWidth">
    <br/>
    <label>Lines Type : </label>
    <select v-model="lineType"><option v-for="object in lineTypes" :value="object.value">{{ object.name }}</option></select>
    <select v-if="lineType !== 'straight'" v-model="parametricCondition"><option v-for="object in parametricConditions" :value="object.value">{{ object.name }}</option></select>
    <input v-if="lineType === 'non-center'" type="number" min="0" max="2" step="0.1" v-model="nonCenterOffset">
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
      i: 0,
      nbElements: 5,
      level:0,
      school:0,
      damage: 0,
      area: 0,
      range: 0,
      levelColor: 'white',
      schoolColor: 'blue',
      damageColor: 'green',
      areaColor: 'red',
      rangeColor: 'orange',
      baseDraw: false,
      baseType: 4,
      baseColor: 'white',
      baseWidth: 3,
      pointsDraw: true,
      pointsColor: 'white',
      pointsWidth: 5,
      lineType: 'straight',
      nonCenterOffset: 0.1,
      linesWidth: 3,
      parametricCondition: 'x',
      baseTypes: [
        {'name': 'Horizontal', 'value': 'line1', 'parametricCondition': 'force-positive'},
        {'name': 'Vertical', 'value': 'line2', 'parametricCondition': 'x'},
        {'name': 'Diagonal 1', 'value': 'diag1', 'parametricCondition': 'x'},
        {'name': 'Diagonal 2', 'value': 'diag2', 'parametricCondition': 'x'},
        {'name': 'Circle', 'value': 'circle', 'parametricCondition': 'x'},
        {'name': 'Half Circle 1', 'value': 'halfCircle1', 'parametricCondition': 'x'},
        {'name': 'Half Circle 2', 'value': 'halfCircle2', 'parametricCondition': 'x'},
        {'name': 'Half Circle 3', 'value': 'halfCircle3', 'parametricCondition': 'x'},
        {'name': 'Half Circle 4', 'value': 'halfCircle4', 'parametricCondition': 'x'},
        {'name': 'Quarter Circle 1', 'value': 'quartCircle1', 'parametricCondition': 'x'},
        {'name': 'Quarter Circle 2', 'value': 'quartCircle2', 'parametricCondition': 'x'},
        {'name': 'Quarter Circle 3', 'value': 'quartCircle3', 'parametricCondition': 'x'},
        {'name': 'Quarter Circle 4', 'value': 'quartCircle4', 'parametricCondition': 'x'},
      ],
      lineTypes: [
        {'name': 'Straight', 'value': 'straight'}, {'name': 'Centered Circle', 'value': 'center'},
        {'name': 'Non-Centered Circle', 'value': 'non-center'}
      ],
      parametricConditions: [
        {'name': 'x-axis', 'value': 'x'}, {'name': 'reverse x-axis', 'value': 'reverse-x'},
        {'name': 'y-axis', 'value': 'y'}, {'name': 'reverse y-axis', 'value': 'reverse-y'},
        {'name': 'positive', 'value': 'force-positive'}, {'name': 'negative', 'value': 'force-negative'}
      ],
      Levels: [
        {'name': 'Cantrip', 'value': 0}, {'name': 'Level 1', 'value': 1}, {'name': 'Level 2', 'value': 2},
        {'name': 'Level 3', 'value': 3}, {'name': 'Level 4', 'value': 4}, {'name': 'Level 5', 'value': 5},
        {'name': 'Level 6', 'value': 6}, {'name': 'Level 7', 'value': 7}, {'name': 'Level 8', 'value': 8},
        {'name': 'Level 9', 'value': 9}, {'name': 'DEBUG', 'value': 1023}
      ],
      Schools: [
        {'name':  'Abjuration', 'value': 0}, {'name':  'Conjuration', 'value': 1}, {'name':  'Divination', 'value': 2},
        {'name':  'Enchantment', 'value': 3}, {'name':  'Evocation', 'value': 4}, {'name':  'Illusion', 'value': 5},
        {'name':  'Necromancy', 'value': 6}, {'name':  'Transmutation', 'value': 7}, {'name': 'DEBUG', 'value': 1023}
      ],
      Damages: [
        {'name':  'Acid', 'value': 0}, {'name':  'Bludgeoning', 'value': 1}, {'name':  'Cold', 'value': 2},
        {'name':  'Damage', 'value': 3}, {'name':  'Extra', 'value': 4}, {'name':  'Fire', 'value': 5},
        {'name':  'Force', 'value': 6}, {'name':  'Lightning', 'value': 7}, {'name':  'Necrotic', 'value': 8},
        {'name':  'Non-magical', 'value': 9}, {'name':  'Piercing', 'value': 10}, {'name':  'Poison', 'value': 11},
        {'name':  'Psychic', 'value': 12}, {'name':  'Radiant', 'value': 13}, {'name':  'Slashing', 'value': 14},
        {'name':  'Thunder', 'value': 15}, {'name': 'DEBUG', 'value': 1023}
      ],
      Area: [
        {'name':  'Cube', 'value': 0},
        {'name':  'Line', 'value': 1},
        {'name':  'Multiple-Targets', 'value': 2},
        {'name':  'Multiple-Targets/Sphere', 'value': 3},
        {'name':  'Cone', 'value': 4},
        {'name':  'Cone/Sphere', 'value': 5},
        {'name':  'Square', 'value': 6},
        {'name':  'Circle', 'value': 7},
        {'name':  'Sphere', 'value': 8},
        {'name':  'Sphere/Cylinder', 'value': 9},
        {'name':  'Single-Target', 'value': 10},
        {'name':  'Single-Target/Cube', 'value': 11},
        {'name':  'Single-Target/Multiple-Targets', 'value': 12},
        {'name':  'Single-Target/Cone', 'value': 13},
        {'name':  'Single-Target/Sphere', 'value': 14},
        {'name':  'Single-Target/Wall', 'value': 15},
        {'name':  'Wall', 'value': 16},
        {'name':  'Cylinder', 'value': 17},
        {'name':  'None', 'value': 18},
        {'name': 'DEBUG', 'value': 1023}
      ],
      Ranges: [
        {'name':  '10-feet radius', 'value': 0}, {'name':  '100-feet line', 'value': 1},
        {'name':  '15-feet cone', 'value': 2}, {'name':  '15-feet cube', 'value': 3},
        {'name':  '15-feet radius', 'value': 4}, {'name':  '30-feet cone', 'value': 5},
        {'name':  '30-feet line', 'value': 6}, {'name':  '30-feet radius', 'value': 7},
        {'name':  '5-feet radius', 'value': 8}, {'name':  '60-feet cone', 'value': 9},
        {'name':  '60-feet line', 'value': 10}, {'name':  'Point (1 miles)', 'value': 11},
        {'name':  'Point (10 feet)', 'value': 12}, {'name':  'Point (1000 feet)', 'value': 13},
        {'name':  'Point (120 feet)', 'value': 14}, {'name':  'Point (150 feet)', 'value': 15},
        {'name':  'Point (30 feet)', 'value': 16}, {'name':  'Point (300 feet)', 'value': 17},
        {'name':  'Point (5 feet)', 'value': 18}, {'name':  'Point (500 feet)', 'value': 19},
        {'name':  'Point (60 feet)', 'value': 20}, {'name':  'Point (90 feet)', 'value': 21},
        {'name':  'Self', 'value': 22}, {'name':  'Sight', 'value': 23}, {'name':  'Special', 'value': 24},
        {'name':  'Touch', 'value': 25}, {'name': 'DEBUG', 'value': 1023}
      ]
    }
  },
  methods: {
    updateGraph() {
      CanvasGrapher.methods.update(
          this.nbElements,
          [this.level, this.school, this.damage, this.area, this.range],
          {
            baseDraw: this.baseDraw, baseType: this.baseTypes[this.baseType], baseColor: this.baseColor, baseWidth: this.baseWidth,
            pointsDraw: this.pointsDraw, pointsColor: this.pointsColor, pointsWidth: this.pointsWidth,
            linesDraw: true, linesColor: [this.levelColor, this.schoolColor, this.damageColor, this.areaColor, this.rangeColor],
            linesWidth: this.linesWidth, lineType: this.lineType, offset: this.nonCenterOffset, parametricCondition: this.parametricCondition
          }
      );
    }
  },
  mounted() {
    CanvasGrapher.methods.initialize()
    this.updateGraph();
  }
}

</script>

<style scoped>

</style>