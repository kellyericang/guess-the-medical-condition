<script setup>
import GameBoard from './components/guessTheCondition/GameBoard.vue'
import ddxGameBoard from './components/ddx/ddxGameBoard.vue'
import { ref } from 'vue'

const gameStarted = ref(false)
const ddxGameStarted = ref(false)
</script>

<template>
  <main>
    <div v-if="!gameStarted && !ddxGameStarted" class="start-screen">
      <h1>Diagnosis Dash</h1>
      <div class="rules">Guess the medical condition based on the symptoms shown.</div>
      <button @click="gameStarted = true">START GAME!</button>
      <h1>ddx</h1>
      <div class="rules">
        Given the primary complaint, fill in the blanks for all of the differential diagnoses.
      </div>
      <button @click="ddxGameStarted = true">START GAME!</button>
    </div>
    <GameBoard v-if="gameStarted" @back="gameStarted = false" />
    <ddxGameBoard v-if="ddxGameStarted" @back="ddxGameStarted = false" />
  </main>
</template>

<style scoped>
main {
  display: flex;
  flex-direction: column;
  overflow-y: hidden;
}
.start-screen {
  padding: 48px;
  text-align: center;
}
.rules {
  padding: 24px;
}
</style>
