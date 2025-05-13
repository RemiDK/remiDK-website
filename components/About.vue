<script setup lang="ts">
import { GLTFModel } from '@tresjs/cientos'
const { onLoop } = useRenderLoop()
const meshRef = shallowRef()

onLoop(({ delta, elapsed }) => {
    if (meshRef.value) {
        meshRef.value.rotation.y = elapsed * 0.2
        meshRef.value.rotation.Z = elapsed * 0.2
    }
})
</script>

<template>
    <div class="about">
        <div class="container">
            <div class="about__skills">
                <h2>Outils utilisés</h2>
                <div class="about__skills__list">
                    <Skill name="Vue.js" />
                    <Skill name="Nuxt" />
                    <Skill name="SCSS / Sass" />
                    <Skill name="Typescript" />
                </div>
            </div>
            <div class="about__canvas">
                <TresCanvas>
                    <TresPerspectiveCamera :position="[0, 3, 3]" :look-at="[0, 0, 0]" />
                    <Suspense>
                        <TresGroup ref="meshRef" :rotation="[Math.PI / 8, 0, 0]">
                            <GLTFModel :path="'/maple.glb'" :scale="[1, 1, 1]" :position="[0, 0, 0]" />
                        </TresGroup>
                    </Suspense>
                    <TresAmbientLight :intensity="1" />
                    <TresDirectionalLight :position="[10, 10, 10]" :intensity="1" />
                </TresCanvas>
            </div>
        </div>

    </div>
</template>