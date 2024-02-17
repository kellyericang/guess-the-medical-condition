<script setup>
defineProps({
  scoreCorrect: {
    type: Number,
    required: true
  },
  scoreIncorrect: {
    type: Number,
    required: true
  }
})

</script>

<template>
  <div class="scoreboard">
    <div class="score">
      Correct: {{ scoreCorrect }}
    </div>
    <div class="score">
      Incorrect: {{ scoreIncorrect }}
    </div>
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
