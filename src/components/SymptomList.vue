<script setup>
import { ref } from "vue"

defineProps({
  symptoms: {
    type: Array,
    required: true,
  },
  guessNumber: {
    type: Number,
    required: true
  }
})


</script>

<template>
  <div class="symptom-list">
    Symptoms
    <ul class="list">
      <li
        v-for="(symptom, index) in symptoms"
        :key="symptom"
        :class="index % 3 <= guessNumber ? '' : 'hide-symptom'"
      >
        {{ symptom }}
      </li>
    </ul>
  </div>
</template>

<style scoped>
.scoreboard {
  color: --color-text;
  display: flex;
}
.score {
  flex-grow: 1;
  text-align: center;
}
.hide-symptom {
  display: none;
}
</style>
