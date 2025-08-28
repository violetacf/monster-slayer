<template>
  <section id="controls" class="container">
    <button @click="$emit('attack')" :disabled="disabled">ATTACK</button>
    <button
      @click="$emit('specialAttack')"
      :disabled="disabled || !mayUseSpecialAttack"
    >
      SPECIAL ATTACK
    </button>
    <button @click="$emit('heal')" :disabled="disabled">HEAL</button>
    <button @click="$emit('surrender')" :disabled="disabled">SURRENDER</button>
    <button @click="$emit('restart')">RESTART</button>
  </section>
</template>

<script setup>
import { defineProps } from "vue";

const props = defineProps({
  disabled: Boolean,
  mayUseSpecialAttack: Boolean,
});
</script>

<style scoped>
button {
  font: inherit;
  border: none;
  background: #ff4747;
  color: white;
  padding: 0.5rem 1rem;
  margin: 0.25rem;
  border-radius: 4px;
  cursor: pointer;
}

button:hover {
  background: #ff2e2e;
}

button:disabled {
  background: #888;
  cursor: not-allowed;
}
</style>
