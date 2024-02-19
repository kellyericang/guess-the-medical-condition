<script setup>
import masterList from "./components/masterList.json"
import EndRoundCard from "./components/EndRoundCard.vue"
import ScoreBoard from "./components/ScoreBoard.vue"
import SymptomList from "./components/SymptomList.vue"
import Lives from "./components/Lives.vue"
import { ref, onMounted } from "vue"

const masterConditionList = ref(masterList.conditions);
const currentCondition = ref(null);
const currentConditionIndex = ref(-1);
const livesLost = ref(0);
const score = ref({ correct: 0, incorrect: 0 });
const gameStarted = ref(false);
const guess = ref("");
const guessesMade = ref([]);
const win = ref(false);

function skip() {
  guessesMade.value.push("skipped");
  livesLost.value++;
  if (livesLost.value === 4) {
    livesLost.value = 0;
    wrongAnswer();
  }
}

function rightAnswer() {
  win.value = true;
  score.value.correct++;
}

function wrongAnswer() {
  score.value.incorrect++;
}
function getMedicalCondition() {
  const rng = Math.floor(Math.random() * masterConditionList.value.length);
  if (rng !== currentConditionIndex) {
    currentConditionIndex.value = rng;
  } else if (rng === masterConditionList.value.length) {
    rng = 0;
    currentConditionIndex.value = rng;
  } else {
    rng++;
    currentConditionIndex.value = rng;
  }
  
  currentCondition.value = masterConditionList.value[rng];
}
function makeGuess() {
  guessesMade.value.push(guess.value);
  if (guess.value === currentCondition.value.name) {
    // correct guess
    rightAnswer();
  } else if (livesLost.value === 2) {
    // wrong guess and lose
    wrongAnswer();
    livesLost.value++;
  } else {
    // wrong guess
    livesLost.value++;
  }
  guess.value = "";
}

function reset() {
  win.value = false;
  getMedicalCondition();
  livesLost.value = 0;
  guess.value = "";
  guessesMade.value = [];
}

onMounted(() => {
  getMedicalCondition();
})
</script>

<template>
  <main>
    <div v-if="!gameStarted">
      <h1>Diagnosis Dash</h1>
      <div class="rules">
        guess the medical condition based on the symptoms shown
      </div>
      <button @click="gameStarted = true">START GAME</button>
    </div>
    <div v-else class="game-board"> 
      <button @click="gameStarted = false">BACK</button>
      <ScoreBoard :score-correct="score.correct" :score-incorrect="score.incorrect" />
      <SymptomList :symptoms="currentCondition.symptoms" :guess-number="livesLost" />
      <Lives :lives-lost="livesLost"/>
      Guesses Made
      <ol>
        <li v-for="guessMade in guessesMade" :key="guessMade">
          {{ guessMade }}
        </li>
      </ol>
      <button @click="skip">
        SKIP
      </button>
      <input type="text" v-model="guess" @keypress.enter="makeGuess"/>
      <button @click="makeGuess">GUESS</button>
      <EndRoundCard
        v-if="win || livesLost === 3"
        :correct="win"
        :right-answer="currentCondition.name"
        :guesses-made="guessesMade"
        @next-round="reset"
      />
    </div>
  </main>
</template>

<style scoped>
main {
  display: flex;
  flex-direction: column;
}
.game-board {
  display: flex;
  flex-direction: column;
}
</style>
