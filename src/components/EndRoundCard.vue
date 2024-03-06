<script setup>
defineProps({
  correct: {
    type: Boolean,
    required: true
  },
  rightAnswer: {
    type: String,
    required: true
  },
  guessesMade: {
    type: Array,
    required: true
  }
})
</script>

<template>
  <div class="end-round-screen">
    <div class="result">{{ correct ? 'Correct!' : 'Incorrect' }}</div>
    <div class="condition">The condition was {{ rightAnswer }}</div>
    <div>
      Your Guesses:
      <div v-for="guessMade in guessesMade" :key="guessMade">
        {{ guessMade }}
      </div>
    </div>
    <button @click="$emit('next-round')">NEXT ROUND</button>
  </div>
</template>

<style scoped>
.end-round-screen {
  position: fixed;
  height: 50dvh;
  width: 80vw;
  max-width: 1080px;
  max-height: 500px;
  background: rgb(255 255 255 / 0.6);
  backdrop-filter: blur(20px);
  border-radius: 20px;
  left: 10vw;
  top: 10dvh;
  display: flex;
  flex-direction: column;
  align-items: center;
  color: black;
  justify-content: space-around;
  padding: 48px;
  text-align: center;
}
.result {
  font-weight: 600;
  color: black;
}
.condition {
  font-weight: 600;
  color: black;
}
@media screen and (max-width: 600px) {
  .end-round-screen {
    padding: 12px;
  }
  .result {
    font-size: 2rem;
  }
  .condition {
    font-size: 1.5rem;
  }
}
@media screen and (min-width: 601px) {
  .result {
    font-size: 4rem;
  }
  .condition {
    font-size: 2rem;
  }
}
ol {
  font-size: 1rem;
}
</style>
