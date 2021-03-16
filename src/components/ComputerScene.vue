<template>
  <div class="three-scene">
    <div id="computer-scene"></div>
  </div>
</template>

<script>
import * as THREE from 'three';
import { GLTFLoader } from 'three/examples/jsm/loaders/GLTFLoader';
import { OBJLoader } from 'three/examples/jsm/loaders/OBJLoader';
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls.js';
import {gsap, Power4, TimelineMax, easeOut} from 'gsap';

export default {
  name: 'ComputerScene',
  mounted() {
    // Render Size
    const renderSize = {
      width: window.innerWidth,
      height: window.innerHeight
    }
    // Scene Setup
    const scene = new THREE.Scene();
    const camera = new THREE.PerspectiveCamera( 75, renderSize.width / renderSize.height, 0.1, 1000 );
    const renderer = new THREE.WebGLRenderer();
    const container = document.querySelector('#computer-scene');
    let macbook, magicmouse, headphones;
    // scene.background = new THREE.Color( 0xe8e8e8);

    camera.position.z = 35;
    // camera.position.y = 10;

    renderer.setPixelRatio( Math.min(window.devicePixelRatio, 2) );
    renderer.setSize( renderSize.width, renderSize.height );

    container.appendChild( renderer.domElement );

    // Orbit Controls
    const controls = new OrbitControls( camera, renderer.domElement );
    // controls.maxPolarAngle = 0.9 * Math.PI / 2;
    controls.enableZoom = false;
    controls.enableDamping = true;

    /* Cursor */
    const cursor = {
      x: 0,
      y: 0
    }
    window.addEventListener('mousemove', (event) => {
      cursor.x = event.clientX / renderSize.width - 0.5;
      cursor.y = -(event.clientY / renderSize.height - 0.5);
    })

    // Object Loader
    const loader = new OBJLoader();

    /*
    *
    * Objects
    *
    */
    // Group
    const group = new THREE.Group();
    scene.add(group);

    // Macbook
    loader.load('./mpm_f21__Apple_MacBook_Pro_15.obj', function (obj) {
      macbook = obj;
      let objScale = .07;
      macbook.scale.x = objScale;
      macbook.scale.y = objScale;
      macbook.scale.z = objScale;
      macbook.position.y = - 10;
      group.add( macbook );
    });

    // Magic Mouse
    loader.load('./mpm_f19__Apple_Magic_Mouse.obj', function (obj) {
      magicmouse = obj;
      let objScale = .06;
      magicmouse.scale.x = objScale;
      magicmouse.scale.y = objScale;
      magicmouse.scale.z = objScale;
      // group.add( magicmouse );
    });

    // Headphones
    loader.load('./headphones.obj', function (obj) {
      headphones = obj;
      let objScale = 2.7;
      headphones.scale.x = objScale;
      headphones.scale.y = objScale;
      headphones.scale.z = objScale;
      // group.add( headphones );
    });

    // Sphere
    const geometry = new THREE.SphereGeometry( 5, 32, 32 );
    const material = new THREE.MeshStandardMaterial();
    const sphere = new THREE.Mesh( geometry, material );
    sphere.position.y = 2;
    // scene.add( sphere );

    // Flying Lights
    const sphereLightHelper = new THREE.SphereBufferGeometry( 0.5, 16, 8 );
    let light1, light2, light3, light4;

    light1 = new THREE.PointLight( 0xff0040, 1, 100 );
    // light1.add( new THREE.Mesh( sphereLightHelper, new THREE.MeshBasicMaterial( { color: 0xff0040 } ) ) );
    scene.add( light1 );

    // const sphereSize = 3;
    // const pointLightHelper = new THREE.PointLightHelper( light1, sphereSize );
    // scene.add( pointLightHelper );

    light2 = new THREE.PointLight( 0x0040ff, 1, 100 );
    // light2.add( new THREE.Mesh( sphereLightHelper, new THREE.MeshBasicMaterial( { color: 0x0040ff } ) ) );
    scene.add( light2 );

    light3 = new THREE.PointLight( 0x80ff80, 1, 100 );
    // light3.add( new THREE.Mesh( sphereLightHelper, new THREE.MeshBasicMaterial( { color: 0x80ff80 } ) ) );
    scene.add( light3 );

    light4 = new THREE.PointLight( 0xffaa00, 1, 100 );
    // light4.add( new THREE.Mesh( sphereLightHelper, new THREE.MeshBasicMaterial( { color: 0xffaa00 } ) ) );
    scene.add( light4 );


    // Directional Lights
    const lightTop = new THREE.DirectionalLight( 0xffffff, .5 );
    const lightBottom = new THREE.DirectionalLight( 0xffffff, .5 );
    // const lightFront = new THREE.DirectionalLight( 0xffffff, .2 );
    lightTop.position.y = 20;
    lightBottom.position.y = -20;
    scene.add( lightTop );
    scene.add( lightBottom );
    // scene.add( lightFront );

    // Light Helpers
    // const helper = new THREE.DirectionalLightHelper( lightBottom, 5 );
    // scene.add( helper );
    // const helper2 = new THREE.DirectionalLightHelper( lightTop, 5 );
    // scene.add( helper2 );
    // const helper3 = new THREE.DirectionalLightHelper( lightFront, 5 );
    // scene.add( helper3 );

    // Resize Event
    window.addEventListener('resize', () => {
      // Update Sizes
      renderSize.width  = window.innerWidth;
      renderSize.height = window.innerHeight;

      // Update Camera
      console.log(renderSize);
      camera.aspect = renderSize.width / renderSize.height;
      camera.updateProjectionMatrix();

      // Update Renderer
      renderer.setSize( renderSize.width, renderSize.height );
      renderer.setPixelRatio( Math.min(window.devicePixelRatio, 2) );
    })

    // Double Click Event
    window.addEventListener('dblclick', () => {
      // full screen
      console.log('double click');
      if (!document.fullscreenElement) {
        console.log('enter fullsceen');
      } else {
        console.log('leave fullscreen');
      }
    })

    // Time
    let t = Date.now();

    // Clock
    const clock = new THREE.Clock();

    // GSAP Animation
    // let tl = new TimelineMax({repeat: -1});
    // tl.to(sphere.position, {x: 10, duration: 1, ease: Power4.easeInOut})
    //   .to(sphere.position, {x: -10, duration: 1, ease: Power4.easeInOut});

    // RAF loop
    const animate = () => {

      window.requestAnimationFrame( animate );

      // Time
      const time = Date.now() * 0.0005;

      // Delta Time
      const currentTime = Date.now();
      const deltaTime = currentTime - t; // use delta time to control fps
      t = currentTime;

      // Clock
      const elapsedTime = clock.getElapsedTime();

      // Update Objects
      // macbook.rotation.y += 0.0004 * deltaTime;

      // magicmouse.position.x = Math.sin( time * 0.4 ) * 10;
      // magicmouse.position.y = Math.cos( time * 0.3 ) * 20;
      // magicmouse.position.z = Math.cos( time * 0.3 ) * 10;
      // magicmouse.rotation.x += 0.01;
      // magicmouse.rotation.y += 0.01;

      // headphones.position.x = Math.sin( time * 0.2 ) * -30;
      // headphones.position.y = Math.cos( time * 0.4 ) * 20;
      // headphones.position.z = Math.cos( time * 0.5 ) * 30;
      // headphones.rotation.x += 0.01;
      // headphones.rotation.y += 0.01;

      // Update Lights
      light1.position.x = Math.sin( time * 0.7 ) * 30;
      light1.position.y = Math.cos( time * 0.5 ) * 40;
      light1.position.z = Math.cos( time * 0.3 ) * 30;

      light2.position.x = Math.cos( time * 0.3 ) * 30;
      light2.position.y = Math.sin( time * 0.5 ) * 40;
      light2.position.z = Math.sin( time * 0.7 ) * 30;

      light3.position.x = Math.sin( time * 0.7 ) * 30;
      light3.position.y = Math.cos( time * 0.3 ) * 40;
      light3.position.z = Math.sin( time * 0.5 ) * 30;

      // light4.position.x = Math.sin( time * 0.3 ) * 30;
      // light4.position.y = Math.cos( time * 0.7 ) * 40;
      // light4.position.z = Math.sin( time * 0.5 ) * 30;
      light4.position.x = cursor.x * 10;
      light4.position.y = cursor.y * 10;

      // Update Camera

      // Update Controls
      controls.update();

      // Render
      renderer.render( scene, camera );

    };

    animate();
  },
  methods: {}
}
</script>

<style scoped lang="scss">
.three-scene {
  position: fixed;
  // z-index: -1;
  top: 0;
  left: 0;
}
</style>
