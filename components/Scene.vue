<template>
  <canvas ref="canvas" />
</template>

<style>
  canvas {
    position: absolute;
    top: 0;
    left: 0;
    z-index: -1;
  }
</style>

<script>
import * as THREE from 'three';
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls";

export default {
  name: 'Scene',
  data() {
    return {
      scene: new THREE.Scene(),
      camera: new THREE.PerspectiveCamera( 75, window.innerWidth / window.innerHeight, 0.1, 1000 ),
      renderer: null,
      cube: new THREE.Object3D(),
      controls: null,
    }
  },
  mounted() {
    this.$nextTick(() => {
      this.init();
    });
  },
  methods: {
    init() {
      const {canvas} = this.$refs;
      this.scene.background = new THREE.Color(0xababab);
      this.renderer = new THREE.WebGLRenderer({ canvas });
      this.renderer.setSize( window.innerWidth, window.innerHeight );
      this.camera.position.z = 3;

      const light = new THREE.HemisphereLight( 0xffffff, 0x020202, 2 );
      this.scene.add( light );

      this.controls = new OrbitControls( this.camera, canvas);
      this.controls.update();

      this.createCube();
      this.mainLoop();
    },
    createCube() {
      const geometry = new THREE.BoxGeometry();
      const material = new THREE.MeshPhongMaterial( { color: 0xffffff } );
      this.cube = new THREE.Mesh( geometry, material );
      this.scene.add(this.cube);
    },
    mainLoop() {
      this.cube.rotation.x += 0.01;
      this.cube.rotation.y += 0.01;

      this.controls.update();

      this.renderer.render(this.scene, this.camera);
      requestAnimationFrame(this.mainLoop);
    },
  }
}
</script>
