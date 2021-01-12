<!--suppress ALL -->
<template>
  <div id="model" ref="model">
    <div id="container"></div>
    <div class="nav-top">
      <span class="nav-top-title">港口可视化平台</span>
    </div>

    <!--总指标-->
    <div id="zongzhibiao" class="tip-container" style="width: 270px; position: fixed; top: 90px; left: 16px;">
      <div class="tip-title">
        <span><img src="../../../static/icon/总指标.png"></span>
        <span>当前设备</span>
        <span class="tip-dropDown"><img src="../../../static/icon/收起.png" @click="show = !show"></span>
      </div>
      <div class="tip-content" style="width: 270px;" v-if="show">
        <ul>
          <li>
            <span class="zhzb-li-name">当前设备<span class="typeStyle">（台）</span></span>
            <span class="zhzb-li-num">{{selectedName}}</span>
          </li>
          <li>
            <span class="zhzb-li-name">运行时长<span class="typeStyle">（小时）</span></span>
            <span class="zhzb-li-num">xxxx</span>
          </li>
          <li>
            <span class="zhzb-li-name">维修及时率<span class="typeStyle">（%）</span></span>
            <span class="zhzb-li-num">xxxx</span>
          </li>
          <li><span class="zhzb-li-name">维修完成率<span class="typeStyle">（%）</span></span>
            <span class="zhzb-li-num">xxxx</span>
          </li>
        </ul>
      </div>
    </div>

    <!--预警-->
    <div id="yujing" class="tip-container" style="width: 270px; position: fixed; top: 290px; left: 16px;">
      <div class="tip-title">
        <span><img src="../../../static/icon/预警.png"></span>
        <span>设备总体情况</span>
        <span class="tip-dropDown"><img src="../../../static/icon/收起.png" @click="show2 = !show2"></span>
      </div>
      <div class="tip-content" style="width: 270px;" v-if="show2">
        <ul>
          <li>
            <span class="zhzb-li-name">总设备数<span class="typeStyle">（台）</span></span>
            <span class="zhzb-li-num"><span></span><span class="font-w yj-span">{{options.length}}</span></span>
          </li>
          <li>
            <span class="zhzb-li-name">xxxx</span>
            <span class="zhzb-li-num"><span><img src="../../../static/icon/上升趋势.png"></span>
    <span class="font-w yj-span">xxxx</span></span>
          </li>
          <li>
            <span class="zhzb-li-name">xxxx</span>
            <span class="zhzb-li-num"><span><img src="../../../static/icon/下降趋势.png"></span>
    <span class="font-w yj-span">xxxx</span></span></li>
        </ul>
      </div>
    </div>

    <!--考勤-->
    <div id="kaoqin" class="tip-container" style="width: 270px; position: fixed; top: 460px; left: 16px;">
      <div class="tip-title">
        <span><img src="../../../static/icon/考勤.png"></span><span>工具</span><span
        class="tip-dropDown">
    <img src="../../../static/icon/收起.png" @click="show3 = !show3"></span>
      </div>
      <div class="tip-content" style="width: 270px;" v-if="show3">
        <ul>
          <li><span class="zhzb-li-name">浏览</span><span class="zhzb-li-num"></span></li>
          <li>
            <el-select v-model="value" placeholder="请选择" @change="change" size="small">
              <el-option
                v-for="item in options"
                :key="item.value"
                :label="item.label"
                :value="item.value">
              </el-option>
            </el-select>
          </li>
          <li><span class="zhzb-li-name">旋转</span><span class="zhzb-li-num"></span></li>
          <li>
            <el-switch
              v-model="flag"
              active-color="#13ce66"
              inactive-color="#ff4949"
              active-text="开"
              inactive-text="关"
            >
            </el-switch>
          </li>
        </ul>
      </div>
    </div>
    <div id="name-box"></div>


    <!--总指标-->
    <div id="Sprite" class="tip-container" style="width: 270px; position: fixed; top: 90px; left: 16px;">
      <div class="tip-title">
        <span><img src="../../../static/icon/总指标.png"></span>
        <span>Three.js Demo</span>
        <span class="tip-dropDown"><img src="../../../static/icon/收起.png" @click="show = !show"></span>
      </div>
      <div class="tip-content" style="width: 270px;" v-if="show">
        <ul>
          <li>
            <span class="zhzb-li-name">xxxx<span class="typeStyle">（xx）</span></span>
            <span class="zhzb-li-num">xxxxx</span>
          </li>
          <li>
            <span class="zhzb-li-name">xxxxx<span class="typeStyle">（xx）</span></span>
            <span class="zhzb-li-num">xxxx</span>
          </li>
          <li>
            <span class="zhzb-li-name">xxxx<span class="typeStyle">（xx）</span></span>
            <span class="zhzb-li-num">xxxx</span>
          </li>
          <li><span class="zhzb-li-name">xxxx<span class="typeStyle">（xx）</span></span>
            <span class="zhzb-li-num">xxxx</span>
          </li>
        </ul>
      </div>
    </div>


  </div>
