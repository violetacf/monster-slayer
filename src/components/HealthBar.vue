<template>
  <section class="container">
    <h2>{{ label }}</h2>
    <div class="healthbar">
      <div class="healthbar__value" :style="{ width: healthValue + '%' }"></div>
    </div>
  </section>
</template>

<script setup>
import { defineProps } from "vue";

const props = defineProps({
  label: String,
  healthValue: Number,
});
</script>

<style scoped>
.healthbar {
  width: 100%;
  height: 2rem;
  border: 1px solid #ccc;
  border-radius: 4px;
  background: #555;
  margin: 0.5rem 0;
}

.healthbar__value {
  height: 100%;
  background: limegreen;
  border-radius: 4px;
  transition: width 0.3s ease-out;
}
</style>
