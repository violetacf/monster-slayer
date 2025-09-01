<template>
    <div class="character-selector">
        <h2>Choose your hero</h2>
        <div class="options">
            <div v-for="char in characters" :key="char.name"
                :class="['option', { selected: selectedCharacter === char.name }]" @click="selectCharacter(char.name)">
                <img :src="char.img" :alt="char.name" />
                <p>{{ char.name }}</p>
            </div>
        </div>
        <button :disabled="!selectedCharacter" @click="confirmSelection">
            Confirm
        </button>
    </div>
</template>

<script setup>
import { ref, defineProps, defineEmits } from 'vue'

const props = defineProps({
    characters: { type: Array, required: true } // [{ name, img }]
})

const emit = defineEmits(['characterSelected'])
const selectedCharacter = ref(null)

function selectCharacter(name) {
    selectedCharacter.value = name
}

function confirmSelection() {
    if (selectedCharacter.value) emit('characterSelected', selectedCharacter.value)
}
</script>

<style scoped>
.character-selector {
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 1rem;
}

.options {
    display: flex;
    gap: 2rem;
    margin: 1rem 0;
}

.option {
    display: flex;
    flex-direction: column;
    align-items: center;
    cursor: pointer;
    transition: transform 0.2s, box-shadow 0.2s;
}

.option img {
    width: 100px;
    height: 100px;
    object-fit: contain;
}

.option.selected {
    transform: scale(1.1);
    box-shadow: 0 0 20px #ffeb3b;
}

button {
    padding: 0.5rem 1rem;
    cursor: pointer;
    font-weight: bold;
    background-color: #4caf50;
    color: white;
    border: none;
    border-radius: 5px;
    transition: transform 0.2s;
}

button:disabled {
    opacity: 0.5;
    cursor: not-allowed;
}

button:hover:not(:disabled) {
    transform: scale(1.05);
}
</style>
