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
      const axesHelper=new THREE.AxesHelper(10)
      this.scene.add(axesHelper)
    },
    createScene(){
      this.scene = new THREE.Scene();
    },
    createCaizhi(){
      //纹理贴图
      // const loader=new THREE.TextureLoader()
      // const loaderTexture=loader.load('./loader.jpg',function(){
      //   this.renderer.render(this.scene, this.camera);
      // })
      const material = new THREE.MeshNormalMaterial()
      
      this.mesh = new THREE.Mesh(new THREE.SphereGeometry(5, 30, 20), material);
      this.mesh.position.set(0,-3,0)
      
      const car=new THREE.Group()
      const material1=new THREE.MeshBasicMaterial({color:'#00FFFF'})
      this.mesh1=new THREE.Mesh(new THREE.BoxGeometry(0.25,0.25,0.5),material1)
      this.mesh1.position.set(0,2,0)
      this.mesh1.rotation.set(0,7.1,0)



      this.scene.add(this.mesh1,this.mesh,car)
    },
    createCamera(){
      this.container=document.querySelector('#container')
      this.camera=new THREE.PerspectiveCamera(
        90,
        this.container.clientWidth/this.container.clientHeight,
        1,
        1000
      )
      this.camera.position.set(2,5,2)
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
    createRenderer(){
      this.renderer=new THREE.WebGLRenderer();
      this.renderer.setSize(this.container.clientWidth,this.container.clientHeight)
      this.renderer.autoClear = false;
      this.renderer.setClearColor(0x000000, 0.0);
      this.renderer.shadowMap.enabled = true
      this.container.appendChild(this.renderer.domElement);
    },
    createCountrols(){
      // 开启鼠标缓震
      this.controls = new OrbitControls(this.camera, this.renderer.domElement);
      this.controls.enableDamping = true
    },
    // 粒子
    createSprites(){
      this.particle = new THREE.Object3D();
      this.liziGeometry = new THREE.TetrahedronGeometry(2, 0,1,1)
      this.liziMaterial = new THREE.MeshNormalMaterial({shading: THREE.FlatShading});
      for(var i=0;i<2000;i++){
        this.mesh3=new THREE.Mesh(this.liziGeometry,this.liziMaterial)
        this.mesh3.position.set(Math.random() - 0.5, Math.random() - 0.5, Math.random() - 0.5).normalize();
        this.mesh3.position.multiplyScalar(90 + (Math.random() * 700));
        this.mesh3.rotation.set(Math.random() * 2, Math.random() * 2, Math.random() * 2);
        this.particle.add(this.mesh3)
      }
      this.scene.add(this.particle)
    },
    // 动画
    animate() {
      this.controls.update()
      // 粒子转动
      this.particle.rotation.x += 0.0000;
      this.particle.rotation.y -= 0.0040;
      this.renderer.clear();
      this.renderer.render(this.scene, this.camera);
      requestAnimationFrame(this.animate);
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
