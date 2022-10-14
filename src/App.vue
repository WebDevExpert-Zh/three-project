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
      moveForward: false,
      moveLeft: false,
      moveBackward: false,
      moveRighta: false,
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
      this.createSprites()//创建粒子
      this.initMobile()// 键盘控制
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
      // 键盘控制物体移动
      if (this.moveForward) {
        //w
        this.box.position.y += 1;
      }
      this.box.rotation.y += 0.0080;
      
      this.mesh.rotation.y -= 0.0040;
      // 粒子转动
      this.particle.rotation.y += 0.0050;
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
      //纹理贴图 球体
      const loader=new THREE.TextureLoader().load(require('../public/loader.jpg'))
      const material = new THREE.MeshBasicMaterial({map:loader})
      this.mesh = new THREE.Mesh(new THREE.SphereGeometry(115, 30, 20), material);
      this.mesh.position.set(0,-3,0)
      //纹理贴图 方块
      this.box = new THREE.Object3D();
      const loader1=new THREE.TextureLoader().load(require('../public/color.jpg'))
      const material1=new THREE.MeshBasicMaterial({map:loader1})
      this.mesh1=new THREE.Mesh(new THREE.BoxGeometry(5,5,2),material1)
      this.mesh1.position.set(60,80,60)
      this.box.add(this.mesh1)
      this.scene.add(this.box,this.mesh)
    },
    // 键盘
    initMobile(){
      console.log(this.controls);
      var onKeyDown=function(event){
        switch(event.keyCode){
          case 87: // w
          this.moveForward = true;
          
          console.log(event.keyCode);
          break;
          
          case 65: // a
          this.moveLeft = true;
          console.log(event.keyCode);
          break;

          case 83: // s
          this.moveBackward = true;
          console.log(event.keyCode);
          break;

          case 68: // d
          this.moveRighta = true;
          console.log(event.keyCode);
          break;
        }
      };
      var onKeyUp=function (event) {
        switch (event.keyCode) {
          case 87: // w
          this.moveForward = false;
          console.log(event.keyCode);
          break;

          case 65: // a
          this.moveLeft = false;
          console.log(event.keyCode);
          break;

          case 83: // s
          this.moveBackward = false;
          console.log(event.keyCode);
          break;

          case 68: // d
          this.moveRighta = false;
          console.log(event.keyCode);
          break;

        }
      };
      this.mesh1=document.addEventListener('keydown', onKeyDown, false);
      this.mesh1=document.addEventListener('keyup', onKeyUp, false);
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
      this.scene.add(this.camera)
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
