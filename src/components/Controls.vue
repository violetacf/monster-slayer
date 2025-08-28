<template>
  <section id="controls" class="controls-container">
    <button 
      @click="$emit('attack')" 
      :disabled="disabled" 
      class="btn attack"
    >
      ATTACK
    </button>
    <button 
      @click="$emit('specialAttack')" 
      :disabled="disabled || !mayUseSpecialAttack" 
      class="btn special"
    >
      SPECIAL ATTACK
    </button>
    <button 
      @click="$emit('heal')" 
      :disabled="disabled" 
      class="btn heal"
    >
      HEAL
    </button>
    <button 
      @click="$emit('surrender')" 
      :disabled="disabled" 
      class="btn surrender"
    >
      SURRENDER
    </button>
    <button 
      @click="$emit('restart')" 
      class="btn restart"
    >
      RESTART
    </button>
  </section>
</template>

<script setup>
import { defineProps } from 'vue'

const props = defineProps({
  disabled: Boolean,
  mayUseSpecialAttack: Boolean
})
</script>

<style scoped>
.controls-container {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 0.5rem;
  margin: 1rem 0;
}

.btn {
  font-family: 'Jost', sans-serif;
  font-weight: 700;
  border: none;
  border-radius: 8px;
  padding: 0.6rem 1.2rem;
  cursor: pointer;
  transition: all 0.2s ease;
  color: white;
  min-width: 120px;
  box-shadow: 0 3px 6px rgba(0,0,0,0.3);
}

.btn:hover:not(:disabled) {
  transform: scale(1.05);
  box-shadow: 0 5px 10px rgba(0,0,0,0.4);
}

.btn:active:not(:disabled) {
  transform: scale(0.98);
}

.btn:disabled {
  background: #888;
  cursor: not-allowed;
  box-shadow: none;
}

.attack {
  background-color: #4caf50;
}

.special {
  background-color: #ff9800;
}

.heal {
  background-color: #2196f3;
}

.surrender {
  background-color: #f44336;
}

.restart {
  background-color: #9c27b0;
}

@media (max-width: 600px) {
  .btn {
    min-width: 100%;
  }
}
</style>
