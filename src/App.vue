<template>
  <header>
    <h1>Monster Slayer</h1>
  </header>

  <main class="game-container">

    <!-- Characters -->
    <div class="characters-container">
      <Character type="monster" :action="monsterAction" />
      <Character type="player" :action="playerAction" />
    </div>

    <div class="healthbars-container">
      <HealthBar label="Monster Health" :healthValue="monsterBarStyles" />
      <HealthBar label="Player Health" :healthValue="playerBarStyles" />
    </div>

    <div class="controls-container">
      <Controls :disabled="!!winner" :mayUseSpecialAttack="mayUseSpecialAttack" @attack="attackMonster"
        @specialAttack="specialAttackMonster" @heal="healPlayer" @surrender="surrender" @restart="startGame" />
    </div>

    <div class="battle-log-container">
      <BattleLog :logMessages="logMessages" />
    </div>

    <GameOver v-if="winner" :winner="winner" @restart="startGame" class="overlay-gameover" />
  </main>
</template>

<script setup>
import { ref, computed, watch } from "vue";
import HealthBar from "./components/HealthBar.vue";
import Controls from "./components/Controls.vue";
import BattleLog from "./components/BattleLog.vue";
import GameOver from "./components/GameOver.vue";
import Character from "./components/Character.vue"; // <-- añadido

function getRandomValue(min, max) {
  return Math.floor(Math.random() * (max - min)) + min;
}

const playerHealth = ref(100);
const monsterHealth = ref(100);
const currentRound = ref(0);
const winner = ref(null);
const logMessages = ref([]);

const playerAction = ref(null)
const monsterAction = ref(null)

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
  playerAction.value = null
  monsterAction.value = null
}

function attackMonster() {
  currentRound.value++;
  const attackValue = getRandomValue(5, 12);
  monsterHealth.value -= attackValue;
  addLogMessage("player", "attack", attackValue);
  playerAction.value = "attack"
  attackPlayer();
}

function attackPlayer() {
  const attackValue = getRandomValue(8, 15);
  playerHealth.value -= attackValue;
  addLogMessage("monster", "attack", attackValue);
  monsterAction.value = "attack"
}

function specialAttackMonster() {
  currentRound.value++;
  const attackValue = getRandomValue(10, 25);
  monsterHealth.value -= attackValue;
  addLogMessage("player", "special-attack", attackValue);
  playerAction.value = "attack"
  attackPlayer();
}

function healPlayer() {
  currentRound.value++;
  const healValue = getRandomValue(8, 20);
  playerHealth.value = Math.min(playerHealth.value + healValue, 100);
  addLogMessage("player", "heal", healValue);
  playerAction.value = "heal"
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

.game-container {
  display: flex;
  flex-direction: column;
  max-width: 600px;
  margin: 0 auto;
  height: calc(100vh - 80px);
  padding: 0.5rem;
  position: relative;
}

/* Contenedor fijo de los personajes */
.characters-container {
  display: flex;
  justify-content: space-around;
  width: 100%;
  height: 120px;
  margin-bottom: 1rem;
}

.healthbars-container {
  flex: 0 0 auto;
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.controls-container {
  flex: 0 0 auto;
}

.battle-log-container {
  flex: 1 1 auto;
  width: 100%;
  overflow-y: auto;
  padding: 0.5rem;
}

.overlay-gameover {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 100%;
  max-width: 500px;
  z-index: 100;
  pointer-events: auto;
}
</style>