</template>
<script>
  import * as THREE from "three";
  import {OBJLoader, MTLLoader} from 'three-obj-mtl-loader';

  /*loader*/
  import {DRACOLoader} from 'three/examples/jsm/loaders/DRACOLoader';
  import {GLTFLoader} from 'three/examples/jsm/loaders/GLTFLoader';
  import {TTFLoader} from 'three/examples/jsm/loaders/TTFLoader';


  import TWEEN from "@tweenjs/tween.js"; // 动画
  import {Loading} from 'element-ui';
  import * as d3Scale from 'd3-scale'
  import html2canvas from 'html2canvas';


  // bloom
  // EffectComposer 效果合成器
  // 1. 导入相关的后期处理库
  import {EffectComposer} from "three/examples/jsm/postprocessing/EffectComposer.js";
  // 4.  配置后期处理过程链 一般RenderPass都是过程链的开始
  import {RenderPass} from "three/examples/jsm/postprocessing/RenderPass.js";
  // 模拟生活中的泛光或说眩光效果
  import {UnrealBloomPass} from "three/examples/jsm/postprocessing/UnrealBloomPass.js";

  import {MaskPass, ClearMaskPass} from "three/examples/jsm/postprocessing/MaskPass.js";
  import {ClearPass} from "three/examples/jsm/postprocessing/ClearPass.js";
  import {ShaderPass} from "three/examples/jsm/postprocessing/ShaderPass.js";

  import {CopyShader} from "three/examples/jsm/shaders/CopyShader.js";
  import {FXAAShader} from "three/examples/jsm/shaders/FXAAShader.js";
  import {LuminosityHighPassShader} from "three/examples/jsm/shaders/LuminosityHighPassShader.js";


  const OrbitControls = require('three-orbit-controls')(THREE);

  export default {
    name: "threeMap",
    data() {
      return {
        fov: 60,
        angle: 1,
        flag: false,
        options: [
          {value: "CK", label: "卡车", x: "100", y: "100", z: "0"},
          {value: "YGC", label: "油罐车", x: "200", y: "200", z: "0"},
          {value: "DGJ", label: "登高机", x: "300", y: "300", z: "30"},
          {value: "DuiGJ", label: "堆高机", x: "0", y: "-500", z: "10"},
          {value: "YXTZQ", label: "箱门调转区", x: "-600", y: "-600", z: "150"},
          {value: "ZMD", label: "正面吊", x: "400", y: "400", z: "10"},
          {value: "ZDHGDD", label: "自动化轨道吊", x: "-100", y: "100", z: "10"},
          {value: "DQ", label: "吊桥", x: "-200", y: "200", z: "0"},
          {value: "QYC1", label: "牵引车1", x: "-300", y: "300", z: "30"},
          {value: "QYC2", label: "牵引车2", x: "-400", y: "400", z: "30"},
          {value: "QYC3", label: "牵引车3", x: "-500", y: "500", z: "30"},
          {value: "MJ", label: "门机", x: "200", y: "-200", z: "10"},
          {value: "MJ10T", label: "门机10T", x: "300", y: "-300", z: "10"},
          {value: "MJ16T", label: "门机16T", x: "400", y: "-400", z: "10"},
          {value: "LD", label: "龙吊", x: "-100", y: "-100", z: "10"},
          {value: "DC", label: "吊车", x: "-200", y: "-200", z: "10"},
          {value: "XZCL", label: "行政车辆", x: "-300", y: "-300", z: "10"},
          {value: "AVG", label: "AVG", x: "-400", y: "-400", z: "10"},
          {value: "ZDHDC", label: "自动化桥吊", x: "0", y: "0", z: "10"}
        ],
        value: '',
        inputX: '',
        inputY: '',
        selectOpacity: 1.0,
        selected: true,
        selectedName: '未知',
        show: true,
        show2: true,
        show3: true,
        swithSize: {
          'CK': "0.02",
          "YGC": "0.02",
          "DGJ": "0.015",
          "DuiGJ": "0.02",
          "YXTZQ": "0.02",
          "ZMD": "0.02",
          "ZDHGDD": "0.013",
          "DQ": "0.03",
          "QYC1": "0.015",
          "QYC2": "0.015",
          "QYC3": "0.015",
          "MJ": "4",
          "MJ10T": "0.008",
          "MJ16T": "0.05",
          "LD": "0.08",
          "DC": "15",
          "XZCL": "0.3",
          "AVG": "0.025",
          "ZDHDC": "0.03"
        },
        FPS: 30, // 设置渲染频率为30FBS，也就是每秒调用渲染器render方法大约30次
        renderT: 1 / 30, //单位秒  间隔多长时间渲染渲染一次
        timeS: 0
      }
    },
    created() {
      // 把精灵文字的模板从body元素中移除
      // this.$refs.model.removeChild(document.querySelector("#Sprite"));
    },
    mounted() {
      this.init();

      /*一个一个加入obj 开始*/
      // for (let i = 0; i < this.options.length; i++) {
      //   this.addObj(this.options[i]['value'], this.options[i]['label'],
      //     this.options[i]['x'], this.options[i]['y']);
      // }
      /*一个一个加入obj 结束*/

      /*用promise方式加载模型 开始*/
      let pro = [];
      for (let i = 0; i < this.options.length; i++) {
        let p = this.addObjPromisePic(this.options[i]['value'], this.options[i]['label'],
          this.options[i]['x'], this.options[i]['y'], this.options[i]['z']);
        pro.push(p);
      }
      this.loadingInstance = Loading.service({
        fullscreen: true,
        text: '加载模型中....',
        background: "rgba(31,86,185,0.25)"
      });
      Promise.all(pro).then((ok) => {
        // console.log(ok);
        this.loadingInstance.close(); // 关闭loading
        this.animate();
      });
      pro = null; /*手动置空*/
      /*用promise方式加载模型 结束*/


      // {value: "DGJ", label: "堆高机", x: "300", y: "300"},
      // this.addObjPic("DGJ", "堆高机", "300", "300");
      // this.render();

    },
    beforeDestroy() {
      this.scene = null;
      this.light = null;
      this.camera = null;
      this.control = null;
      this.renderer = null;
      this.params = null;
      this.box3 = null;
      this.lastSelect = null;
      this.nowSelect = null;
      this.labelRenderer = null;
      this.loadingInstance = null;
      this.dracoLoader = null; // dracoLoader实例
      cancelAnimationFrame(this.req);
      // console.log("实例已经被销毁");
    },
    methods: {
      async init() {
        let self = this;
        this.scene = null;
        this.light = null;
        this.camera = null;
        this.control = null;
        this.renderer = null;
        this.req = null;

        this.lastSelect = null;
        this.nowSelect = null;

        this.labelRenderer = null;
        this.box3 = new THREE.Box3();

        this.loadingInstance = null; // 加载实例
        /*dracoLoader实例初始化 开始*/
        this.dracoLoader = null; // dracoLoader实例
        this.dracoLoader = new DRACOLoader();
        this.dracoLoader.setDecoderPath("../../../static/draco/"); // 最后要加/
        this.dracoLoader.preload();
        /*dracoLoader实例初始化 结束*/
        this.itemList = []; // 存放raycaster检测对象

        this.raycaster = new THREE.Raycaster();
        this.mouse = new THREE.Vector2(1, 1);
        this.mousePosition = {
          x: 0,
          y: 0
        };
        this.clock = new THREE.Clock();

        this.scene = new THREE.Scene();
        this.normalScene = new THREE.Scene(); // 地球


        /*后期处理效果 开始*/
        // 后期处理
        // 创建效果处理器
        // const {bloomComposer, finalComposer} = this.createComposer();

        /*后期处理效果 结束*/


        /*生成圆角矩形地面 开始*/
        const shape = this.createArcRect(2000, 2000, 100);
        const extrudeSettings = {
          steps: 64,
          depth: 1, // step设置为1，保证侧面只有一个平面，如果想更高，可以通过scale放大
          bevelEnabled: false,
        };
        const groundGeo = new THREE.ExtrudeGeometry(shape, extrudeSettings);
        let groundMat = new THREE.MeshBasicMaterial({
          opacity: 1.0,
          color: "rgb(159, 161, 162)",
          transparent: true,
          side: THREE.DoubleSide
        });
        let ground = new THREE.Mesh(groundGeo, groundMat);
        ground.position.y = 0;
        ground.rotation.x = -Math.PI / 2;

        this.initConfig(ground, {
          position: {x: -1000, z: 1000}
        })
        this.scene.add(ground);
        /*生成圆角矩形地面 结束*/


        /*文字精灵  开始*/
        const {sprite: spriteText1} = await this.createSpriteText("#Sprite", {
          position: {x: -1000, z: 250, y: 450},
          rotation: {x: Math.PI / 2}
        }); // 精灵文字
        // console.log(spriteText1);
        this.scene.add(spriteText1);
        this.$refs.model.removeChild(document.querySelector("#Sprite"));
        /* 文字精灵 结束*/

        /*添加光圈 开始*/
        let beam = this.createLightBeam(2000, 2000, 100, 'rgb(100,149,237)', {
          position: {x: -1000, y: 0, z: 1000},
          rotation: {x: -Math.PI / 2}
        });
        this.scene.add(beam)

        const tween1 = new TWEEN.Tween(beam.material[1])
          .to({opacity: 0}, 1000)
          .onComplete(() => {
            tween2.start(); // 结束后调用tween2，开始显示
          }); // 渐隐动画
        const tween2 = new TWEEN.Tween(beam.material[1])
          .to({opacity: 1}, 1000)
          .onComplete(() => {
            tween1.start(); // 结束后调用tween1，开始隐藏
          }); // 渐显动画
        tween1.start();

        /*添加光圈 结束*/

        /*添加文字 开始*/
        this.createText(
          "Three.js---Demo",
          "rgb(100,149,237)",
          {
            position: {y: 600, z: 250, x: -50},
          }
        ).then((text) => {
          self.scene.add(text);
        })
        /*添加文字 结束*/


        /*地面管道 开始*/
        const {texture: tubeTexture1, mesh: tube1} = await this.createTube(
          [900, 10, 900],
          [900, 10, -900],
          [-900, 10, -900],
          [-900, 10, 900],
          [900, 10, 900]
        );
        this.scene.add(tube1);
        this.tubeTexture1 = tubeTexture1;
        /*地面管道 结束*/

        /*添加地球 开始*/
        const earth = this.createEarth({position: {x: 900, y: 100, z: 900}, scale: {x: 10, y: 10, z: 10}});
        // this.change2BasicMat(earth);
        const earth1 = this.createClone(earth);
        const earth2 = this.createClone(earth, {position: {x: 900, y: 100, z: -900}});
        const earth3 = this.createClone(earth, {position: {x: -900, y: 100, z: -900}});
        const earth4 = this.createClone(earth, {position: {x: -900, y: 100, z: 900}});

        this.scene.add(earth1);
        this.scene.add(earth2);
        this.scene.add(earth3);
        this.scene.add(earth4);

        /*添加地球 结束*/


        this.initLight();
        this.scene.add(this.light);

        //初始化相机
        let pointLight = new THREE.PointLight(0xffffff, 0.8);
        this.camera = new THREE.PerspectiveCamera(this.fov, window.innerWidth / window.innerHeight, 1, 10000);
        // this.camera.position.set(300, 300, 150);
        this.camera.position.set(-500, 1040, 1050);
        this.camera.up.set(0, 1, 0);
        // this.camera.up.set(1, 0, 0);
        // this.camera.up.set(0, 0, 1);
        // this.camera.up.set(0, -1, 0);
        this.camera.lookAt(0, 0, 0);
        this.camera.add(pointLight);
        this.scene.add(this.camera);

        //渲染
        this.renderer = new THREE.WebGLRenderer({
          // 开启抗锯齿
          antialias: true,
          // 开启背景透明
          alpha: true
        });//会在body里面生成一个canvas标签,
        this.renderer.setPixelRatio(window.devicePixelRatio);//为了兼容高清屏幕
        this.renderer.setClearColor(new THREE.Color(0x303030));
        this.renderer.setSize(window.innerWidth, window.innerHeight);

        //初始化控制器
        this.controls = new OrbitControls(this.camera, this.renderer.domElement);
        this.controls.enableDamping = true; // an animation loop is required when either damping or auto-rotation are enabled
        this.controls.dampingFactor = 0.05;
        this.controls.screenSpacePanning = false;
        this.controls.target.set(0, 0, 0);
        this.controls.minDistance = 10;
        this.controls.maxDistance = 5000;
        // this.controls.maxPolarAngle = Math.PI * (79 / 180);
        this.controls.maxPolarAngle = Math.PI / 2;
        this.controls.update();
        // this.controls.addEventListener("change",self.render);



        // 参数设置坐标轴大小,150，编写程序的时候，可以根据相机参数调整为合适的值，如果太小可以无法显示出来
        const axesHelper = new THREE.AxesHelper(2100);
        this.scene.add(axesHelper);

        const container = document.getElementById('container');
        container.appendChild(this.renderer.domElement);
        container.addEventListener('click', this.onMouseClick, false);

        window.addEventListener('resize', this.onWindowResize, false);//添加窗口监听事件（resize-onresize即窗口或框架被重新调整大小）
        // container.addEventListener("mousemove", this.onMouseMove, false); // 只在容器中监听事件
        document.addEventListener("mousemove", this.onMouseMove, false);
      },
      initThree(selector) {

      },
      initLight() {
        this.light = new THREE.AmbientLight(0xffffff);
        this.light.position.set(50, 200, 100);
      },
      onWindowResize() {
        this.camera.aspect = window.innerWidth / window.innerHeight;
        this.camera.updateProjectionMatrix();
        this.renderer.setSize(window.innerWidth, window.innerHeight);
      },
      onMouseMove(event) {
        event.preventDefault();
        if (event.toElement.tagName == 'CANVAS') {
          this.mousePosition.x = event.clientX;
          this.mousePosition.y = event.clientY;
          this.mouse.x = (event.clientX / window.innerWidth) * 2 - 1;
          this.mouse.y = -(event.clientY / window.innerHeight) * 2 + 1;
        } else {
          document.getElementById("name-box").style.display = "none";
        }
      },
      onMouseClick(event) {
        let self = this;
        event.preventDefault();

        self.mouse.x = (event.offsetX / window.innerWidth) * 2 - 1; // 坐标变换
        self.mouse.y = -(event.offsetY / window.innerHeight) * 2 + 1;
        self.raycaster.setFromCamera(self.mouse, self.camera);
        let intersects = self.raycaster.intersectObjects(self.itemList);
        if (intersects.length > 0) {

          if (self.nowSelect) {
            self.nowSelect.check = false;
          }

          self.nowSelect = intersects[0].object.parent; // 当前选中的对象
          self.selectedName = self.nowSelect.name;
          self.nowSelect.check = true;

          self.flag = false;
          for (let i = 0; i < this.options.length; i++) {
            if (this.options[i].label == self.nowSelect.name) {
              self.change(this.options[i].value)
            }
          }
        }

      },
      animate() {
        let detal = this.clock.getDelta();
        this.timeS = this.timeS + detal;
        if (this.timeS > this.renderT) {
          // console.log("执行");
          this.raycaster.setFromCamera(this.mouse, this.camera);
          const intersection = this.raycaster.intersectObjects(this.itemList);
          const nameBox = document.getElementById("name-box");
          if (intersection.length > 0) {
            // 名称提示
            // console.log(intersection[0]);
            let str = `<div style="font-size: 18px;">名称 :${intersection[0].object.parent.name}</div>
                     <div style="font-size: 16px">x坐标 :${intersection[0].object.parent.position.x}</div>
                     <div style="font-size: 16px">y坐标 :${intersection[0].object.parent.position.y}</div>
                     <div style="font-size: 16px">z坐标 :${intersection[0].object.parent.position.z}</div>
                     <div style="font-size: 16px">当前状态 :${intersection[0].object.parent.check ? '选中' : '未选中'}</div>
        `
            // console.log(intersection[0].object.parent);
            nameBox.innerHTML = str;
            nameBox.style.display = "block";
            nameBox.style.top = this.mousePosition.y + 'px'; // 跟随鼠标的位置
            nameBox.style.left = this.mousePosition.x + 30 + 'px';
          } else {
            nameBox.style.display = "none";
          }

          this.controls.update(detal); // 更新控制器

          TWEEN.update(); // 补间动画


          // this.render();
          // this.bloomComposer.render(detal); // 3. 不在使用WebGLRenderer的render方法
          this.render(); //  静态动画  可以将render写在chang里面

          // console.log(this.camera.up);
          if (this.flag) {
            this.camera.position.x = 800 * Math.cos(this.angle * Math.PI / 180);
            this.camera.position.z = 800 * Math.sin(this.angle * Math.PI / 180);
            this.camera.position.y = 1000;
            this.angle += 0.05;
            if (this.angle > 360) {
              this.angle = 0;
            }
          }
          // console.log(`调用.render时间间隔`,this.timeS*1000+'毫秒');
          this.timeS = 0;

          this.tubeTexture1.offset.x -= 0.006;
        }


        this.req = requestAnimationFrame(this.animate);
      },
      render() {
        this.renderer.render(this.scene, this.camera);
      },
      addObjPic(val, name, x, y) {
        let self = this;
        self.flag = false;
        return new Promise(function (resolve, reject) {
          new GLTFLoader().setDRACOLoader(self.dracoLoader).load(
            `/static/model/${val}/${val}.glb`,
            (gltf) => {
              self.loadingInstance.close();

              let model = gltf.scene;

              // 获取组别的名字
              model.name = name;
              let scale = 0.02;
              model.scale.set(scale, scale, scale);
              // model.rotateX(-Math.PI / 2);//绕x轴旋转π/4
              model.position.set(x, "10", y);

              self.scene.add(model);

              self.itemList.push(...model.children);
              resolve(val);
            },
            (xhr) => {

              self.loadingInstance = Loading.service({
                fullscreen: true,
                text: '加载贴图模型中....',
                background: "rgba(31,86,185,0.25)"
              });
            },
            (error) => {
              // console.log(error);
              console.log('error');
              // called when loading has errors
              console.error('An error happened', error);
              reject('error')
            }
          );
        })

      },
      addObj(val, name, x, y) {
        let self = this;
        self.flag = false;
        self.animateList = [];
        // 加载楼房 压缩之后的gltf 要使用了DRACOLoader才可以加载
        const dracoLoader = new DRACOLoader();
        dracoLoader.setDecoderPath("../../../static/draco/"); // 最后要加/
        dracoLoader.preload();

        new GLTFLoader().setDRACOLoader(dracoLoader).load(
          `/static/model/${val}/${val}.gltf`,
          (gltf) => {
            self.loadingInstance.close();

            let model = gltf.scene;

            // 获取组别的名字
            model.name = name;
            let scale = 0.2;
            model.scale.set(scale, scale, scale);
            model.rotateX(-Math.PI / 2);//绕x轴旋转π/4
            model.position.set(x, "10", y);

            self.scene.add(model);

            // 改变模型的材质，偏暗蓝且带透明
            const list = [...model.children];
            list.forEach(item => {
              self.change2BasicMat(item);
            });

            self.itemList.push(...model.children);
          },
          (xhr) => {

            self.loadingInstance = Loading.service({
              fullscreen: true,
              text: '加载模型中....',
              background: "rgba(31,86,185,0.25)"
            });
          },
          (error) => {
            // console.log(error);
            console.log('error');
            // called when loading has errors
            console.error('An error happened', error);
          }
        );

      },
      addObjPromisePic(val, name, x, y, z) {
        let self = this;
        return new Promise(function (resolve, reject) {
          new GLTFLoader().setDRACOLoader(self.dracoLoader).load(
            `/static/model/${val}/${val}.glb`,
            (gltf) => {
              let model = gltf.scene;
              // 获取组别的名字
              model.name = name;
              let scale = self.swithSize[val];
              // let scale = 0.03;
              model.scale.set(scale, scale, scale);
              // model.rotateX(-Math.PI / 2);//绕x轴旋转π/4
              model.position.set(x, z, y);
              self.scene.add(model);

              self.itemList.push(...model.children);

              resolve(val);
            },
            (xhr) => {
              // self.loadingInstance = Loading.service({
              //   fullscreen: true,
              //   text: '加载模型中....',
              //   background: "rgba(31,86,185,0.25)"
              // });
            },
            (error) => {
              reject('error');
            }
          );
        })
      },
      addObjPromise(val, name, x, y) {
        let self = this;
        return new Promise(function (resolve, reject) {
          new GLTFLoader().setDRACOLoader(self.dracoLoader).load(
            // `/static/model/${val}/${val}.gltf`,
            `/static/model/${val}/${val}.glb`,
            (gltf) => {
              let model = gltf.scene;
              // 获取组别的名字
              model.name = name;
              let scale = 0.2;
              model.scale.set(scale, scale, scale);
              model.rotateX(-Math.PI / 2);//绕x轴旋转π/4
              model.position.set(x, "10", y);
              self.scene.add(model);

              // 改变模型的材质，偏暗蓝且带透明
              const list = [...model.children];
              list.forEach(item => {
                self.change2BasicMat(item);
              });


              self.itemList.push(...model.children);

              resolve(val);
            },
            (xhr) => {
              // self.loadingInstance = Loading.service({
              //   fullscreen: true,
              //   text: '加载模型中....',
              //   background: "rgba(31,86,185,0.25)"
              // });
            },
            (error) => {
              reject('error');
            }
          );
        })
      },
      change2BasicMat(object3d) {
        let self = this;
        object3d.traverse(item => {
          if (item.material) {
            item.material = new THREE.MeshBasicMaterial({
              opacity: 0.6,
              color: 0x1f56b9,
              side: THREE.BackSide,
              transparent: true,
            });
          }
        });
      },
      animateCamera(p1, p2) {
        let self = this;
        self.controls.enabled = false;   //关闭控制器
        let tween = new TWEEN.Tween(p1).to(p2, 1000).easing(TWEEN.Easing.Cubic.InOut);
        tween.onUpdate(function () {
          self.camera.position.set(p1.x, p1.y, p1.z);
        });
        tween.onComplete(function () {
          self.controls.enabled = true;   ///开启控制器
        });
        tween.start();
      },
      animateMove(p1, p2, obj) {
        let self = this;
        self.controls.enabled = false;   //关闭控制器
        let tween = new TWEEN.Tween(p1).to(p2, 1000).easing(TWEEN.Easing.Cubic.InOut);
        tween.onUpdate(function () {
          obj.position.set(p1.x, p1.y, p1.z);
        });
        tween.onComplete(function () {
          self.controls.enabled = true;   ///开启控制器
        });
        tween.start();
      },
      change(val) {
        let self = this;
        for (let i = 0; i < this.options.length; i++) {
          if (this.options[i].value == val) {
            let obj = this.scene.getObjectByName(this.options[i].label);
            let res = this.getBound(obj);
            // console.log('-1');
            // console.log(obj);
            if (obj.position.x >= 0 && obj.position.z >= 0) {
              self.animateCamera(self.camera.position, {
                x: res.max.x + 50,
                y: res.max.y + 50,
                z: res.max.z + 50
              });
              // console.log("0");
            } else if (obj.position.x <= 0 && obj.position.z <= 0) {
              self.animateCamera(self.camera.position, {
                x: res.min.x - 50,
                y: res.max.y + 50,
                z: res.min.z - 50
              });
              // console.log("1");
            } else if (obj.position.x <= 0 && obj.position.z >= 0) {
              self.animateCamera(self.camera.position, {
                x: res.min.x - 50,
                y: res.max.y + 50,
                z: res.max.z + 50
              });
              // console.log("2");
            } else if (obj.position.x >= 0 && obj.position.z <= 0) {
              self.animateCamera(self.camera.position, {
                x: res.max.x + 50,
                y: res.max.y + 50,
                z: res.min.z - 50
              });
              // console.log("3");
            }
            break;
          }
        }
      },
      getBound(model) {
        let box = new THREE.Box3();
        let {max, min} = box.expandByObject(model);
        return {
          max,
          min
        }
      },
      hide() {
        this.show = false;
      },
      // 创建围绕物体的辉光效果
      createLightBeam(width, height, arc, color, conf) {
        const shape = this.createArcRect(width, height, arc);
        const extrudeSettings = {
          steps: 64,
          depth: 100, // step设置为1，保证侧面只有一个平面，如果想更高，可以通过scale放大
          bevelEnabled: false,
        };
        const geometry = new THREE.ExtrudeGeometry(shape, extrudeSettings);
        const bottomMaterial = new THREE.MeshBasicMaterial({
          visible: false // 设置上下底面的材质不可见
        });
        const texture = this.createTexture("../../../static/gradient.png");
        const sideMaterial = new THREE.MeshBasicMaterial({
          map: texture,
          side: THREE.DoubleSide,
          transparent: true,
          opacity: 1,
          depthWrite: true,
          color
        }); // 给侧面进行贴图，贴图的图片为一张渐变色的图片
        const mesh = new THREE.Mesh(geometry, [bottomMaterial, sideMaterial]);
        this.initConfig(mesh, conf);
        return mesh;
      },
      // 创建一个弧角矩形
      // 利用Three中的Shape创建一个带弧角的矩形的形状，三个参数分别代表长、宽、弧度
      createArcRect(width, height, arc) {
        const shape = new THREE.Shape();
        const w = width - arc;
        const h = height - arc;
        shape.moveTo(w, height);
        shape.arc(0, -1 * arc, arc, Math.PI / 2, 0, true);
        shape.lineTo(width, arc);
        shape.arc(-1 * arc, 0, arc, 0, (3 * Math.PI) / 2, true);
        shape.lineTo(arc, 0);
        shape.arc(0, arc, arc, (3 * Math.PI) / 2, Math.PI, true);
        shape.lineTo(0, h);
        shape.arc(arc, 0, arc, Math.PI, Math.PI / 2, true);
        shape.lineTo(w, height);
        return shape;
      },
      // 创建一种纹理,path为贴图图片路径（此处用来模拟管线动画）
      createTexture(path, conf) {
        const texture = new THREE.TextureLoader().load(path);
        this.initConfig(texture, conf);
        return texture;
      },
      // 对传入的conf进行处理，因为大部分几何体都能对其position（位置）、rotation（渲染）、scale（缩放）等进行设置
      // 应用举例：initConfig(mesh, { position: { x: -15, y: -1 } })
      // 第一个参数不一定要传入mesh，也可以传入纹理Texture
      initConfig(mesh, conf) {
        if (conf) {
          const {position, rotation, scale, repeat} = conf;
          if (position) {
            const {x, y, z} = position;
            typeof x !== 'undefined' ? (mesh.position.x = x) : null;
            typeof y !== 'undefined' ? (mesh.position.y = y) : null;
            typeof z !== 'undefined' ? (mesh.position.z = z) : null;
          }
          if (rotation) {
            const {x, y, z} = rotation;
            typeof x !== 'undefined' ? (mesh.rotation.x = x) : null;
            typeof y !== 'undefined' ? (mesh.rotation.y = y) : null;
            typeof z !== 'undefined' ? (mesh.rotation.z = z) : null;
          }
          if (scale) {
            const {x, y, z} = scale;
            typeof x !== 'undefined' ? (mesh.scale.x = x) : null;
            typeof y !== 'undefined' ? (mesh.scale.y = y) : null;
            typeof z !== 'undefined' ? (mesh.scale.z = z) : null;
          }
          if (repeat) {
            const {x, y} = repeat;
            // 对Texture的repeat进行处理
            if (typeof x !== 'undefined') {
              // 设置x方向的重复数
              mesh.wrapS = THREE.RepeatWrapping;
              mesh.repeat.x = x;
            }
            if (typeof y !== 'undefined') {
              // 设置y方向的重复数
              mesh.wrapT = THREE.RepeatWrapping;
              mesh.repeat.y = y;
            }
          }
        }
      },
      // 创建立体3D文字
      // 为了解决回调地狱，同样引入Promise
      createText(text, color, conf) {
        let self = this;
        return new Promise((res) => {
          new TTFLoader().load("../../../static/font/simhei.ttf", function (data) {
            const font = new THREE.Font(data);
            const geometry = new THREE.TextBufferGeometry(text, {
              font,
              size: 50,
              height: 1,
              curveSegments: 64,
            });
            geometry.center(); // 将文字居中
            const material = new THREE.MeshBasicMaterial({color});
            const mesh = new THREE.Mesh(geometry, material);
            self.initConfig(mesh, conf);
            res(mesh);
          });
        });
      },
      // 创建永远朝向自己这一面的文字
      async createSpriteText(selcetor, conf) {
        const elem = document.querySelector(selcetor); // selector是传入的选择器
        const canvas = await html2canvas(elem, {
          x: elem.offsetLeft, // 加入x、y配置，防止画布偏移 产生部分空白
          y: elem.offsetTop,
        });
        const canvasW = canvas.width;
        const canvasH = canvas.height;
        const texture = new THREE.CanvasTexture(canvas);
        texture.magFilter = THREE.NearestFilter; // 提高清晰度
        texture.minFilter = THREE.NearestFilter;
        const spriteMaterial = new THREE.SpriteMaterial({
          map: texture,
          opacity: 0.8,
        }); // 创建精灵材质，map属性设置贴图，为了更高的可配置度，我们选择用canvas贴图
        const sprite = new THREE.Sprite(spriteMaterial);
        const shape = this.createArcRect((400 * canvasW) / canvasH, 400, 20);
        const geometry = new THREE.ShapeBufferGeometry(shape, 64);
        this.computeUV(geometry); // 计算并更新该几何体的UV
        sprite.geometry = geometry; // 用创建好的弧角矩形平面代替Sprite默认的geometry
        this.initConfig(sprite, conf);
        return {spriteMaterial, sprite};
      },
      // 计算对应UV坐标
      computeUV(geometry) {
        geometry.computeBoundingBox(); // 计算外边界矩形，这样才能得到geometry的boundingBox属性值
        const max = geometry.boundingBox.max,
          min = geometry.boundingBox.min; // 获取最大、最小值
        const offset = new THREE.Vector2(0 - min.x, 0 - min.y); // 计算偏移量
        const range = new THREE.Vector2(max.x - min.x, max.y - min.y); // 计算范围
        const uvArr = geometry.getAttribute("uv");
        uvArr.array = uvArr.array.map((item, index) =>
          index % 2 ? item / range.y + offset.y : item / range.x + offset.x
        );
        geometry.setAttribute("uv", uvArr); // 将geometry的uv属性设置成我们刚刚计算出来的新uv值
        geometry.uvsNeedUpdate = true; // needUpdate必须为true才会更新
      },
      // 创建一条管道利用 TubeGeometry
      async createTube(...pointsArr) {
        const path = this.createPath(pointsArr);
        const geometry = new THREE.TubeGeometry(path, 64, 10);

        // 模拟管线运动动画，将两个素材图按比例合并，然后生成贴图texture
        const base64 = await this.mergeImage("../../../static/img/2.png", "./../../static/img/1.png", 1, 7);

        const texture = this.createTexture(base64, {repeat: {x: 1}});
        const material = new THREE.MeshBasicMaterial({
          map: texture,
          transparent: true,
        });
        const mesh = new THREE.Mesh(geometry, material);
        return {texture, mesh};
      },
      // 创建一条路径，可以是三维路径，传入一组点
      createPath(pointsArr) {
        pointsArr = pointsArr.map((point) => new THREE.Vector3(...point));
        // 方法一：自定义三维路径
        const path = new THREE.CurvePath();
        for (let i = 0; i < pointsArr.length - 1; i++) {
          const lineCurve = new THREE.LineCurve3(pointsArr[i], pointsArr[i + 1]);
          path.curves.push(lineCurve);
        }
        // 方法二：利用CatmullRomCurve3 创建三位路径，不过是平滑的三维样条曲线
        // const path = new THREE.CatmullRomCurve3(pointsArr);
        return path;
      },
      mergeImage(imgSrc1, imgSrc2, a, b) {
        return new Promise((res, rej) => {
          const canvas = document.createElement("canvas");
          const ctx = canvas.getContext("2d");
          const img1 = new Image();
          img1.src = imgSrc1;
          img1.onload = function () {
            const img2 = new Image();
            img2.src = imgSrc2;
            img2.onload = function () {
              canvas.width = img1.width * a + img2.width * b;
              canvas.height = img1.height;
              ctx.rect(0, 0, canvas.width, canvas.height);
              ctx.fillStyle = "#fff";
              ctx.fill();

              let width = 0;
              // 绘制img1
              for (let i = 0; i < a; i++) {
                ctx.drawImage(img1, width, 0, img1.width, img1.height);
                width += img1.width;
              }

              // 绘制img2
              for (let i = 0; i < b; i++) {
                ctx.drawImage(img2, width, 0, img2.width, img2.height);
                width += img2.width;
              }

              // 合并
              const base64 = canvas.toDataURL("image/png"); // "image/png" 这里注意一下
              // const img = document.createElement('img');
              // img.setAttribute('src', base64);
              // res(img);
              res(base64);
            };
          };
        });
      },
      // 创建一个Layer，用于区分辉光物体
// Three中为所有的几何体分配 1个到 32 个图层，编号从 0 到 31，所有几何体默认存储在第 0 个图层上，
// 为了更好的区分辉光物体和非辉光物体，我们需要利用 Layer 创建一个图层，把辉光物体额外添加在一个新的图层上
      createLayer(num) {
        const layer = new THREE.Layers();
        layer.set(num);
        return layer;
      },
      // 创建一个地球（球体）
      createEarth(conf) {
        const geometry = new THREE.SphereBufferGeometry(5, 64, 64);  //  构建一个球型几何体，BufferGeometry性能比Geometry好
        const texture = new THREE.TextureLoader().load("../../../static/img/earth.png");  // 创建一个纹理贴图，将其贴到一个表面
        const material = new THREE.MeshBasicMaterial({map: texture}); // 创建一个材质，map属性传入刚刚创建好的纹理贴图
        const mesh = new THREE.Mesh(geometry, material); // 利用Mesh将几何体和材质联系在一起，形成最终的物体
        this.initConfig(mesh, conf);
        // mesh.castShadow = true; // 使该物体能产生阴影
        mesh.layers.enable(1); // 局部辉光的layer
        return mesh;
      },
      // 创建一个克隆体
      createClone(mesh, conf) {
        const newMesh = mesh.clone();
        this.initConfig(newMesh, conf);
        return newMesh;
      },
      // 后期处理，效果合成器
// 后期处理的通常步骤：
//   1. 创建一个 EffectComposer，假设命名为composer
//   2. 给composer加入(addPass)各种通道
// 通常第一个加入的通道是RenderPass，后续可以看需求选择加入的通道类型和顺序，例如这里后续就用到了BloomPass

// 1.准备两个EffectComposer，一个bloomComposer用来产生辉光效果，另一个 finalComposer 用来正常渲染整个场景
// 2.将除辉光物体外的其他物体的材质转成黑色
// 3.在 bloomComposer中利用 BloomPass产生辉光，但这里需要设置bloomComposer.renderToScreen = false;表示不渲染到屏幕上
// 4.将转成黑色材质的物体还原成初始材质
// 5.用 finalComposer开始渲染，其中finalComposer需要加入一个通道(addPass)，该通道利用了bloomComposer的渲染结果
      createComposer() {
        // 根据scene和camera渲染出一个场景，和普通的webGLRenderer一样
        const renderPass = new RenderPass(this.scene, this.camera); // 第一个分组的RenderPass 用来产生辉光效果
        const renderNormalPass = new RenderPass(this.normalScene, this.camera); // 第二个分组的RenderPass 用来正常渲染整个场景

        // 产生辉光，但是不渲染到屏幕上
        const bloomComposer = new EffectComposer(this.renderer);
        bloomComposer.renderToScreen = false; // 不渲染到屏幕上
        const bloomPass = createUnrealBloomPass(); // 用来创建BloomPass（辉光效果）
        bloomComposer.addPass(renderPass);
        bloomComposer.addPass(bloomPass);

        // 利用 MaskPass 最终渲染到屏幕上
        // 最终真正渲染到屏幕上的效果合成器 finalComposer
        const finalComposer = new EffectComposer(this.renderer);
        finalComposer.renderTarget1.stencilBuffer = true;
        finalComposer.renderTarget2.stencilBuffer = true; // 两个都设置为true
        renderPass.clear = false;
        renderNormalPass.clear = false; //非常重要，否则 renderNormalPass 会清除掉上一个 RenderPass —— renderPass 的颜色

        finalComposer.addPass(renderPass);  /*渲染当前场景*/
        finalComposer.addPass(renderNormalPass);


        const clearMaskPass = new ClearMaskPass();

        // 第一组开始渲染  加辉光、抗锯齿
        const maskPass1 = new MaskPass(this.scene, this.camera);
        // 该通道利用了 bloomComposer 的渲染结果
        const shaderPass = this.createShaderPass(bloomComposer); // 创建自定义的着色器Pass
        const FxaaPass = this.createFxaaPass(); // 抗锯齿
        finalComposer.addPass(maskPass1); // 添加第一组的maskPass
        finalComposer.addPass(shaderPass); // 追加光效
        finalComposer.addPass(FxaaPass); // 输出到屏幕
        finalComposer.addPass(clearMaskPass); // 清除第一组的maskPass

        // 第二组开始渲染  文字精灵 光圈
        const maskPass2 = new MaskPass(this.normalScene, this.camera);
        finalComposer.addPass(maskPass2); // 添加第二组的maskPass
        finalComposer.addPass(clearMaskPass); // 清除第二组的maskPass


        const effectCopy = new ShaderPass(CopyShader);
        finalComposer.addPass(effectCopy); // 最后需要CopyShader，因为设置了手动清除
        return {bloomComposer, finalComposer};
      },
      // UnrealBloomPass，辉光效果
      createUnrealBloomPass() {
        const bloomPass = new UnrealBloomPass(
          new THREE.Vector2(window.innerWidth, window.innerHeight),
          1.5,
          0.4,
          0.85
        );
        const params = {
          exposure: 1,
          bloomThreshold: 0.2,
          bloomStrength: 0.5, // 辉光强度
          bloomRadius: 0
        };
        bloomPass.threshold = params.bloomThreshold;
        bloomPass.strength = params.bloomStrength;
        bloomPass.radius = params.bloomRadius;
        return bloomPass;
      },
      // ShaderPass，着色器pass，自定义程度高，需要编写OpenGL代码
      // 传入bloomComposer
      createShaderPass(bloomComposer) {
        // 着色器材质，自定义shader渲染的材质
        const shaderMaterial = new THREE.ShaderMaterial({
          uniforms: {
            baseTexture: {value: null},
            bloomTexture: {value: bloomComposer.renderTarget2.texture} // 辉光贴图属性设置为传入的bloomComposer，这里就说明了为什么bloomComposer不要渲染到屏幕上
          },
          vertexShader: document.getElementById("vertexshader").textContent, // 顶点着色器
          fragmentShader: document.getElementById("fragmentshader").textContent,// 片元着色器
          defines: {},
        });
        const shaderPass = new ShaderPass(shaderMaterial, "baseTexture");
        shaderPass.needsSwap = true;
        return shaderPass;
      },
// 抗锯齿，fxaa、smaa、ssaa都可以抗锯齿，抗锯齿效果依次减弱
// 加入抗锯齿fxaa通道
      createFxaaPass() {
        let FxaaPass = new ShaderPass(FXAAShader);
        const pixelRatio = renderer.getPixelRatio();
        FxaaPass.material.uniforms["resolution"].value.x =
          1 / (window.innerWidth * pixelRatio);
        FxaaPass.material.uniforms["resolution"].value.y =
          1 / (window.innerHeight * pixelRatio);
        FxaaPass.renderToScreen = true; /*输出屏幕*/
        return FxaaPass;
      }

    }
  }
