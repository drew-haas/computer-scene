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

    // scene.background = new THREE.Color( 0xe8e8e8);

    // camera.position.y = 10;
    camera.position.z = 30;
    renderer.setPixelRatio( Math.min(window.devicePixelRatio, 2) );
    renderer.setSize( renderSize.width, renderSize.height );
    container.appendChild( renderer.domElement );

    // Orbit Controls
    const controls = new OrbitControls( camera, renderer.domElement );
    controls.enableZoom = false;
    controls.enableDamping = true;
    controls.maxPolarAngle = 0.9 * Math.PI / 2;

    // Cursor
    const cursor = {
      x: 0,
      y: 0
    }

    // Mouse Move Event
    window.addEventListener('mousemove', (event) => {
      cursor.x = event.clientX / renderSize.width - 0.5;
      cursor.y = -(event.clientY / renderSize.height - 0.5);
    })

    // Group
    const group = new THREE.Group();
    scene.add(group);

    // Object Loader
    // const loader = new OBJLoader();

    // Objects
    // let macbook, magicmouse, headphones, watch; // objects

    // Macbook
    // loader.load('./mpm_f21__Apple_MacBook_Pro_15.obj', function (obj) {
    //   macbook = obj;
    //   console.log(macbook)
    //   let objScale = .07;
    //   macbook.scale.x = objScale;
    //   macbook.scale.y = objScale;
    //   macbook.scale.z = objScale;
    //   macbook.position.y = - 10;
    //   group.add( macbook );
    // });

    // Magic Mouse
    // loader.load('./mpm_f19__Apple_Magic_Mouse.obj', function (obj) {
    //   magicmouse = obj;
    //   let objScale = .06;
    //   magicmouse.scale.x = objScale;
    //   magicmouse.scale.y = objScale;
    //   magicmouse.scale.z = objScale;
    //   group.add( magicmouse );
    // });

    // Headphones
    // loader.load('./headphones.obj', function (obj) {
    //   headphones = obj;
    //   let objScale = 2.7;
    //   headphones.scale.x = objScale;
    //   headphones.scale.y = objScale;
    //   headphones.scale.z = objScale;
    //   group.add( headphones );
    // });

    // Watch
    // loader.load('./nike-watch.obj', function (obj) {
    //   watch = obj;
    //   let objScale = .7;
    //   watch.scale.x = objScale;
    //   watch.scale.y = objScale;
    //   watch.scale.z = objScale;
    //   // watch.position.set(-15,0,0);
    //   // group.add( watch );
    // });

    // Stars Background
    const planeGeometry = new THREE.PlaneGeometry( 1, 1 );
    const starsTexture = new THREE.TextureLoader().load( './2k_stars_milky_way.jpg' );
    const planeMaterial = new THREE.MeshBasicMaterial( {map: starsTexture} );
    const plane = new THREE.Mesh( planeGeometry, planeMaterial );
    let planeScale = 300;
    plane.scale.x = planeScale;
    plane.scale.y = planeScale;
    plane.scale.z = planeScale;
    plane.position.z = -100;
    plane.rotation.x = -.07;
    scene.add( plane );

    // Spheres
    const jupiterTexture = new THREE.TextureLoader().load( './2k_jupiter_dark_purp.jpg' );
    const venusTexture = new THREE.TextureLoader().load( './2k_venus_surface_pink.jpg' );
    const marsTexture = new THREE.TextureLoader().load( './2k_mars_colorful.jpg' );

    // immediately use the texture for material creation
    const jupiterMaterial = new THREE.MeshStandardMaterial( { map: jupiterTexture } );
    const venusMaterial = new THREE.MeshStandardMaterial( { map: venusTexture } );
    const marsMaterial = new THREE.MeshStandardMaterial( { map: marsTexture } );

    const geometry = new THREE.SphereGeometry( 5, 32, 32 );
    // const material = new THREE.MeshStandardMaterial(); // TODO: add planet texture to sphere
    // const depthMaterial = new THREE.MeshLambertMaterial();

    let sphereScale = 1.5;
    let sphere3Scale = 7;
    const sphere = new THREE.Mesh( geometry, jupiterMaterial );
    const sphere2 = new THREE.Mesh( geometry, venusMaterial );
    const sphere3 = new THREE.Mesh( geometry, marsMaterial );

    sphere.position.y = 2;
    sphere.scale.x = sphereScale;
    sphere.scale.y = sphereScale;
    sphere.scale.z = sphereScale;

    sphere2.position.y = 0;
    sphere2.position.z = -10;
    sphere2.scale.x = .5;
    sphere2.scale.y = .5;
    sphere2.scale.z = .5;

    sphere3.scale.x = sphere3Scale;
    sphere3.scale.y = sphere3Scale;
    sphere3.scale.z = sphere3Scale;

    scene.add( sphere, sphere2, sphere3 );

    // Flying Lights
    const sphereLightHelper = new THREE.SphereBufferGeometry( 0.5, 16, 8 );

    let light1 = new THREE.PointLight( 0xff0040, .6, 100 ); // Red
    let light2 = new THREE.PointLight( 0x0040ff, .6, 100 ); // Blue
    let light3 = new THREE.PointLight( 0xea00ff, .6, 100 ); // Pink
    let light4 = new THREE.PointLight( 0xffffff, .6, 100 ); // Orange

    scene.add( light1, light2, light3, light4 );

    // Flying Light Helpers
    // light1.add( new THREE.Mesh( sphereLightHelper, new THREE.MeshBasicMaterial( { color: 0xff0040 } ) ) ); // Red
    // light2.add( new THREE.Mesh( sphereLightHelper, new THREE.MeshBasicMaterial( { color: 0x0040ff } ) ) ); // Blue
    // light3.add( new THREE.Mesh( sphereLightHelper, new THREE.MeshBasicMaterial( { color: 0xea00ff } ) ) ); // Pink
    // light4.add( new THREE.Mesh( sphereLightHelper, new THREE.MeshBasicMaterial( { color: 0xffffff } ) ) ); // Orange

    // light1.position.z = 10;
    light1.position.y = 10;
    light1.position.x = 5;

    // light2.position.z = 10;
    light2.position.y = -10;
    light2.position.x = -5;

    // light3.position.z = 10;
    light3.position.y = -10;
    light3.position.x = 5;

    // light4.position.z = 10;
    light4.position.y = 10;
    light4.position.x = -5;


    // Directional Lights
    const lightTop = new THREE.DirectionalLight( 0xffffff, .2 );
    const lightBottom = new THREE.DirectionalLight( 0xffffff, .2 );
    const lightLeft = new THREE.DirectionalLight( 0xffffff, .2 );
    const lightRight = new THREE.DirectionalLight( 0xffffff, .2 );
    const lightFront = new THREE.DirectionalLight( 0xffffff, 1 );

    lightTop.position.y = 20;
    lightBottom.position.y = -20;
    lightLeft.position.x = -30;
    lightRight.position.x = 30;
    lightFront.position.z = 20;

    scene.add( lightTop );
    scene.add( lightBottom );
    scene.add( lightLeft );
    scene.add( lightRight );
    scene.add( lightFront );

    // Directional Light Helpers
    // const helper = new THREE.DirectionalLightHelper( lightBottom, 5 );
    // scene.add( helper );
    // const helper2 = new THREE.DirectionalLightHelper( lightTop, 5 );
    // scene.add( helper2 );
    // const helper3 = new THREE.DirectionalLightHelper( lightFront, 5 );
    // scene.add( helper3 );
    // const helper4 = new THREE.DirectionalLightHelper( lightLeft, 5 );
    // scene.add( helper4 );
    // const helper5 = new THREE.DirectionalLightHelper( lightRight, 5 );
    // scene.add( helper5 );

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
    // const clock = new THREE.Clock();

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
      // const elapsedTime = clock.getElapsedTime();

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

      // Update Sphere
      // sphere.position.x = Math.sin( time * 1) * 30;
      // sphere.position.z = Math.cos( time * 1) * 30;
      sphere.rotation.y += 0.002;
      sphere.rotation.x += 0.000015;

      sphere2.position.x = Math.sin( time * .5) * -20;
      sphere2.position.z = Math.cos( time * .5) * 20;
      sphere2.position.y = Math.sin( time * .5) * 10;
      sphere2.rotation.y += 0.002;
      sphere2.rotation.x += 0.001;

      sphere3.position.x = Math.sin( time * .2) * 100;
      sphere3.position.z = Math.cos( time * .2) * 100;
      sphere3.rotation.y += 0.001;

      // Update Lights
      // light1.position.x = Math.sin( time * 0.7 ) * 30;
      // light1.position.y = Math.cos( time * 0.5 ) * 40;
      // light1.position.z = Math.cos( time * 0.3 ) * 30;

      // light2.position.x = Math.cos( time * 0.3 ) * 30;
      // light2.position.y = Math.sin( time * 0.5 ) * 40;
      // light2.position.z = Math.sin( time * 0.7 ) * 30;

      // light3.position.x = Math.sin( time * 0.7 ) * 30;
      // light3.position.y = Math.cos( time * 0.3 ) * 40;
      // light3.position.z = Math.sin( time * 0.5 ) * 30;

      // light4.position.x = Math.sin( time * 0.3 ) * 30;
      // light4.position.y = Math.cos( time * 0.7 ) * 40;
      // light4.position.z = Math.sin( time * 0.5 ) * 30;
      // light4.position.x = cursor.x * 10;
      // light4.position.y = cursor.y * 10;

      // Update Camera
      // camera.lookAt(watch.position);
      // camera.lookAt(sphere.position);

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
