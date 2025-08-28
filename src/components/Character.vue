<template>
    <div ref="characterRef" :class="['character', type]">
        <span>{{ emoji }}</span>
    </div>
</template>

<script setup>
import { ref, watch, onMounted, computed } from 'vue'
import { gsap } from 'gsap'

const props = defineProps({
    type: { type: String, default: 'player' }, // 'player' o 'monster'
    action: String, // 'attack', 'heal', etc.
})

const characterRef = ref(null)
const emoji = computed(() => (props.type === 'player' ? '🧙‍♂️' : '👹'))

watch(() => props.action, (newAction) => {
    if (!newAction) return

    if (newAction === 'attack') {
        gsap.fromTo(
            characterRef.value,
            { x: 0 },
            { x: props.type === 'player' ? 20 : -20, duration: 0.1, yoyo: true, repeat: 3 }
        )
    }

    if (newAction === 'heal') {
        gsap.fromTo(characterRef.value, { scale: 1 }, { scale: 1.2, duration: 0.3, yoyo: true, repeat: 1 })
    }
})

onMounted(() => {
    gsap.from(characterRef.value, { opacity: 0, y: -50, duration: 0.5 })
})
</script>

<style scoped>
.character {
    font-size: 4rem;
    display: flex;
    justify-content: center;
    align-items: center;
    width: 100px;
    height: 100px;
    margin: 1rem auto;
}

.character.player {
    color: #4caf50;
}

.character.monster {
    color: #f44336;
}
</style>
