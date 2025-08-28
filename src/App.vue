<script setup>
import { ref, computed, watch } from "vue";
import HealthBar from "./components/HealthBar.vue";
import Controls from "./components/Controls.vue";
import BattleLog from "./components/BattleLog.vue";
import GameOver from "./components/GameOver.vue";

function getRandomValue(min, max) {
  return Math.floor(Math.random() * (max - min)) + min;
}

const playerHealth = ref(100);
const monsterHealth = ref(100);
const currentRound = ref(0);
const winner = ref(null);
const logMessages = ref([]);

const monsterBarStyles = computed(() => monsterHealth.value);
const playerBarStyles = computed(() => playerHealth.value);
const mayUseSpecialAttack = computed(() => currentRound.value % 3 !== 0);

watch(playerHealth, (value) => {
  if (value <= 0 && monsterHealth.value <= 0) winner.value = "draw";
  else if (value <= 0) winner.value = "monster";
});

watch(monsterHealth, (value) => {
  if (value <= 0 && playerHealth.value <= 0) winner.value = "draw";
  else if (value <= 0) winner.value = "player";
});

function startGame() {
  playerHealth.value = 100;
  monsterHealth.value = 100;
  winner.value = null;
  currentRound.value = 0;
  logMessages.value = [];
}

function attackMonster() {
  currentRound.value++;
  const attackValue = getRandomValue(5, 12);
  monsterHealth.value -= attackValue;
  addLogMessage("player", "attack", attackValue);
  attackPlayer();
}

function attackPlayer() {
  const attackValue = getRandomValue(8, 15);
  playerHealth.value -= attackValue;
  addLogMessage("monster", "attack", attackValue);
}

function specialAttackMonster() {
  currentRound.value++;
  const attackValue = getRandomValue(10, 25);
  monsterHealth.value -= attackValue;
  addLogMessage("player", "special-attack", attackValue);
  attackPlayer();
}

function healPlayer() {
  currentRound.value++;
  const healValue = getRandomValue(8, 20);
  playerHealth.value = Math.min(playerHealth.value + healValue, 100);
  addLogMessage("player", "heal", healValue);
  attackPlayer();
}

function surrender() {
  winner.value = "monster";
}

function addLogMessage(who, what, value) {
  logMessages.value.unshift({
    actionBy: who,
    actionType: what,
    actionValue: value,
  });
}
</script>

<template>
  <header>
    <h1>Monster Slayer</h1>
  </header>

  <HealthBar label="Monster Health" :healthValue="monsterBarStyles" />
  <HealthBar label="Player Health" :healthValue="playerBarStyles" />

  <GameOver v-if="winner" :winner="winner" @restart="startGame" />
  <Controls
    v-else
    :disabled="!!winner"
    :mayUseSpecialAttack="mayUseSpecialAttack"
    @attack="attackMonster"
    @specialAttack="specialAttackMonster"
    @heal="healPlayer"
    @surrender="surrender"
    @restart="startGame"
  />

  <BattleLog :logMessages="logMessages" />
</template>

<style scoped>
@import url("https://fonts.googleapis.com/css2?family=Jost:wght@400;700&display=swap");

html {
  font-family: "Jost", sans-serif;
}

body {
  margin: 0;
  background-color: #1f1f1f;
  color: white;
}

header {
  text-align: center;
  padding: 1rem;
}
</style>
