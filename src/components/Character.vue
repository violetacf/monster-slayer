<template>
    <div ref="characterRef" class="character">
        <img :src="characterImg" alt="character" />
    </div>
</template>

<script setup>
import { ref, watch, onMounted } from 'vue'
import { gsap } from 'gsap'

const props = defineProps({
    img: { type: String, required: true },
    action: String,
})

const characterRef = ref(null)
const characterImg = props.img

watch(
    () => props.action,
    (newAction) => {
        if (!newAction) return

        if (newAction === 'attack') {
            gsap.fromTo(
                characterRef.value,
                { x: 0, scale: 1, rotate: 0 },
                {
                    x: 20,
                    scale: 1.1,
                    rotate: 10,
                    duration: 0.1,
                    yoyo: true,
                    repeat: 3,
                    ease: 'power1.inOut',
                    onComplete: () =>
                        gsap.to(characterRef.value, { x: 0, scale: 1, rotate: 0, duration: 0.1 }),
                }
            )
        }

        if (newAction === 'heal') {
            gsap.fromTo(
                characterRef.value,
                { scale: 1 },
                { scale: 1.15, duration: 0.3, yoyo: true, repeat: 1, ease: 'power1.inOut' }
            )
        }
    }
)

onMounted(() => {
    gsap.from(characterRef.value, { opacity: 0, y: -50, duration: 0.5 })
    gsap.to(characterRef.value, {
        y: '+=10',
        duration: 2,
        repeat: -1,
        yoyo: true,
        ease: 'sine.inOut',
    })
})
</script>

<style scoped>
.character {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 120px;
    height: 120px;
    margin: 1rem auto;
    filter: drop-shadow(0 4px 6px rgba(0, 0, 0, 0.3));
}

.character img {
    max-width: 100%;
    max-height: 100%;
    object-fit: contain;
}
</style>
