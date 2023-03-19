<template>
  <div id="htmlPage">
  </div>

  <div id="htmlPage2">

  </div>

  <div id="htmlPage3">
    <img id="clouds" src="clouds.png"/>
    <img id="clouds2" src="clouds.png"/>
    <img id="clouds3" src="clouds.png"/>
    <img id="clouds4" src="clouds.png"/>
  </div>

  <div id="contact">
  </div>

  <Renderer ref="rendererC" :alpha="true" antialias :orbit-ctrl="{ enabled: false }" resize="window">
    <Camera ref="cameraC" :position="{ z: -20 }" />
    <Scene>
      <AmbientLight :intensity="1" :position="{ y: 0, z: 0 }" />
      <Points ref="pointsC" :position="{y: 0,z: 50}"/>
      <Points ref="pointsD" :position="{y: 0,z: 50}"/>

    </Scene>
  </Renderer>

</template>

<script setup>
import { ref, onMounted } from 'vue'
import { AmbientLight, Points, Box, Camera, LambertMaterial, PointLight, Renderer, Scene } from 'troisjs'
import {Vector3, BufferAttribute, BufferGeometry, PointsMaterial} from "three";
const rendererC = ref()
const pointsC = ref()
const pointsD = ref()
const cameraC = ref()

const starMaterial = new PointsMaterial({
  size: .17,
  color: 0xffffff
})

const geometry = new BufferGeometry();
const geometry2 = new BufferGeometry();

const getRandomParticlePos = (particleCount) => {
  const arr = new Float32Array(particleCount * 3);
  for(let i = 0; i < particleCount; i++) {
    arr[i] = (Math.random() - .5) * 100;
  }
  return arr;
}

const getRandomParticlePos2 = (particleCount) => {
  const arr = new Float32Array(particleCount * 3);
  for(let i = 0; i < particleCount; i++) {
    arr[i] = (Math.random() - .5) * 150;
  }
  return arr;
}

geometry.setAttribute(
    "position",
    new BufferAttribute(getRandomParticlePos(7000),3)
);

geometry2.setAttribute(
    "position",
    new BufferAttribute(getRandomParticlePos2(7000),3)
);

onMounted(() => {
  const renderer = rendererC.value
  const points1 = pointsC.value
  const points2 = pointsD.value
  const camera = cameraC.value.camera
  const rendererRef = rendererC.value.renderer


  points1.setMaterial(starMaterial)
  points1.setGeometry(geometry)

  points2.setMaterial(starMaterial)
  points2.setGeometry(geometry2)

  function updateCamera() {
    camera.updateProjectionMatrix()
  }

  renderer.onBeforeRender(() => {
    points1.mesh.rotation.x += .0006
    points2.mesh.rotation.x += .00009

    updateCamera()
  })
})
</script>

<style>
#htmlPage{
  overflow-x: hidden;
  position: absolute;
  background-color: black;
  color: white;
  font-size: 10vw;
  height: 100vh;
  width: 100vw;
}

#htmlPage2{
  overflow-x: hidden;
  position: absolute;
  color: white;
  margin-top: 100vh;
  font-size: 10vw;
  height: 100vh;
  width: 100vw;
  background-image: linear-gradient(black, rgb(48,120,164));
}

#htmlPage3{
  overflow-x: hidden;
  overflow-y: hidden;
  position: absolute;
  background-image: linear-gradient(rgb(48,120,164), white);
  color: white;
  margin-top: 200vh;
  font-size: 10vw;
  height: 100vh;
  width: 100vw;
}

#contact{
  overflow-x: hidden;
  overflow-y: hidden;
  position: absolute;
  background-color: transparent;
  color: black;
  margin: 250vh 20vw 0 20vw;
  font-size: 10vw;
  height: 50vh;
  width: 60vw;
  z-index: 10;
}

#clouds{
  opacity: 0;
  bottom: 0;
  position: absolute;
  animation-name: cloud;
  animation-duration: 30s;
  animation-delay: 0s;
  animation-iteration-count: infinite;
}

#clouds2{
  opacity: 0;
  bottom: 0;
  position: absolute;
  animation-name: cloud;
  animation-duration: 30s;
  animation-delay: 5s;
  animation-iteration-count: infinite;
}

#clouds3{
  opacity: 0;
  bottom: 0;
  position: absolute;
  animation-name: cloud;
  animation-duration: 30s;
  animation-delay: 10s;
  animation-iteration-count: infinite;
}

#clouds4{
  opacity: 0;
  bottom: 0;
  position: absolute;
  animation-name: cloud;
  animation-duration: 36s;
  animation-delay: 15s;
  animation-iteration-count: infinite;
}

@keyframes cloud {
  0% {scale: 100%; translate: 0 0; opacity: 0}
  30% {opacity: 15}
  60% {opacity: 100}
  100% {scale: 160%; translate: 40vw 40vh; opacity: 0}
}

body {
  overflow-y: auto;
  padding: 0;
  margin: 0;
}
canvas {
  position: absolute;
  overflow-x: hidden;
  top:0;
  height: 100vh;
  width: 100vw;
  z-index: 10;
}
</style>