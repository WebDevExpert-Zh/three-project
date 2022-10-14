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
      // this.initMobile()// 键盘控制
      this.createLight()
      // const axesHelper=new THREE.AxesHelper(140)
      // this.scene.add(axesHelper)
      this.addFog()
    },
    createRenderer(){
      this.renderer=new THREE.WebGLRenderer({alpha: true});
      this.renderer.setSize(this.container.clientWidth,this.container.clientHeight)
      this.renderer.autoClear = false;
      this.renderer.setClearColor(0x000000, 1);
      // this.renderer.shadowMap.enabled = true
      this.renderer.shadowMapEnabled = true;
      this.container.appendChild(this.renderer.domElement);
    },
    // 动画
    animate() {
      this.controls.update()
      // 键盘控制物体移动
      // if (this.moveForward) {
      //   //w
      //   this.box.position.y += 1;
      // }
      this.box.rotation.y += 0.0080;
      // this.light.rotation.y += 0.0080;
      this.meshGeometry.rotation.y -= 0.0040;
      this.meshYun.rotation.y -= 0.0040;
      // 粒子转动
      this.particle.rotation.y -= 0.0050;
      this.renderer.clear();
      this.renderer.render(this.scene, this.camera);
      requestAnimationFrame(this.animate);
    },
    
    createScene(){
      this.scene = new THREE.Scene();
    },
    addFog(){
      this.scene.fog = new THREE.Fog('#00CCFF', 1, 900)
    },
    // 粒子
    createSprites() {
      //轨道
      this.geometry=new THREE.RingGeometry(175.9,176,70)
      this.materia3=new THREE.MeshBasicMaterial({color:''})
      this.mesh4=new THREE.Mesh(this.geometry,this.materia3)
      this.mesh4.rotation.x=Math.PI*0.5
      this.mesh4.position.set(0,2,0)
      this.scene.add(this.mesh4)
      //太空
      this.particle = new THREE.Object3D();
      this.liziGeometry = new THREE.IcosahedronGeometry(1, 0)
      this.liziMaterial = new THREE.MeshNormalMaterial({ shading: THREE.FlatShading });
      for (let i = 0; i < 2000; i++) {
        this.meshLizi = new THREE.Mesh(this.liziGeometry, this.liziMaterial)
        this.meshLizi.position.set(Math.random() - 0.5, Math.random() - 0.5, Math.random() - 0.5).normalize();
        this.meshLizi.position.multiplyScalar(20 + (Math.random() * 1000));
        this.meshLizi.rotation.set(Math.random() * 1, Math.random() * 1, Math.random() *1);
        this.particle.add(this.meshLizi)
      }
      this.scene.add(this.particle)
    },
    createCaizhi(){
      //纹理贴图 球体
      const loaderGeometry=new THREE.TextureLoader().load(require('../public/2.jpg'))
      const materialGeometry = new THREE.MeshStandardMaterial({map:loaderGeometry})
      this.meshGeometry = new THREE.Mesh(new THREE.SphereGeometry(125, 30, 20), materialGeometry);
      this.meshGeometry.position.set(0,-3,0)

      // 云层
      const loaderYun=new THREE.TextureLoader().load(require('../public/云层.png'))
      const materiaYun = new THREE.MeshBasicMaterial({map:loaderYun,transparent: true,opacity: 0.25})
      this.meshYun = new THREE.Mesh(new THREE.SphereGeometry(135, 30, 20), materiaYun);

      //月球
      this.box = new THREE.Object3D();
      const loaderBox=new THREE.TextureLoader().load(require('../public/4.jpg'))
      const materialBox=new THREE.MeshBasicMaterial({map:loaderBox})
      this.meshBox=new THREE.Mesh(new THREE.SphereGeometry(8,30, 20),materialBox)
      this.meshBox.position.set(60,0,165)
      this.box.add(this.meshBox)
      this.scene.add(this.box,this.meshGeometry,this.meshYun,this.mesh3)
    },
    createCamera(){
      this.container=document.querySelector('#container')
      this.camera=new THREE.PerspectiveCamera(
        90,
        this.container.clientWidth/this.container.clientHeight,
        1,
        1000
      )
      this.camera.position.set(180,-20,120)
      this.scene.add(this.camera)
    },
    //灯光
    createLight(){
      this.light = new THREE.Object3D();
      this.lights = [];
      this.lights[1] = new THREE.DirectionalLight('#ffffff', 1.75)
      this.lights[1].position.set(0.25, 3, - 1.25)
      this.lights[2] = new THREE.DirectionalLight('#ffffff', 0.3)
      this.lights[2].position.set(-0.25, 1,  1.25)
      this.light.add(this.lights[1],this.lights[2])
      this.scene.add(this.light)
    },
    createCountrols(){
      // 开启鼠标缓震
      this.controls = new OrbitControls(this.camera, this.renderer.domElement);
      this.controls.enableDamping = true
    },
    // 键盘
    // initMobile(){
    //   console.log(this.controls);
    //   var onKeyDown=function(event){
    //     switch(event.keyCode){
    //       case 87: // w
    //       this.moveForward = true;
          
    //       console.log(event.keyCode);
    //       break;
          
    //       case 65: // a
    //       this.moveLeft = true;
    //       console.log(event.keyCode);
    //       break;

    //       case 83: // s
    //       this.moveBackward = true;
    //       console.log(event.keyCode);
    //       break;

    //       case 68: // d
    //       this.moveRighta = true;
    //       console.log(event.keyCode);
    //       break;
    //     }
    //   };
    //   var onKeyUp=function (event) {
    //     switch (event.keyCode) {
    //       case 87: // w
    //       this.moveForward = false;
    //       console.log(event.keyCode);
    //       break;

    //       case 65: // a
    //       this.moveLeft = false;
    //       console.log(event.keyCode);
    //       break;

    //       case 83: // s
    //       this.moveBackward = false;
    //       console.log(event.keyCode);
    //       break;

    //       case 68: // d
    //       this.moveRighta = false;
    //       console.log(event.keyCode);
    //       break;

    //     }
    //   };
    //   this.mesh1=document.addEventListener('keydown', onKeyDown, false);
    //   this.mesh1=document.addEventListener('keyup', onKeyUp, false);
    // },
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
  /* background: #074cfb;
  background: -moz-linear-gradient(top,  #074cfb 0%, #a7bdd7 100%); 
  background: linear-gradient(to bottom,  #074cfb 0%,#a7bdd7 100%); */
}

</style>
