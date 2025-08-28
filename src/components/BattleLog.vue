<template>
  <section id="log" class="container">
    <h2>Battle Log</h2>
    <ul>
      <li v-for="(logMessage, index) in logMessages" :key="index">
        <span
          :class="{
            'log--player': logMessage.actionBy === 'player',
            'log--monster': logMessage.actionBy === 'monster',
          }"
        >
          {{ logMessage.actionBy === "player" ? "Player" : "Monster" }}
        </span>
        <span v-if="logMessage.actionType === 'heal'">
          heals himself for
          <span class="log--heal">{{ logMessage.actionValue }}</span>
        </span>
        <span v-else>
          attacks and deals
          <span class="log--damage">{{ logMessage.actionValue }}</span>
        </span>
      </li>
    </ul>
  </section>
</template>

<script setup>
import { defineProps } from "vue";

const props = defineProps({
  logMessages: Array,
});
</script>

<style scoped>
#log ul {
  list-style: none;
  margin: 0;
  padding: 0;
}

#log li {
  margin: 0.25rem 0;
}

.log--player {
  color: #4da6ff;
  font-weight: bold;
}

.log--monster {
  color: #ff4747;
  font-weight: bold;
}

.log--heal {
  color: limegreen;
}

.log--damage {
  color: orange;
}
</style>
