<script setup lang="ts">
import { PCFSoftShadowMap, SRGBColorSpace } from 'three'

import Planet from './Planet.vue'
import { shallowRef, watch } from 'vue'

const state = {
  clearColor: '#11101B',
  shadows: true,
  alpha: false,
  outputColorSpace: SRGBColorSpace,
  useLegacyLights: true,
  shadowMapType: PCFSoftShadowMap,
}
const directionalLightRef = shallowRef<THREE.DirectionalLight>()

watch(
  () => directionalLightRef.value,
  directionalLight => {
    if (!directionalLight) return

    directionalLight.shadow.mapSize.width = 2048
    directionalLight.shadow.mapSize.height = 2048
  },
)
</script>

<template>
  <TresCanvas v-bind="state">
    <TresPerspectiveCamera :position="[0, 1, 3]" :fov="75" :near="0.1" :far="1000" />
    <OrbitControls />

    <TresFog color="#11101B" :near="0.1" :far="1000" />
    <TresAmbientLight :color="0x484068" :intensity="1" />
    <Suspense>
      <Planet />
    </Suspense>

    <TresAxesHelper />
    <TresPointLight color="#1BFFEF" :position="[0, 8, -16]" :intensity="8" cast-shadow />
    <TresDirectionalLight ref="directionalLightRef" :position="[0, 2, 4]" :intensity="1" cast-shadow />
  </TresCanvas>
</template>
