<script setup>
import IconHeart from './icons/IconHeart.vue'
import IconHeartBroken from './icons/IconHeartBroken.vue'
defineProps({
  livesLost: {
    type: Number,
    required: true
  }
})
</script>

<template>
  <div class="lives">
    <IconHeart v-if="livesLost < 3" />
    <IconHeart v-if="livesLost < 2" />
    <IconHeart v-if="livesLost < 1" />
    <IconHeartBroken v-if="livesLost > 0" />
    <IconHeartBroken v-if="livesLost > 1" />
    <IconHeartBroken v-if="livesLost > 2" />
  </div>
</template>

<style scoped>
svg {
  height: 50px;
  width: 50px;
}
.lives {
  display: flex;
  justify-content: center;
}
</style>
