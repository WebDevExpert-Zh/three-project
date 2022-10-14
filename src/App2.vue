<template>
    <div>
      <div id="container"></div>
    </div>
  </template>
  
<script>
  import * as Three from '../../node_modules/three/build/three.module.js';
  //import * as Three from 'three'
  import {OrbitControls} from 'three/examples/jsm/controls/OrbitControls'
  import { PointerLockControls } from 'three/examples/jsm/controls/PointerLockControls.js';
  export default {
    name: 'ThreeTest',
    data() {
      return {
        camera: null,
        scene: null,
        renderer: null,
        mesh: null,
        controls:"",
        intersections:null,
        objects : [],
        clock:"",
        moveForward:false,
        moveLeft:false,
        moveBackward:false,
        moveRighta:false,
        direction : new Three.Vector3(),
        velocity : new Three.Vector3(),
        prevTime : performance.now()
      }
    },
    methods: {
      init: function() {
        this.initScene()//场景对象
        this.initCamera()//相机
        this.initWebGLRenderer()//渲染器
        this.initAxisHelper()//辅助坐标
        this.render()
        this.createControls()//控件对象
        this.Box()
        this.initControls()//相机视角
        this.initPlane()//地板
        this.initMobile()//移动
      },
      //鼠标控制移动相机视角*****
      initControls(){
        function onDocumentKeyDown(event) {
          var moveSpeed = 10;//速度
          if (event.keyCode == 87) {
            var vector = new THREE.Vector3(0, 0, 1.0).unproject(camera);//屏幕正中间对应的z==1的点
            var tempStep = vector.sub(camera.position).normalize();//得到方向
            var step = new THREE.Vector3(tempStep.x, tempStep.y, tempStep.z);
            camera.position.x += step.x * moveSpeed;
            camera.position.y += step.y * moveSpeed;
            camera.position.z += step.z * moveSpeed;
          }
        }
      },
      initMobile(){
        let that=this
        console.log(this.controls)
        var onKeyDown = function ( event ) {

          switch ( event.keyCode ) {

            case 38: // up
            case 87: // w
              that.moveForward = true;
              break;

            case 37: // left
            case 65: // a
              that.moveLeft = true;
              break;

            case 40: // down
            case 83: // s
              that.moveBackward = true;
              break;

            case 39: // right
            case 68: // d
              that.moveRighta = true;
              break;

          }

        };

        var onKeyUp = function ( event ) {

          switch ( event.keyCode ) {

            case 38: // up
            case 87: // w
              that.moveForward = false;
              break;

            case 37: // left
            case 65: // a
              that.moveLeft = false;
              break;

            case 40: // down
            case 83: // s
              that.moveBackward = false;
              break;

            case 39: // right
            case 68: // d
              that.moveRighta = false;
              break;

          }

        };

        document.addEventListener( 'keydown', onKeyDown, false );
        document.addEventListener( 'keyup', onKeyUp, false );
      },
      // 创建场景对象Scene
      initScene(){
        this.scene = new Three.Scene();
      },
      // 相机
      initCamera(){
        let container = document.getElementById('container');
        this.camera = new Three.PerspectiveCamera(60, container.clientWidth/container.clientHeight, 1, 1000);
        this.camera.position.set(292, 109, 268);//设置相机位置
        this.camera.lookAt(this.scene.position); //设置相机方向(指向的场景对象)
      },
      Box(){
        //正方形
        let geometry = new Three.BoxGeometry(50, 50, 50);
        let material = new Three.MeshNormalMaterial();
        this.mesh = new Three.Mesh(geometry, material);
        this.scene.add(this.mesh);
      },
      //地板
      initPlane(){
        var planeGeometry = new Three.PlaneGeometry(600, 600);
        //平面使用颜色为0xcccccc的基本材质
        var planeMaterial = new Three.MeshBasicMaterial({color: 0xcccccc});
        var plane = new Three.Mesh(planeGeometry, planeMaterial);
        //设置屏幕的位置和旋转角度
        plane.rotation.x = -0.5 * Math.PI;
        plane.position.x = 0;
        plane.position.y = 0;
        plane.position.z = 0;
        //将平面添加场景中
        this.scene.add(plane);
      },
      //创建渲染器对象
      initWebGLRenderer(){
        this.renderer = new Three.WebGLRenderer({antialias: true});
        this.renderer.setSize(container.clientWidth, container.clientHeight);//设置渲染区域尺寸
        this.renderer.setClearColor(0xb9d3ff, 1); //设置背景颜色
        container.appendChild(this.renderer.domElement);//body元素中插入canvas对象
      },
      //辅助三维坐标系AxisHelper
      initAxisHelper(){
        this.axisHelper = new Three.AxisHelper(250);
        this.scene.add(this.axisHelper);
      },
      render:function(){
        let that=this
        //this.mesh.rotation.x+=0.01
        requestAnimationFrame(that.render); //请求再次执行渲染函数render
        that.renderer.render(that.scene,that.camera);//执行渲染操作

        if (that.moveForward) {
          //w
          this.mesh.position.z -= 1;
        }
        if (that.moveBackward) {
          //s
          this.mesh.position.z += 1;
        }
        if (that.moveLeft) {
          this.mesh.position.x -= 1;
        }
        if (that.moveRighta) {
          this.mesh.position.x += 1;
        }
      },
      // 创建控件对象
      createControls () {
        this.controls = new OrbitControls(this.camera,this.renderer.domElement)
      }

    },
    mounted() {
      this.init();
      this.render();

    }
  }
</script>
<style scoped>
    #container {
      height: 400px;
    }
  </style>
  