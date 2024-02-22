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
    <h1>Symptoms</h1>
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
.list {
  @media screen and (max-width: 600px) {
    max-height: 50vh;
    overflow-y: auto;
  }
}
.hide-symptom {
  filter: blur(4px);
}
</style>
