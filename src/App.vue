<template>

  <div id="navbar"><img width="25vw" src="/letter-f-16.ico"/></div>

    <div id="htmlPage">
      <div id="headline">Innovative designs for forward-thinking brands</div>
    </div>

    <div id="htmlPage2"><div style="padding-top: 20vh">
      <div id="mywork">MY WORK</div>
      <div id="portfolio"><img id="portPics" src="/resumePage.png"/><div id="description">Resume Page</div></div>
      <div id="portfolio"><img id="portPics" src="/onePNG.png"/><div id="description">Freelance Work Promotion</div></div>
      <div id="portfolio"><img id="portPics" src="/solarsystem.png"/><div id="description">Solar System Model</div></div>
      <div id="portfolio"><img id="portPics" src="/tompademo.png"/><div id="description">Senior Design Project - VR Game</div></div>
    </div>
    </div>

    <div id="htmlPage3">
      <img id="clouds" src="/clouds.png"/>
      <img id="clouds2" src="/clouds.png"/>
      <img id="clouds3" src="/clouds.png"/>
      <img id="clouds4" src="/clouds.png"/>
    </div>

    <div id="contact">
      <div style="width: 36vw; padding: 40vh 32vw 0 32vw">CONTACT ME</div>
      <div style="margin: 0 5vw;">
        <a href="https://www.linkedin.com/in/fpimentel/"><img style="margin: 5vh 10vw; width: 10vw;" src="/linkedinlogo.svg" /></a>
        <a href="https://github.com/fpimentel-threejs/"><img style="margin: 5vh 10vw; width: 10vw;" src="/githublogo.svg" /></a>
        <a href="https://www.instagram.com/rxnando/"><img style="margin: 5vh 10vw; width: 10vw;" src="/instagramlogo.svg" /></a>
      </div>
    </div>

  <Renderer ref="rendererC" :alpha="true" antialias :orbit-ctrl="{ enabled: false }" resize="window">
    <Camera ref="cameraC" :position="{ z: -20 }" />
    <Scene>
      <AmbientLight :intensity="1" :position="{ y: 0, z: 0 }" />
      <Points ref="pointsC" :position="{y: 0,z: -30}"/>
      <Points ref="pointsD" :position="{y: 0,z: -30}"/>
      <Group ref="satteliteC" :rotation="{x: 1, y: 1, z: 1}">
        <Cylinder :rotation="{z: 90*Math.PI /180}" :height="4"><StandardMaterial><Texture src="/spacemetal1.jpg" /></StandardMaterial></Cylinder>
        <Cylinder :rotation="{z: 90*Math.PI /180}" :height="6" :radiusTop=".3" :radiusBottom="2"><StandardMaterial><Texture src="/spacemetal1.jpg" /></StandardMaterial></Cylinder>
        <Cylinder :position="{y: 2}" :radiusTop=".3" :radiusBottom=".1" :height="4"><StandardMaterial><Texture src="/spacemetal1.png" /></StandardMaterial></Cylinder>
        <Cylinder :position="{y: -2}" :radiusTop=".1" :radiusBottom=".3" :height="4"><StandardMaterial><Texture src="/spacemetal1.png" /></StandardMaterial></Cylinder>
        <Box :position="{y: 6}" :width="2" :depth=".5" :height="4"><StandardMaterial><Texture src="/spacemetal1.jpg"/></StandardMaterial></Box>
        <Box :position="{y: -6}" :width="1.5" :depth=".5" :height="4"><StandardMaterial><Texture src="/spacemetal1.jpg"/></StandardMaterial></Box>
      </Group>

    </Scene>
  </Renderer>

</template>

<script setup>
import { ref, onMounted } from 'vue'
import { StandardMaterial, Texture, Sphere, Cylinder, AmbientLight, Points, Box, Camera, LambertMaterial, PointLight, Renderer, Scene } from 'troisjs';
import { Vector3, BufferAttribute, BufferGeometry, PointsMaterial} from "three";
import * as THREE from 'three'
const rendererC = ref()
const pointsC = ref()
const pointsD = ref()
const cameraC = ref()
const satteliteC = ref()

function createCircleTexture(color, size) {
  var matCanvas = document.createElement('canvas');
  matCanvas.width = matCanvas.height = size;
  var matContext = matCanvas.getContext('2d');
  // create texture object from canvas.
  var texture = new THREE.Texture(matCanvas);
  // Draw a circle
  var center = size / 2;
  matContext.beginPath();
  matContext.arc(center, center, size/2, 0,  Math.PI, false);
  matContext.arc(center, center, size/2, Math.PI,  2*Math.PI, false);
  matContext.closePath();
  matContext.fillStyle = color;
  matContext.fill();
  // need to set needsUpdate
  texture.needsUpdate = true;
  // return a texture made from the canvas
  return texture;
}

