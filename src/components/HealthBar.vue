<template>
  <section class="container">
    <h2>{{ label }}</h2>
    <div class="healthbar">
      <div
        class="healthbar__value"
        :style="{ 
          width: healthValue + '%',
          backgroundColor: healthValue > 50 ? '#4caf50' 
                          : healthValue > 20 ? '#ff9800' 
                          : '#f44336'
        }"
      ></div>
    </div>
  </section>
</template>

<script setup>
import { defineProps } from 'vue'

const props = defineProps({
  label: String,
  healthValue: Number
})
</script>

<style scoped>
.container {
  margin: 1rem 0;
  text-align: center;
}

.healthbar {
  width: 100%;
  height: 2rem;
  border-radius: 8px;
  background: #444;
  box-shadow: inset 0 2px 4px rgba(0,0,0,0.5);
  overflow: hidden;
}

.healthbar__value {
  height: 100%;
  border-radius: 8px;
  transition: width 0.6s ease, background-color 0.3s ease;
  box-shadow: inset 0 -1px 2px rgba(0,0,0,0.3);
}
</style>
