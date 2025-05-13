<script setup lang="ts">
import { GLTFModel } from '@tresjs/cientos'
const { onLoop } = useRenderLoop()
const meshRef = shallowRef()
const isRotationEnable = ref<boolean>(true)
const meshScale = ref(1)

onLoop(({ delta, elapsed }) => {
    if (meshRef.value && isRotationEnable.value) {
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
                                    Rotation auto
                                </label>
                            </div>
                            <div class="form-group">
                                <label class="form-label">Taille</label>
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