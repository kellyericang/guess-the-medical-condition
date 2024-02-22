<script setup>
import masterList from "../masterList.json"
import EndRoundCard from "../EndRoundCard.vue"
import ScoreBoard from "../ScoreBoard.vue"
import SymptomList from "./SymptomList.vue"
import Lives from "../Lives.vue"
import { ref, onMounted } from "vue"

const masterConditionList = ref(masterList.conditions);
const currentCondition = ref({ name: "", symptoms: [] });
const currentConditionIndex = ref(-1);
const livesLost = ref(0);
const score = ref({ correct: 0, incorrect: 0 });
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
  if (guess.value === currentCondition.value.name || guess.value === currentCondition.value.also_accept) {
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
  <div class="game-board"> 
    <button class="back-button" @click="$emit('back')">BACK</button>
    <ScoreBoard :score-correct="score.correct" :score-incorrect="score.incorrect" />
    <SymptomList :symptoms="currentCondition.symptoms" :guess-number="livesLost" />
    <div class="right-side">
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
      <div class="guess">
        <input v-model="guess" type="text" class="input-guess" @keypress.enter="makeGuess"/>
        <button @click="makeGuess">GUESS</button>
      </div>
      <EndRoundCard
        v-if="win || livesLost === 3"
        :correct="win"
        :right-answer="currentCondition.name"
        :guesses-made="guessesMade"
        @next-round="reset"
      />
    </div>
  </div>
</template>

<style scoped>
.game-board {
  display: grid;
  grid-template-columns: repeat(2, 50%);
  padding: 72px 24px 40dvh 24px;
  width: 100%;
}
.back-button {
  position: absolute;
  top: 12px;
  left: 24px;
}
.scoreboard {
  position: absolute;
  top: 36px;
  left: 0px;
  width: 100%;
}
.right-side {
  display: flex;
  flex-direction: column;
  height: 50dvh;
  justify-items: space-around;
}
.input-guess {
  max-width: 50%;
}
</style>