</script>

<style scoped lang="less">
  #model /deep/ canvas {
    /*要修改元素的display，否则右侧会出现滚动条*/
    display: block;
  }

  #model {


    .fade-enter-active, .fade-leave-active { //指就是html中fade名称
      transition: opacity .1s; // 0.1s动画过渡的时间
    }

    .fade-enter, .fade-leave-to {
      opacity: 0;
    }

    .nav-top {
      position: fixed;
      top: 0;
      left: 0;
      height: 80px;
      width: 100%;
      background-color: rgba(3, 21, 24, 0.7);
      text-align: center;

      .nav-top-title {
        line-height: 80px;
        color: #ffffff;
        font-size: 24px;
        font-weight: 600;
      }

    }

    .el-select {
      width: 100%;

    }

    .el-switch {
      display: flex;
      justify-content: center;

      /deep/ span {
        color: #303030;
        margin-left: 10px;
        margin-right: 10px;
      }

      /deep/ .is-active span {
        color: white;
      }
    }


    button {
      position: fixed;
      left: 200px;
      top: 10px;
    }


    #name-box {
      position: absolute;
      font-size: 20px;
      pointer-events: none;
      padding: 15px;
      border-radius: 10px;

      border-style: solid;
      white-space: nowrap;
      z-index: 9999999;
      transition: left 0.4s cubic-bezier(0.23, 1, 0.32, 1) 0s, top 0.4s cubic-bezier(0.23, 1, 0.32, 1) 0s;
      background-color: rgba(50, 50, 50, 0.5);
      border-width: 0px;
      border-color: rgb(51, 51, 51);
      color: rgb(255, 255, 255);
      font: 14px / 21px "Microsoft YaHei";
    }

    .tip-container {
      background-color: rgba(22, 31, 38, 0.87);

      .tip-title {
        width: 100%;
        height: 48px;
        color: #ffffff;
        overflow: hidden;
        box-sizing: border-box;
        border-bottom: 1px solid #052025;
        font-family: 微软雅黑;
        font-size: 16px;

        span {
          line-height: 48px;
          vertical-align: middle;
          font-size: 16px;
          font-weight: 600;

          > img {
            width: 16px;
            height: 16px;
          }
        }

        span:nth-child(1) {
          float: left;
          margin-left: 12px;
        }

        span:nth-child(2) {
          float: left;
          margin-left: 10px;
        }

        span:nth-child(3) {
          float: right;
          margin-right: 10px;
        }

      }

      .tip-content {
        padding: 16px;

        .speed-container {
          width: 100%;
          box-sizing: border-box;
          border-bottom: 1px solid #052025;
          padding-bottom: 10px;

          .speed-title {
            overflow: hidden;
            line-height: 30px;

            span {
              display: block;
              float: left;
              color: #ffffff;
              padding-right: 10px;
            }

          }

          .speed-content {
            margin-top: 10px;
            width: 100%;

            ul {
              width: 100%;

              li {
                width: 100%;
                list-style: none;
                overflow: hidden;
                margin: 0;

                span:nth-child(1) {
                  display: block;
                  float: left;
                  padding: 0;
                  margin: 0;
                }

                span:nth-child(2) {
                  display: block;
                  float: right;
                  padding: 0;
                  margin: 0;
                }

                .speed-line {
                  width: 200px;
                  background-color: #043037;
                  height: 13px;
                  margin-top: 8px !important;
                  border-radius: 5px;
                  position: relative;

                  .speed-num {
                    /* width: 90%; */
                    display: block;
                    height: 100%;
                    border-radius: 5px;
                  }

                  .numText {
                    position: absolute;
                    top: -9px;
                    right: 4px;
                    color: #ffffff;
                    font-size: 12px;
                  }
                }


              }
            }
          }

        }


        ul {
          li {
            color: #ffffff;
            list-style: none;
            overflow: hidden;
            line-height: 30px;
            font-size: 14px;

            > .zhzb-li-name {
              float: left;

              > .typeStyle {
                color: #1079A6;
                font-weight: 600;
              }
            }

            > .zhzb-li-num {
              float: right;
              color: #00CEFA;

              > .yj-span {
                display: block;
                width: 30px;
                float: right;
                text-align: right;
              }
            }

          }
        }
      }
    }

  }
</style>
