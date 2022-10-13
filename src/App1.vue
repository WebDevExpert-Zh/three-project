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
        controls:null
      };
    },
    methods: {
      //初始化
      init() {
        //  创建场景对象Scene
        this.scene = new THREE.Scene();
  
        // //网格模型添加到场景中
        // let geometry = new THREE.SphereGeometry(0.1, 32,16);
        // let material = new THREE.MeshNormalMaterial({
        //   // color: "white"
        // });
        // this.mesh = new THREE.Mesh(geometry, material);
        // this.mesh.position.set(0,-0.08,0)
        // this.scene.add(this.mesh);
  
        // let geometry1=new THREE.BoxGeometry(0.0085,0.0085,0.0085)
        // let material1=new THREE.MeshBasicMaterial({color:'#00FFFF'})
        // this.mesh1=new THREE.Mesh(geometry1,material1)
        // this.mesh1.position.set(0,0.023,0)
        // this.scene.add(this.mesh1)
        // 粒子
        this.createSprites()
  
        /**
         * 相机设置
         */
        let container = document.getElementById("container");
        this.camera = new THREE.PerspectiveCamera(
          5,
          container.clientWidth / container.clientHeight,
          1,
          1000
        );
        this.camera.position.set(0,0.4,2)
        this.scene.add(this.camera)
  
        const axesHelper = new THREE.AxesHelper(2)
        this.scene.add(axesHelper)
  
        /**
         * 创建渲染器对象
         */
        this.renderer = new THREE.WebGLRenderer();
        this.renderer.setSize(container.clientWidth, container.clientHeight);
        container.appendChild(this.renderer.domElement);
  
        //创建控件对象 
        this.controls = new OrbitControls(this.camera, this.renderer.domElement);
        this.controls.enableDamping = true
      },
      // 粒子
      createSprites(){
        const liziMaterial  = new THREE.SpriteMaterial()
        for (let x = -5; x < 5; x++) {
          for (let y = -5; y < 5; y++) {
            //const particle = new THREE.Particle(material) // 创建粒子
            const liziParticle = new THREE.Sprite(liziMaterial) // 创建粒子
            liziParticle.position.set(x * 10, y * 10, 0)
            this.scene.add(liziParticle)
          }
        }
      },
      // 动画
      animate() {
        let clock=new THREE.Clock()
        let timer=clock.getElapsedTime()
        this.mesh.rotation.y=timer*0.5
        this.controls.update()
        // this.mesh.rotation.x += 0.01;
        // this.mesh.rotation.y += 0.02;
        this.renderer.render(this.scene, this.camera);
        requestAnimationFrame(this.animate);
      }
    },
    mounted() {
      this.init();
      this.animate();
    },
  };
  </script>
  
  <style>
  /* *{
    margin: 0;
    padding: 0;
  } */
  #container {
    position: absolute;
    width: 100%;
    height: 100%;
  }
  html {
    width: 100%;
    height: 100%;
    
    background: #11e8bb; /* Old browsers */
    background: -moz-linear-gradient(top,  #11e8bb 0%, #8200c9 100%);
    background: linear-gradient(to bottom,  #11e8bb 0%,#8200c9 100%); 
    overflow: hidden; 
  }
  
  body {
    margin: 0;
    padding: 0;
  }
  </style>
  