<script setup lang="ts">
import { GLTFModel } from '@tresjs/cientos'
import type { SkillItem } from '~/types'
const { onLoop } = useRenderLoop()
const meshRef = shallowRef()
const isRotationEnable = ref<boolean>(true)
const meshScale = ref(1)
const skillItems = ref<HTMLElement[]>([])
const currentIndex = ref<number>(-1)
const skills: SkillItem[] = [
    {
        title: 'Vue.js',
        description: 'Lightweight, reactive JavaScript framework that I use to create dynamic interfaces.'
    },
    {
        title: 'Nuxt',
        description: 'Vue.js-based framework. Ideal for routing, SSR and SEO optimization.'
    },
    {
        title: 'Sass/SCSS',
        description: 'CSS preprocessor that I use to structure my styles. A well-organized code to help you find your way around.'
    },
    {
        title: 'Wordpress',
        description: 'Simple website with visual builder or PHP development'
    }
]

onLoop(({ delta, elapsed }) => {
    if (meshRef.value && isRotationEnable.value) {
        meshRef.value.rotation.y = elapsed * 0.2
        meshRef.value.rotation.Z = elapsed * 0.2
    }
})

onMounted(() => {
    nextTick(() => {
        const observer = new IntersectionObserver(entries => {
            const visible = entries
                .filter(entry => entry.isIntersecting)
                .sort((entryA, entryB) => entryA.boundingClientRect.top - entryB.boundingClientRect.top)[0]

            if (visible) {
                const index = skillItems.value.findIndex(item => item === visible.target)
                if (index !== -1) currentIndex.value = index
            }
        }, { threshold: 0.5 })

        skillItems.value.forEach(item => {
            if (item) observer.observe(item)
        })
    })
})
</script>

<template>
    <div class="about">
        <div class="container">
            <div class="about__skills">
                <h2>Technologies</h2>
                <div v-for="(skill, index) in skills" :key="index" class="about__skills__item"
                    :class="{ 'active': currentIndex === index }" ref="skillItems">
                    <div class="about__skills__item__inner">
                        <h3>{{ skill.title }}</h3>
                        <p>{{ skill.description }}</p>
                    </div>
                </div>
            </div>
            <div class="about__canvas">
                <div class="about__canvas__controls-container">
                    <div class="about__canvas__controls">
                        <div class="about__canvas__controls__header">
                            <h2>Control Panel</h2>
                        </div>
                        <div class="about__canvas__controls__content">
                            <div class="form-group form-group--row">
                                <label for="autoRotate" class="form-label">
                                    <div class="form-checkbox">
                                        <input id="autoRotate" type="checkbox" v-model="isRotationEnable" />
                                        <span class="form-slider"></span>
                                    </div>
                                    Auto Rotation
                                </label>
                            </div>
                            <div class="form-group">
                                <label class="form-label">Size</label>
                                <div class="form-range">
                                    <input type="range" min="0" max="1" step="0.1" v-model="meshScale"
                                        class="form-range__slider" />
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
                <ClientOnly>
                    <TresCanvas>
                        <OrbitControls :enable-zoom="false" />
                        <TresPerspectiveCamera :position="[0, 3, 3]" :look-at="[0, 0, 0]" />
                        <Suspense>
                            <TresGroup ref="meshRef" :rotation="[Math.PI / 8, 0, 0]">
                                <GLTFModel :path="'/maple.glb'" :scale="[meshScale, meshScale, meshScale]"
                                    :position="[0, 0, 0]" />
                            </TresGroup>
                        </Suspense>
                        <TresAmbientLight :intensity="1" />
                        <TresDirectionalLight :position="[10, 10, 10]" :intensity="1" />
                    </TresCanvas>
                </ClientOnly>
            </div>
        </div>
    </div>
</template>