const starMaterial = new PointsMaterial({
  size: .5,
  map: createCircleTexture('#ffffff', 256),
  transparent: true,
  depthWrite: false
});

const starMaterial2 = new PointsMaterial({
  size: 1,
  map: createCircleTexture('#ffffff', 256),
  transparent: true,
  depthWrite: false
});

const geometry = new BufferGeometry();
const geometry2 = new BufferGeometry();

const getRandomParticlePos = (particleCount) => {
  const arr = new Float32Array(particleCount * 3);
  for(let i = 0; i < particleCount; i++) {
    arr[i] = (Math.random() - .5) * 300;
  }
  return arr;
}

const getRandomParticlePos2 = (particleCount) => {
  const arr = new Float32Array(particleCount * 3);
  for(let i = 0; i < particleCount; i++) {
    arr[i] = (Math.random() - .5) * 400;
  }
  return arr;
}

geometry.setAttribute(
    "position",
    new BufferAttribute(getRandomParticlePos(4000),3)
);

geometry2.setAttribute(
    "position",
    new BufferAttribute(getRandomParticlePos2(4000),3)
);

onMounted(() => {
  const renderer = rendererC.value
  const points1 = pointsC.value
  const points2 = pointsD.value
  const camera = cameraC.value.camera
  const rendererRef = rendererC.value.renderer
  const sattelite = satteliteC.value.group


  points1.setMaterial(starMaterial)
  points1.setGeometry(geometry)

  points2.setMaterial(starMaterial2)
  points2.setGeometry(geometry2)

  function updateCamera() {
    camera.updateProjectionMatrix()
  }

  let time = 0

  renderer.onBeforeRender(() => {

    time++;

    points1.mesh.rotation.x -= .00064
    points2.mesh.rotation.x -= .0004

    sattelite.position.x = Math.cos( time/500 ) * 4 + 20;
    sattelite.position.y = Math.cos( time/500 ) * 5 + 10;
    sattelite.position.z = Math.cos( time/100 ) * 2 + 100;

    updateCamera()
  })
})
</script>

<style>

@import url('https://fonts.googleapis.com/css2?family=Comfortaa:wght@500&family=Rubik:wght@500&display=swap');

#navbar{
  font-family: 'Comfortaa', cursive;
  padding: 5vh 5vw;
  overflow: hidden;
  position: fixed;
  color: white;
  top: 0;
  width: 100vw;
  z-index: 100;
  background-color: transparent;
}

#htmlPage{
  overflow-x: hidden;
  position: absolute;
  background-color: black;
  color: white;
  font-size: 10vw;
  height: 100vh;
  width: 100vw;
}

#headline{
  margin: 30vh 0vw 30vh 40vw;
  width: 50vw;
  font-size: 50%;
  font-family: 'Rubik', sans-serif;
  justify-items: center;
}

#htmlPage2{
  font-family: 'Comfortaa', cursive;
  overflow-x: hidden;
  position: absolute;
  color: #fce5cd;
  margin-top: 100vh;
  padding: 0 10vw;
  font-size: 10vw;
  height: 100vh;
  width: 80vw;
  background-image: linear-gradient(black, rgb(48,120,164));
}

#mywork{
  font-family: 'Comfortaa', cursive;
}

#portfolio{
  display: inline-block;
  width: 100px;
  height: 100px;
  margin: 3vh 3vh;
  border: solid beige 1px;
  background-color: beige;
  border-radius: 5%;
}

#portPics{
  margin: 10px 15px 0 15px;
  width: 110px;
  height: 110px;
  border-radius: 5%;
  box-shadow: 3px 3px 5px black;
}

#description{
  color: white;
  width: 25vw;
  padding: 1vh 0vw;
  margin: 3vh 5px 0 0;
  font-size: 1vw;
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
  font-family: 'Comfortaa', cursive;
  overflow-x: hidden;
  overflow-y: hidden;
  position: absolute;
  background-image: linear-gradient(transparent, white 80%);
  color: black;
  margin: 220vh 0vw 0 0vw;
  font-size: 5vw;
  height: 80vh;
  width: 100vw;
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
  0% {scale: 100%; translate: 0 0; opacity: 0;}
  30% {opacity: 40}
  40% {opacity: 100}
  50% {opacity: 30}
  70% {opacity: 20}
  100% {scale: 160%; translate: 40vw 40vh; opacity: 0}
}

body {
  overflow-x: hidden;
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
  pointer-events: none;
}
</style>