<template>
  <section id="log" class="log-container">
    <h2>Battle Log</h2>
    <transition-group name="log" tag="ul" class="log-list">
      <li v-for="(logMessage, index) in logMessages" :key="index" :class="logMessage.actionBy">
        <span class="actor">
          {{ logMessage.actionBy === 'pokemon' ? '🧝 Pokemon' : '🧟 Monster' }}
        </span>
        <span v-if="logMessage.actionType === 'heal'" class="heal">
          heals for <strong>{{ logMessage.actionValue }}</strong>
        </span>
        <span v-else class="damage">
          attacks and deals <strong>{{ logMessage.actionValue }}</strong>
        </span>
      </li>
    </transition-group>
  </section>
</template>

<script setup>
import { defineProps } from 'vue'

const props = defineProps({
  logMessages: Array
})
</script>

<style scoped>
.log-container {
  margin: 1rem auto;
  max-width: 600px;
  background-color: #2a2a2a;
  padding: 1rem;
  border-radius: 10px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.4);
}

.log-container h2 {
  text-align: center;
  margin-bottom: 0.5rem;
  font-weight: 700;
  font-size: 1.2rem;
}

.log-list {
  list-style: none;
  padding: 0;
  margin: 0;
}

.log-list li {
  display: flex;
  gap: 0.5rem;
  margin: 0.25rem 0;
  padding: 0.25rem 0.5rem;
  border-radius: 6px;
  align-items: center;
  font-family: 'Jost', sans-serif;
  font-weight: 500;
}

.pokemon {
  background-color: rgba(77, 166, 255, 0.1);
}

.monster {
  background-color: rgba(255, 71, 71, 0.1);
}

.heal {
  color: #4caf50;
}

.damage {
  color: #ffb74d;
}

.log-enter-active,
.log-leave-active {
  transition: all 0.4s ease;
}

.log-enter-from,
.log-leave-to {
  opacity: 0;
  transform: translateY(-10px);
}
</style>
