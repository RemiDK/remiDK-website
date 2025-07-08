<template>
    <section class="career">
        <div class="career__title">
            <h2>My journey</h2>
            <div class="career__title__divider"></div>
        </div>
        <div class="career__timeline">
            <div class="career__timeline__line" :style="{ height: `${progressHeight}px` }">
                <div class="career__timeline__line__start"></div>
                <div class="career__timeline__line__end"></div>
            </div>

            <div v-for="(event, index) in events" :key="index" class="career__timeline__event"
                :ref="el => setEventRef(el as HTMLElement, index)"
                :class="[index % 2 === 0 ? 'career__timeline__event--left' : 'career__timeline__event--right', { 'career__timeline__event--visible': event.visible }]"
                :style="{ top: `${index * 160 + 100}px` }">
                <div class="career__timeline__event__content">
                    <h3>{{ event.date }}</h3>
                    <p>{{ event.description }}</p>
                </div>
            </div>
        </div>
    </section>
</template>


<script setup lang="ts">
interface TimelineEvent {
    date: string
    description: string
    visible: boolean
}

const events = ref<TimelineEvent[]>([
    { date: '2013', description: 'Japanese degree', visible: false },
    { date: '2014', description: "Master's degree in Japanese", visible: false },
    { date: '2014–2016', description: 'Two years of living in Tokyo', visible: false },
    { date: '2015', description: 'JLPT N3', visible: false },
    { date: '2022', description: 'Professional license in computer development', visible: false }
])

const eventRefs = ref<HTMLElement[]>([])
const baseOffset = 100
const stepHeight = 160
const progressHeight = ref(0)

const setEventRef = (el: HTMLElement | null, index: number) => {
    if (el)
        eventRefs.value[index] = el
}

const updateProgress = () => {
    const visibleCount = events.value.filter(el => el.visible).length
    progressHeight.value = baseOffset + stepHeight * visibleCount
}

onMounted(() => {
    const observer = new IntersectionObserver((entries) => {
        entries.forEach((entry) => {
            const index = eventRefs.value.findIndex(el => el === entry.target)
            if (index !== -1) {
                const wasVisible = events.value[index].visible
                events.value[index].visible = entry.isIntersecting

                if (wasVisible !== entry.isIntersecting) {
                    updateProgress()
                }
            }
        })
    }, { threshold: 0.5 })

    eventRefs.value.forEach(el => {
        if (el) observer.observe(el)
    })

    onUnmounted(() => {
        observer.disconnect()
    })
})
</script>