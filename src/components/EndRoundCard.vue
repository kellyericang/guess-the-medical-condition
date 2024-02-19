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
    <div class="result">{{ correct ? "Correct!" : "Incorrect" }}</div>
    <div class="condition">
      The condition was {{ rightAnswer }}
    </div>
    <ol>
      Your Guesses
      <li v-for="guessMade in guessesMade" :key="guessMade">
        {{ guessMade }}
      </li>
    </ol>
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
}
.result {
  font-weight: 600;
  font-size: 4rem;
  color: black;
}
.condition {
  font-weight: 600;
  font-size: 2rem;
  color: black;
}
ol {
  font-size: 1rem;
}
</style>
