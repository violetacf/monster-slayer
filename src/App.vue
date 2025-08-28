<script setup>
import { ref, computed, watch } from "vue";

function getRandomValue(min, max) {
  return Math.floor(Math.random() * (max - min)) + min;
}

const playerHealth = ref(100);
const monsterHealth = ref(100);
const currentRound = ref(0);
const winner = ref(null);
const logMessages = ref([]);

const monsterBarStyles = computed(() => {
  return { width: (monsterHealth.value < 0 ? 0 : monsterHealth.value) + "%" };
});

const playerBarStyles = computed(() => {
  return { width: (playerHealth.value < 0 ? 0 : playerHealth.value) + "%" };
});

const mayUseSpecialAttack = computed(() => currentRound.value % 3 !== 0);

watch(playerHealth, (value) => {
  if (value <= 0 && monsterHealth.value <= 0) {
    winner.value = "draw";
  } else if (value <= 0) {
    winner.value = "monster";
  }
});

watch(monsterHealth, (value) => {
  if (value <= 0 && playerHealth.value <= 0) {
    winner.value = "draw";
  } else if (value <= 0) {
    winner.value = "player";
  }
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
  if (playerHealth.value + healValue > 100) {
    playerHealth.value = 100;
  } else {
    playerHealth.value += healValue;
  }
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

  <div id="game">
    <section id="monster" class="container">
      <h2>Monster Health</h2>
      <div class="healthbar">
        <div class="healthbar__value" :style="monsterBarStyles"></div>
      </div>
    </section>

    <section id="player" class="container">
      <h2>Your Health</h2>
      <div class="healthbar">
        <div class="healthbar__value" :style="playerBarStyles"></div>
      </div>
    </section>

    <section class="container" v-if="winner">
      <h2>Game Over!</h2>
      <h3 v-if="winner === 'player'">You've won!!!</h3>
      <h3 v-else-if="winner === 'monster'">You've lost!!!</h3>
      <h3 v-else>It's a draw!</h3>
      <button @click="startGame">Start New Game</button>
    </section>

    <section id="controls" v-else>
      <button @click="attackMonster">ATTACK</button>
      <button :disabled="mayUseSpecialAttack" @click="specialAttackMonster">
        SPECIAL ATTACK
      </button>
      <button @click="healPlayer">HEAL</button>
      <button @click="surrender">SURRENDER</button>
    </section>

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
  </div>
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

.container {
  margin: 1rem auto;
  padding: 1rem;
  max-width: 40rem;
  border: 1px solid #ccc;
  border-radius: 6px;
  background: #333;
}

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
