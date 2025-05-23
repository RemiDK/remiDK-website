<script setup lang="ts">
import mapleLeaf from '@/assets/svg/maple-leaf.svg?raw';

const container = useTemplateRef('hero')
let isHeroVisible = false;
let heroContainerObserver: IntersectionObserver
let animationFrameId: number
let lastTime = 0
const maxLeaves = 10
let leafCount = 0

const createLeaf = () => {
    if (!container.value || leafCount >= maxLeaves) return

    const leaf = document.createElement('div')
    leaf.className = 'leaf'
    leaf.innerHTML = mapleLeaf;

    const startPosX = Math.random() * window.innerWidth
    const rotation = Math.random() * 360
    const duration = Math.random() * 10 + 10
    const delayAnimation = Math.random() * 2

    Object.assign(leaf.style, {
        left: `${startPosX}px`,
        transform: `rotate(${rotation}deg)`
    })
    container.value?.appendChild(leaf)
    leafCount++

    setTimeout(() => {
        leaf.style.top = `${window.innerHeight + 100}px`;
        leaf.style.left = `${startPosX + (Math.random() * 200 - 100)}px`;
        leaf.style.transform = `rotate(${rotation + Math.random() * 360}deg)`;
        leaf.style.opacity = "0";
        leaf.style.transition = `all ${duration}s ease-in-out`;

        setTimeout(() => {
            if (leaf.parentNode) leaf.parentElement?.removeChild(leaf)
            leafCount--
        }, duration * 1000)
    }, delayAnimation * 1000)
}

onMounted(() => {
    if (container.value) {
        heroContainerObserver = new IntersectionObserver(
            ([entry]) => {
                isHeroVisible = entry.isIntersecting
            },
            { threshold: 0.3 }
        )
        heroContainerObserver.observe(container.value)
    }
    const loop = (timestamp = performance.now()) => {
        if (document.visibilityState === 'visible' && isHeroVisible) {
            if (timestamp - lastTime >= 1000) {
                createLeaf()
                lastTime = timestamp
            }
        }
        animationFrameId = requestAnimationFrame(loop)
    }
    loop()
})

onBeforeUnmount(() => {
    heroContainerObserver.disconnect()
    cancelAnimationFrame(animationFrameId)
})
</script>

<template>
    <section ref="hero" id="hero" class="hero">
        <div class="hero__overlay"></div>
        <div class="hero__content">
            <h1>Kaede</h1>
            <p>French frontend developer</p>
        </div>
        <a href="#about" class="hero__button" aria-label="Scroll to next section">
            ↓
        </a>
    </section>
</template>