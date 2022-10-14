<template>
  <div>
    <div id="container"></div>
  </div>
</template>

<script>
import * as THREE from "three";
import {OrbitControls} from 'three/examples/jsm/controls/OrbitControls.js';
export default {
  data() {
    return {
      camera: null,
      scene: null,
      renderer: null,
      mesh: null,
      controls:null,
      particle:null
    };
  },
  mounted() {
    this.init();
    this.animate();
  },
  methods: {
    init() {
      this.createScene()//创建场景
      this.createCaizhi()
      this.createCamera()//创建相机
      this.createRenderer()//创建渲染器
      this.createCountrols()//创建控物对象
      this.createLight()//创建光源
      this.createSprites()//创建粒子
      
      const axesHelper=new THREE.AxesHelper(140)
      this.scene.add(axesHelper)
    },
    createRenderer(){
      this.renderer=new THREE.WebGLRenderer();
      this.renderer.setSize(this.container.clientWidth,this.container.clientHeight)
      this.renderer.autoClear = false;
      this.renderer.setClearColor(0x000000, 0.0);
      this.renderer.shadowMap.enabled = true
      this.container.appendChild(this.renderer.domElement);
    },
    // 动画
    animate() {
      this.controls.update()
      this.mesh.rotation.y -= 0.0040;
      // 粒子转动
      this.particle.rotation.x += 0.0020;
      this.particle.rotation.y -= 0.0050;
      this.particle.rotation.z -= 0.0050;
      this.renderer.clear();
      this.renderer.render(this.scene, this.camera);
      requestAnimationFrame(this.animate);
    },
    createScene(){
      this.scene = new THREE.Scene();
    },
    // 粒子
    createSprites() {
      this.particle = new THREE.Object3D();
      this.liziGeometry = new THREE.TetrahedronGeometry(2, 0)
      this.liziMaterial = new THREE.MeshNormalMaterial({ shading: THREE.FlatShading });
      for (var i = 0; i < 2000; i++) {
        this.mesh3 = new THREE.Mesh(this.liziGeometry, this.liziMaterial)
        this.mesh3.position.set(Math.random() - 0.5, Math.random() - 0.5, Math.random() - 0.5).normalize();
        this.mesh3.position.multiplyScalar(90 + (Math.random() * 700));
        this.mesh3.rotation.set(Math.random() * 1, Math.random() * 1, Math.random() *1);
        this.particle.add(this.mesh3)
      }
      this.scene.add(this.particle)
    },
    createCaizhi(){
      //纹理贴图
      const loader=new THREE.TextureLoader().load(require('../public/loader.jpg'))
      const material = new THREE.MeshBasicMaterial({map:loader})
      
      this.mesh = new THREE.Mesh(new THREE.SphereGeometry(115, 30, 20), material);
      this.mesh.position.set(0,-3,0)
      
      const loader1=new THREE.TextureLoader().load(require('../public/color.jpg'))
      const material1=new THREE.MeshBasicMaterial({map:loader1})
      this.mesh1=new THREE.Mesh(new THREE.BoxGeometry(5,5,2),material1)
      this.mesh1.position.set(60,80,60)
      this.mesh1.rotation.set(0,17.1,0)



      this.scene.add(this.mesh1,this.mesh)
    },
    createCamera(){
      this.container=document.querySelector('#container')
      this.camera=new THREE.PerspectiveCamera(
        70,
        this.container.clientWidth/this.container.clientHeight,
        1,
        1000
      )
      this.camera.position.set(120,150,120)
      this.camera.lookAt(this.mesh1.position)
      this.scene.add(this.camera)
    },
    // 灯光
    createLight(){
      this.lights = [];
      this.lights[0] = new THREE.DirectionalLight(0xffffff, 2);
      this.lights[0].position.set(1, 0, 0);
      this.lights[1] = new THREE.DirectionalLight(0xffffff, 2);
      this.lights[1].position.set(0.75, 1, 0.5)
      this.lights[2] = new THREE.DirectionalLight(0xffffff, 2);
      this.lights[2].position.set(-0.75, -1, 0.5);
      this.scene.add(this.lights[0]);
      this.scene.add(this.lights[1]);
      this.scene.add(this.lights[2]);
    },
    createCountrols(){
      // 开启鼠标缓震
      this.controls = new OrbitControls(this.camera, this.renderer.domElement);
      this.controls.enableDamping = true
    }
  }
};
</script>

<style>
*{
  margin: 0;
  padding: 0;
}
#container {
  position: absolute;
  width: 100%;
  height: 100%;
  background: #074cfb;
  background: -moz-linear-gradient(top,  #074cfb 0%, #a7bdd7 100%); /* FF3.6-15 */
  background: linear-gradient(to bottom,  #074cfb 0%,#a7bdd7 100%);
}

</style>
