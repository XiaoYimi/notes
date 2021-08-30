# 3D Three 开发手册

## 概念理解

### 场景 Scene

 物体、灯光和摄像机 的容器。



### 相机 Camera

#### 正交投影照相机 OrthographicCamera

##### 物理效果

对于三维空间的渲染对象，以二维平面的形式展示,不因为投影而改变物体大小比例。

##### 用途

用于渲染对象的制图,建模。

##### 参数

- 摄像机视锥体左侧面 left
- 摄像机视锥体有侧面 right
- 摄像机视锥体上侧面 top
- 摄像机视锥体下侧面 bottom
- 摄像机视锥体近端面 near
- 摄像机视锥体远端面 far



#### 透视相机 PerspectiveCamera

##### 物理效果

更接近人眼的观察效果,人眼观测的近大远小效果。

##### 用途

虚拟场景的·现场观测。

##### 参数

- 摄像机视锥体垂直视野角度 fov

- 摄像机视锥体长宽比(渲染范围) width/height
- 摄像机视锥体近端面(近剪切面) near
- 摄像机视锥体远端面(远剪切面) far



### 渲染器 Renderer

#### WebGL渲染器 WebGLRenderer



#### Canvas渲染器 CanvasRenderer





### 渲染材质 Material





### 辅助网格 Mesh





## 原理解析

1. 创建 html 标签容器,用于存放渲染内容动画。
2. 创建场景(Scene)。
3. 创建相机(Camera),并配置相关参数(`const { devicePixedlRatio, innerWidth, innerHeigth } = window | parentDom`)。
4. 创建渲染器(Renderer),并配置相关参数。
5. 创建几何图形对象(Geometry),并配置相关参数。
6. 创建材质对象(Material),并配置相关参数。
7. 创建网格模型对象(Mesh),并配置相关配置。
8. 通过场景对象(Scene)绘制方法(Scene.add())添加网格模型对象(Mesh)。
9. 再通过网格模型对象(Mesh)在场景坐标上进行微调参数。





## 性能优化

### 卡顿原因

由于 Vue data 参数响应式原理导致的。data 属性一旦有所变化，会触发 getter, setter 方法,导致重新计算。而 Three.js 是一个庞大系列对象,全部由 data 来存储数据,会导致计算压力巨大,故导致卡顿。



### 解决方案

- 在 beforeMounted 对某些变量进行初始化操作(this.xxx = null)。
- 在 mounted 开始进行定义 Three 的场景,相机,渲染器等对象。 
- 当然,可在 beforeDestroy 进行释放。







## 开发教程

### 引入核心插件包

```js
import * as Three from 'three'
```



### 创建场景

```js
function initScene () {
	return new Three.Scene();
}
```



### 创建虚拟摄相机

```js
/** 常用 2 种摄像机(透视|正交) */
// 透视摄像机
function initPerspectiveCamera (fov = 45, whScale = window.innerWidth / window.innerHeight, near = 0.1, far = 1000) {
    return new Three.PerspectiveCamera(fov, whScale, near, far);
}

// 正交摄像机
function initOrthographicCamera (left = window.innerWidth / 2, right = window.innerWidth / 2, top = window.innerHeight / 2, bottom = window.innerHeight / 2, near = 1, far = 1000) {
    return new Three.OrthographicCamera(left, right, top, bottom, near, far);
}
```



### 创建渲染器

```js
/** 常用 2 中渲染器 */
// WebGLRenderer 渲染器
function initWebGLRenderer () {
    /** antialias 抗锯齿 */
	return new Three.WebGLRenderer({ antialias: true })
}
```



### 创建虚拟坐标系

```js
function initAxesHelper (number) {
    return new Three.AxesHelper(number);
}
```



### 创建帧数显示

```js
import Stats from "three/examples/jsm/libs/stats.module";

function initStats (type = 'ms') {
    const index = ['fps', 'ms', 'mb', 'custom'].findIndex(item => item === type)
    const stats = new Stats();
    stats.showPanel(index);
    return stats;
}
```



### 创建几何体

```js
/** 常见几何体 方形,球 */

// 几何体-球
function initSphereGeometry (radius = 50, widthSegments = 100, heightSegments = 100) {
    return new Three.SphereGeometry(radius, widthSegments, heightSegments);
}


function initBoxGeometry (long = 50, wide = 50,high = 50) {
    return new Three.BoxGeometry(long, wide, )
}
```

