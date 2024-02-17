<script setup>
import { ref } from "vue"

defineProps({
  symptoms: {
    type: Array,
    required: false,
    default: [],
  }
})

const hideSymptoms = ref(3);
</script>

<template>
  <div class="symptom-list">
    Symptoms
    <ul class="list">
      <li v-for="symptom in symptoms" :key="symptom">
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
</style>
