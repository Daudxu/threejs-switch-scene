<script setup>
import * as THREE from 'three'
import  Stats  from "three/examples/jsm/libs/stats.module"
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls.js"

import { onMounted, ref } from 'vue'
const sceneType = ref(0)

let scene1, scene2, scene3

onMounted (()=>{
    // 创建场景
    scene1 = new THREE.Scene();
    scene2 = new THREE.Scene();
    scene3 = new THREE.Scene();
    // 创建相机
    const camera = new THREE.PerspectiveCamera(
      75,
      window.innerWidth / window.innerHeight,
      0.1,
      1000
    )
    camera.position.set(0, 5, 10)

    // 创建渲染器
    const container = document.getElementById("container");
    const renderer = new THREE.WebGL1Renderer({ antialias: true })
    renderer.setPixelRatio(window.devicePixelRatio);
    renderer.setSize(window.innerWidth, window.innerHeight);
    renderer.shadowMap.enabled = true;
    renderer.shadowMap.type = THREE.VSMShadowMap;
    renderer.outputEncoding = THREE.sRGBEncoding;
    renderer.toneMapping = THREE.ACESFilmicToneMapping;
    container.appendChild(renderer.domElement)
    
    const stats = new Stats();
    stats.domElement.style.position = "absolute";
    stats.domElement.style.top = '0px';
    container.appendChild(stats.domElement);
    
    const controls = new OrbitControls(camera, renderer.domElement);
    controls.target.set(0, 0, 0);


    scene1.add(createPlane());
    scene1.add(createLight());
    scene1.add(createBox());

    scene2.add(createPlane());
    scene2.add(createLight());
    scene2.add(createSphere());

    scene3.add(createPlane());
    scene3.add(createLight());
    scene3.add(createCylinder());
  
    function animate() {
      if(sceneType.value === 2){
        renderer.render(scene2, camera);
      }else if(sceneType.value === 3){
        renderer.render(scene3, camera);
      }else{
        renderer.render(scene1, camera);
      }

      stats.update();
      controls.update();
      requestAnimationFrame(animate);
    }
    animate();

    

});


const createBox = () => {
  const geometry = new THREE.BoxGeometry( 1, 1, 1 );
  const material = new THREE.MeshBasicMaterial( {color: 0x00ff00} );
  const cube = new THREE.Mesh( geometry, material );
  return cube
}

const createSphere = () => {
  const geometry = new THREE.CapsuleGeometry( 1, 1, 4, 8 );
  const material = new THREE.MeshBasicMaterial( {color: 0x00ff00} );
  const capsule = new THREE.Mesh( geometry, material );
  return capsule
}

const createCylinder = () => {
  const geometry = new THREE.CylinderGeometry( 1, 1, 1, 8);
  const material = new THREE.MeshBasicMaterial( {color: 0xffff00} );
  const cylinder = new THREE.Mesh( geometry, material );
  return cylinder
}
const createPlane = () => {
    // 创建一个平面
    const planeGeometry = new THREE.PlaneGeometry(20, 20, 1, 1);
    const planMaterial = new THREE.MeshBasicMaterial({
      color: 0xffffff,
      side: THREE.DoubleSide
    });
    const plane = new THREE.Mesh(planeGeometry, planMaterial);
    plane.receiveShadow = true;
    plane.rotation.x = -Math.PI / 2;
    plane.position.y = -0.5;
    plane.updateMatrixWorld()
    return plane
}
const createLight = () => {
  //创建灯光
  const light = new THREE.SpotLight(0xffffff, 1, 100, Math.PI / 6, 0.5);
  light.position.set(10, 30, 10);
  light.castShadow = true;
  return light
}

const handleClick = (type) => {
  sceneType.value = type
}


</script>

<template>
  <div id="container">
  </div>
  <div class="cl-switch">
    <button @click="handleClick(1)">scene1</button>
    <button @click="handleClick(2)">scene2</button>
    <button @click="handleClick(3)">scene3</button>
  </div>
</template>

<style scoped>
.cl-switch {
  position: fixed;
  top: 0;
  right: 0;
  z-index: 999;
  background: #ffffff;
}
/* .logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
} */
</style>
