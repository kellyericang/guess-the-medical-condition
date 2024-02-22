<script setup>
import GameBoard from "./components/guessTheCondition/GameBoard.vue"
import { ref } from "vue"

const gameStarted = ref(false);
</script>

<template>
  <main>
    <div v-if="!gameStarted" class="start-screen">
      <h1>Diagnosis Dash</h1>
      <div class="rules">
        Guess the medical condition based on the symptoms shown.
      </div>
      <button @click="gameStarted = true">START GAME!</button>
    </div>
    <GameBoard v-else @back="gameStarted = false"/>
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
