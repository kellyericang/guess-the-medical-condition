<script setup>
import masterList from "../masterList.json"
import { ref, watch } from "vue"

const masterConditionList = ref(masterList.primary_complaints);
const currentPrimaryComplaint = ref({ name: "", systems: [] });
const currentConditionIndex = ref(-1);
const livesLost = ref(0);
let randomOrder = ref([]);

function generateSystemsOrder() {
  console.log(currentPrimaryComplaint.value.systems.length);
  const numArray = [];
  for (let i = 0; i < currentPrimaryComplaint.value.systems.length; i++) {
    numArray.push(i);
  }
  for (let i = numArray.length - 1; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1));
    [numArray[i], numArray[j]] = [numArray[j], numArray[i]];
  }
  randomOrder.value = numArray;
  console.log(randomOrder.value);
}

watch(currentPrimaryComplaint, () => {
  if (currentPrimaryComplaint.value.name !== "") {
    generateSystemsOrder();
  }
})

</script>

<template>
  <div class="ddx-game-board"> 
    <div v-if="currentPrimaryComplaint.name === ''">
      <h1>Pick a Condition</h1>
      <div v-for="symptom in masterConditionList">
        <button @click="currentPrimaryComplaint = symptom">{{ symptom.name }}</button>
      </div>
    </div>
    <div v-else>
      <h1>Complete the ddx for: {{ currentPrimaryComplaint.name }}</h1>
      <div v-for="num in randomOrder">
        {{ currentPrimaryComplaint.systems[randomOrder[num]].name }}
        <div v-for="condition in currentPrimaryComplaint.systems[randomOrder[num]].conditions">
          {{ condition }}
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
</style>
