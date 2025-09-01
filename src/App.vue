<template>
  <header>
    <h1>Monster Slayer</h1>
  </header>

  <main class="game-container">
    <CharacterSelector v-if="!chosenCharacter" :characters="pokemons" @characterSelected="onCharacterSelected" />

    <div v-else>
      <div class="characters-container">
        <Character :img="monsterGif" :action="monsterAction" />
        <Character :img="chosenHeroImg" :action="pokemonAction" />
      </div>

      <div class="healthbars-container">
        <HealthBar label="Monster Health" :healthValue="monsterBarStyles" />
        <HealthBar :label="`${chosenCharacter} Health`" :healthValue="pokemonBarStyles" />
      </div>

      <div class="controls-container">
        <Controls :disabled="!!winner" :mayUseSpecialAttack="mayUseSpecialAttack" @attack="attackMonster"
          @specialAttack="specialAttackMonster" @heal="healPokemon" @surrender="surrender" @restart="startGame" />
      </div>

      <div class="battle-log-container">
        <BattleLog :logMessages="logMessages" />
      </div>

      <GameOver v-if="winner" :winner="winner" @restart="startGame" class="overlay-gameover" />
    </div>
  </main>
</template>

<script setup>
import { ref, computed, watch } from "vue";
import HealthBar from "./components/HealthBar.vue";
import Controls from "./components/Controls.vue";
import BattleLog from "./components/BattleLog.vue";
import GameOver from "./components/GameOver.vue";
import Character from "./components/Character.vue";
import CharacterSelector from "./components/CharacterSelector.vue";

import MewGif from './assets/Mew.gif';
import PikachuGif from './assets/Pikachu.gif';
import CharizardGif from './assets/Charizard.gif';
import MonsterGif from './assets/Monster.gif';

function getRandomValue(min, max) {
  return Math.floor(Math.random() * (max - min)) + min;
}

const pokemonHealth = ref(100);
const monsterHealth = ref(100);
const currentRound = ref(0);
const winner = ref(null);
const logMessages = ref([]);

const pokemonAction = ref(null);
const monsterAction = ref(null);

const monsterBarStyles = computed(() => monsterHealth.value);
const pokemonBarStyles = computed(() => pokemonHealth.value);
const mayUseSpecialAttack = computed(() => currentRound.value % 3 !== 0);

const monsterGif = MonsterGif;

const pokemons = [
  { name: 'Mew', img: MewGif },
  { name: 'Pikachu', img: PikachuGif },
  { name: 'Charizard', img: CharizardGif },
];

const chosenCharacter = ref(null);

function onCharacterSelected(characterName) {
  chosenCharacter.value = characterName;
}

// Computed hero GIF
const chosenHeroImg = computed(() => {
  const hero = pokemons.find(h => h.name === chosenCharacter.value);
  return hero ? hero.img : '';
});

watch(pokemonHealth, (value) => {
  if (value <= 0 && monsterHealth.value <= 0) winner.value = "draw";
  else if (value <= 0) winner.value = "monster";
});

watch(monsterHealth, (value) => {
  if (value <= 0 && pokemonHealth.value <= 0) winner.value = "draw";
  else if (value <= 0) winner.value = "pokemon";
});

function startGame() {
  pokemonHealth.value = 100;
  monsterHealth.value = 100;
  winner.value = null;
  currentRound.value = 0;
  logMessages.value = [];
  pokemonAction.value = null;
  monsterAction.value = null;
  chosenCharacter.value = null;
}

function attackMonster() {
  currentRound.value++;
  const attackValue = getRandomValue(5, 12);
  monsterHealth.value -= attackValue;
  addLogMessage(chosenCharacter.value, "attack", attackValue);
  pokemonAction.value = "attack";
  attackPokemon();
}

function attackPokemon() {
  const attackValue = getRandomValue(8, 15);
  pokemonHealth.value -= attackValue;
  addLogMessage("monster", "attack", attackValue);
  monsterAction.value = "attack";
}

function specialAttackMonster() {
  currentRound.value++;
  const attackValue = getRandomValue(10, 25);
  monsterHealth.value -= attackValue;
  addLogMessage(chosenCharacter.value, "special-attack", attackValue);
  pokemonAction.value = "attack";
  attackPokemon();
}

function healPokemon() {
  currentRound.value++;
  const healValue = getRandomValue(8, 20);
  pokemonHealth.value = Math.min(pokemonHealth.value + healValue, 100);
  addLogMessage(chosenCharacter.value, "heal", healValue);
  pokemonAction.value = "heal";
  attackPokemon();
}

function surrender() {
  winner.value = "monster";
}

function addLogMessage(who, what, value) {
  let characterName = '';
  let characterImg = '';

  if (who === 'monster') {
    characterName = 'Monster';
    characterImg = MonsterGif;
  } else {
    const hero = pokemons.find(p => p.name === chosenCharacter.value);
    if (hero) {
      characterName = hero.name;
      characterImg = hero.img;
    }
  }

  logMessages.value.unshift({
    actionBy: who === 'monster' ? 'monster' : 'pokemon',
    actionType: what,
    actionValue: value,
    characterName,
    characterImg
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
