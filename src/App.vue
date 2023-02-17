<template>
  <div>
    <label>Darkmode: </label>
    <input type="checkbox" v-model="darkmode" :true-value="true" :false-value="false">
    <SpellGrapher />
  </div>
</template>

<script>
import SpellGrapher from "@/components/SpellGrapher.vue";
export default {
  components: {SpellGrapher},
  data() {
    return {
      darkmode: true
    }
  },
  updated() {
    if (this.darkmode) {
      document.body.classList.add('darkmode')
    } else {
      document.body.classList.remove('darkmode')
    }
  }
}
</script>

<style scoped>
</style>
