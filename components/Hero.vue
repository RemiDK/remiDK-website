<script setup lang="ts">
const container = useTemplateRef('hero')
let animationFrameId: number
let lastTime = 0
const delay = 1000
const maxLeaves = 10
let leafCount = 0

const createLeaf = () => {
    if (!container.value || leafCount >= maxLeaves) return

    const leaf = document.createElement('div')
    leaf.className = 'leaf'
    leaf.innerHTML = `<svg viewBox="0 0 100 100" width="20" height="20" fill="currentColor">
      <path d="M50,0 C60,20 80,20 100,30 C80,40 70,60 80,100 C60,80 40,80 20,100 C30,60 20,40 0,30 C20,20 40,20 50,0 Z" />
    </svg>`;

    const size = Math.random() * 15 + 10
    const startPosX = Math.random() * window.innerWidth
    const rotation = Math.random() * 360
    const duration = Math.random() * 10 + 10
    const delayAnimation = Math.random() * 2

    const colors = ["#9E2B25", "#2A5E45", "#8B5A2B"]
    const color = colors[Math.floor(Math.random() * colors.length)]
    Object.assign(leaf.style, {
        width: `${size}px`,
        height: `${size}px`,
        left: `${startPosX}px`,
        color,
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

const loop = (time: number) => {
    if (time - lastTime >= delay && document.visibilityState === 'visible') {
        createLeaf()
        lastTime = time
    }

    animationFrameId = requestAnimationFrame(loop)
}

onMounted(() => {
    animationFrameId = requestAnimationFrame(loop)
})

onBeforeUnmount(() => {
    cancelAnimationFrame(animationFrameId)
})
</script>

<template>
    <section ref="hero" id="hero" class="hero">
        <div class="hero__overlay"></div>
        <div class="hero__content">
            <h1>Kaede</h1>
            <p>Développeur informatique</p>
        </div>
        <a href="#about" class="hero__button" aria-label="Scroll to next section">
            ↓
        </a>
    </section>
</template>