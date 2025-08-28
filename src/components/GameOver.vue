<template>
  <section class="container" id="game-over">
    <h2>Game Over!</h2>
    <h3 v-if="winner === 'player'">You've won!!!</h3>
    <h3 v-else-if="winner === 'monster'">You've lost!!!</h3>
    <h3 v-else>It's a draw!</h3>
    <button @click="$emit('restart')">Start New Game</button>
  </section>
</template>

<script setup>
import { defineProps } from "vue";

const props = defineProps({
  winner: String,
});
</script>

<style scoped>
#game-over {
  text-align: center;
  background: #222;
  margin: 1rem auto;
  padding: 1rem;
  max-width: 40rem;
  border-radius: 6px;
}
</style>
