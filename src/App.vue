<script setup>
import masterList from "./components/masterList.json"
import ScoreBoard from "./components/ScoreBoard.vue"
import SymptomList from "./components/SymptomList.vue"
import Lives from "./components/Lives.vue"
import { ref, onMounted } from "vue"

const masterConditionList = ref(masterList.conditions);
const currentCondition = ref(null);
const livesLost = ref(0);
const score = ref({ correct: 0, incorrect: 0 });
const gameStarted = ref(false);
const guess = ref("");

function skip() {
  livesLost.value++;
  if (livesLost.value === 4) {
    livesLost.value = 0;
    wrongAnswer();
  }
}

function rightAnswer() {
  score.value.correct++;
}

function wrongAnswer() {
  score.value.incorrect++;
}
function getMedicalCondition() {
  const rng = Math.floor(Math.random() * masterConditionList.value.length);
  currentCondition.value = masterConditionList.value[rng];
}
function makeGuess() {
  if (guess.value === currentCondition.value.name) {
    // correct guess
    rightAnswer();
    reset();
  } else if (livesLost.value === 2) {
    // wrong guess and lose
    wrongAnswer();
    reset();
  } else {
    // wrong guess
    livesLost.value++;
    guess.value = "";
  }
}

function reset() {
  getMedicalCondition();
  livesLost.value = 0;
}

onMounted(() => {
  getMedicalCondition();
})
</script>

<template>
  <main>
    <div v-if="!gameStarted">
      <div class="rules">
        guess the medical condition based on the symptoms shown
      </div>
      <button @click="gameStarted = true">START GAME</button>
    </div>
    <div v-else> 
      <button @click="gameStarted = false">BACK</button>
      <ScoreBoard :score-correct="score.correct" :score-incorrect="score.incorrect" />
      <SymptomList :symptoms="currentCondition.symptoms" :guess-number="livesLost" />
      <Lives :lives-lost="livesLost"/>
      <button @click="skip">
        SKIP
      </button>
      <input type="text" v-model="guess" @keypress.enter="makeGuess"/>
      <button @click="makeGuess">GUESS</button>
    </div>
  </main>
</template>

<style scoped>
main {
  display: flex;
  flex-direction: column;
}
</style>
