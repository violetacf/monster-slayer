<template>
  <section class="game-over" :class="winnerClass">
    <h2>Game Over!</h2>
    <h3 v-if="winner === 'pokemon'">You've won!!! 🎉</h3>
    <h3 v-else-if="winner === 'monster'">You've lost!!! 💀</h3>
    <h3 v-else>It's a draw!</h3>
    <button @click="$emit('restart')" class="btn-restart">Start New Game</button>

    <!-- Animaciones -->
    <div v-if="winner === 'pokemon'" class="fireworks-container">
      <span v-for="n in 10" :key="n" class="firework"></span>
    </div>
    <div v-else-if="winner === 'monster'" class="blood-container">
      <span v-for="n in 15" :key="n" class="blood-drop"></span>
    </div>
  </section>
</template>

<script setup>
import { defineProps, computed } from 'vue'

const props = defineProps({
  winner: String
})

const winnerClass = computed(() => {
  if (props.winner === 'pokemon') return 'victory'
  if (props.winner === 'monster') return 'defeat'
  return ''
})
</script>

<style scoped>
.game-over {
  position: relative;
  text-align: center;
  padding: 2rem;
  border-radius: 12px;
  max-width: 500px;
  margin: 1rem auto;
  color: white;
  font-family: 'Jost', sans-serif;
  font-weight: 700;
  box-shadow: 0 6px 15px rgba(0, 0, 0, 0.5);
  overflow: hidden;
}

/* Fondo según resultado */
.victory {
  background: linear-gradient(135deg, #4caf50, #2196f3);
}

.defeat {
  background: linear-gradient(135deg, #f44336, #8b0000);
}

/* Botón destacado */
.btn-restart {
  margin-top: 1rem;
  padding: 0.6rem 1.4rem;
  font-weight: 700;
  border: none;
  border-radius: 8px;
  background: #ff9800;
  color: white;
  cursor: pointer;
  transition: transform 0.2s ease, box-shadow 0.2s ease;
}

.btn-restart:hover {
  transform: scale(1.05);
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.4);
}

/* Fuegos artificiales */
.fireworks-container {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  pointer-events: none;
}

.firework {
  position: absolute;
  top: 50%;
  left: 50%;
  font-size: 2rem;
  content: '✨';
  animation: fireworkAnim 1s ease-in-out infinite;
}

/* Animación random simple para fuego artificial */
@keyframes fireworkAnim {
  0% {
    transform: translate(0, 0) scale(0);
    opacity: 1;
  }

  50% {
    transform: translate(calc(-50px + 100px * var(--i)), calc(-50px + 100px * var(--i))) scale(1);
    opacity: 1;
  }

  100% {
    transform: translate(calc(-100px + 200px * var(--i)), calc(-100px + 200px * var(--i))) scale(0);
    opacity: 0;
  }
}

/* Sangre cayendo */
.blood-container {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  pointer-events: none;
}

.blood-drop {
  position: absolute;
  top: -50px;
  font-size: 2rem;
  left: calc(10% + 80% * var(--i));
  animation: bloodFall linear infinite;
  content: '🩸';
}

@keyframes bloodFall {
  0% {
    transform: translateY(0) rotate(0deg);
    opacity: 1;
  }

  100% {
    transform: translateY(100vh) rotate(360deg);
    opacity: 0;
  }
}
</style>
