<script setup>
import masterList from '../masterList.json'
import { ref, watch } from 'vue'

const masterConditionList = ref(masterList.primary_complaints)
const currentPrimaryComplaint = ref({ name: '', systems: [] })
const currentConditionIndex = ref(-1)
let randomOrder = ref([])
let randomOrderIndex = ref(0)
let answers = ref([])
let guess = ref('')

function generateSystemsOrder() {
  const numArray = []
  for (let i = 0; i < currentPrimaryComplaint.value.systems.length; i++) {
    numArray.push(i)
  }
  for (let i = numArray.length - 1; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1))
    ;[numArray[i], numArray[j]] = [numArray[j], numArray[i]]
  }
  randomOrder.value = numArray
}

function resetAnswers() {
  answers.value = []
  for (const x in currentPrimaryComplaint.value.systems) {
    let system = []
    for (const y in currentPrimaryComplaint.value.systems[x].conditions) {
      system.push(false)
    }
    answers.value.push(system)
  }
}

function makeGuess() {
  const index = currentPrimaryComplaint.value.systems[
    randomOrder.value[randomOrderIndex.value]
  ].conditions.indexOf(guess.value)
  if (index !== -1) {
    answers.value[randomOrder.value[randomOrderIndex.value]][index] = true
    guess.value = ''
  }
}

function showAnswers() {
  for (const x in answers.value[randomOrder.value[randomOrderIndex.value]]) {
    answers.value[randomOrder.value[randomOrderIndex.value]][x] = true
  }
}

watch(currentPrimaryComplaint, () => {
  if (currentPrimaryComplaint.value.name !== '') {
    generateSystemsOrder()
    resetAnswers()
  }
})
</script>

<template>
  <div class="ddx-game-board">
    <div v-if="currentPrimaryComplaint.name === ''">
      <label for="symptom-select">Pick a symptom: </label>
      <select name="symptoms" id="symptom-select" v-model="currentPrimaryComplaint">
        <option v-for="symptom in masterConditionList" :key="symptom" :value="symptom">
          {{ symptom.name }}
        </option>
      </select>
    </div>
    <div v-else>
      <button @click="currentPrimaryComplaint = { name: '', systems: [] }">change topic</button>
      <h1>Complete the ddx for: {{ currentPrimaryComplaint.name }}</h1>
      <h2>{{ currentPrimaryComplaint.systems[randomOrder[randomOrderIndex]].name }}</h2>
      <div class="buttons">
        <button :disabled="randomOrderIndex === 0" @click="randomOrderIndex--">PREVIOUS</button>
        <button :disabled="randomOrderIndex === randomOrder.length - 1" @click="randomOrderIndex++">
          NEXT
        </button>
        <button @click="showAnswers">SHOW ALL ANSWERS</button>
      </div>
      <input v-model="guess" type="text" class="input-guess" placeholder="your guess here" @keyup="makeGuess" />
      <div
        v-for="(condition, index) in currentPrimaryComplaint.systems[randomOrder[randomOrderIndex]]
          .conditions"
        :key="condition"
        class="answer-row"
      >
        {{ answers[randomOrder[randomOrderIndex]][index] === false ? '????????????' : condition }}
        <button :disabled="answers[randomOrder[randomOrderIndex]][index]" @click="answers[randomOrder[randomOrderIndex]][index] = true"> show answer </button>
      </div>
    </div>
  </div>
</template>

<style scoped>
.ddx-game-board {
  padding: 16px;
}
.buttons {
  display: flex;
  width: 100%;
  justify-content: space-between;
  padding: 16px 0px;
}
.answer-row {
  display: flex;
  justify-content: space-between;
  padding-top: 8px;
  padding-bottom: 8px;
  border-bottom: solid 2px rgb(0 0 0 / 0.5);
}
</style>
