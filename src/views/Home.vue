<template>
  <div class="home">
    <!-- <img alt="Vue logo" src="../assets/logo.png"> -->
    <!-- <HelloWorld msg="Welcome to Your Vue.js + TypeScript App"/> -->
  </div>
</template>

<script lang="ts">
import { Component, Vue, Provide, Watch } from 'vue-property-decorator';
import HelloWorld from '@/components/HelloWorld.vue'; // @ is an alias to /src
import * as THREE from 'three';

@Component({
  components: {
    HelloWorld
  }
})
export default class Home extends Vue {
  @Provide() obj: Object = {};
  @Provide() name: string = 'jun';

  get computedMsg() {
    return 'hello ' + this.name;
  }

  mounted() {
    // this.runWait();

    // Initiate function or other initializations here
    this.init();
  }

  async init() {
    const width = window.innerWidth * 1;
    const height = window.innerHeight * 1;
    var scene = new THREE.Scene();
    var camera = new THREE.PerspectiveCamera(75, width / height, 0.1, 1000);

    var renderer = new THREE.WebGLRenderer();
    renderer.setSize(width, height);
    renderer.domElement.style.width = `${window.innerWidth}px`;
    renderer.domElement.style.height = `${window.innerHeight}px`;
    document.body.appendChild(renderer.domElement);

    // 创建模型
    var geometry = new THREE.BoxGeometry(1, 1, 1);
    // 创建材质
    var material = new THREE.MeshBasicMaterial({ color: 0x00ff00 });
    // 生产着色器
    var cube = new THREE.Mesh(geometry, material);
    scene.add(cube);

    camera.position.z = 5;
    renderer.setPixelRatio(window.devicePixelRatio);

    var animate = function() {
      requestAnimationFrame(animate);

      cube.rotation.x += 0.01;
      cube.rotation.y += 0.01;

      renderer.render(scene, camera);
    };

    animate();
  }

  async initLine() {
    var renderer = new THREE.WebGLRenderer();
    renderer.setSize(window.innerWidth, window.innerHeight);
    document.body.appendChild(renderer.domElement);

    var camera = new THREE.PerspectiveCamera(45, window.innerWidth / window.innerHeight, 1, 500);
    camera.position.set(0, 0, 20);
    camera.lookAt(0, 0, 0);

    var scene = new THREE.Scene();

    var material = new THREE.LineBasicMaterial({ color: 0x0000ff });

    var geometry = new THREE.Geometry();
    geometry.vertices.push(new THREE.Vector3(-10, 0, 0));
    geometry.vertices.push(new THREE.Vector3(0, 10, 0));
    geometry.vertices.push(new THREE.Vector3(10, 0, 0));

    var line = new THREE.Line(geometry, material);

    scene.add(line);
    renderer.setPixelRatio(window.devicePixelRatio);
    renderer.render(scene, camera);
  }

  async runWait() {
    await this.delay(2000);

    window.console.log('2');
  }

  async delay(ms: number) {
    return new Promise(resolve => setTimeout(resolve, ms));
  }

  @Watch('child')
  onChildChanged(val: string, oldVal: string) {}
}
</script>
