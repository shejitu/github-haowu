# PlayCanvas Engine — 开源 Web 3D 游戏引擎

> ⭐ 16.6k+ Stars | JavaScript/TypeScript | MIT | [原仓库](https://github.com/playcanvas/engine) | [官网](https://playcanvas.com)

## 一句话简介

一个构建在 **WebGL2 / WebGPU / WebXR / glTF** 之上的开源游戏引擎，用它在浏览器里做交互式 3D 应用、游戏和数据可视化——任何设备、任何浏览器都能直接跑，不装插件。

## 核心亮点

- 🧊 **2D + 3D 图形**：基于 WebGL2 与 WebGPU 的高级渲染管线
- 💠 **3D 高斯泼溅**：一等公民支持加载渲染 3D Gaussian Splats（实景扫描重建的 新玩法）
- 🥽 **XR 开箱即用**：内置 WebXR，AR/VR 沉浸式应用直接写
- ⚛️ **物理引擎**：完整集成 ammo.js 刚体物理
- 🏃 **动画系统**：基于状态机的角色动画，任意场景属性也能做动画
- 🎮 **全套输入**：鼠标、键盘、触屏、游戏手柄 API 齐全
- 🔊 **3D 空间音效**：基于 Web Audio API
- 📦 **资产流式加载**：glTF 2.0 + Draco + Basis 压缩，异步加载大场景不卡
- 📜 **TS/JS 写逻辑**：TypeScript 或 JavaScript 都行
- **大厂背书**：Disney、BMW、King、Miniclip、Samsung、Snap 等都在生产环境用

生态全家桶：

| 包 | 说明 |
|---|---|
| `playcanvas` | 核心引擎（本仓库） |
| `@playcanvas/react` | React 渲染器 |
| `@playcanvas/web-components` | Web Components 声明式 3D |
| `create-playcanvas` | 项目脚手架 CLI |
| PlayCanvas Editor | 浏览器可视化编辑器 |

## 安装部署

```bash
# 只装引擎
npm install playcanvas

# 或者一步脚手架整个项目（推荐新手）
npm create playcanvas@latest
```

## 使用方法

官方 Hello World：一个旋转的立方体（注释已翻成中文）：

```js
import {
  Application,
  Color,
  Entity,
  FILLMODE_FILL_WINDOW,
  RESOLUTION_AUTO
} from 'playcanvas';

// 创建画布并塞进页面
const canvas = document.createElement('canvas');
document.body.appendChild(canvas);

// 新建应用实例
const app = new Application(canvas);

// 画布铺满窗口、自动分辨率
app.setCanvasFillMode(FILLMODE_FILL_WINDOW);
app.setCanvasResolution(RESOLUTION_AUTO);

// 窗口尺寸变化时重设画布
window.addEventListener('resize', () => app.resizeCanvas());

// 创建立方体实体
const box = new Entity('cube');
box.addComponent('render', { type: 'box' });
app.root.addChild(box);

// 创建相机实体
const camera = new Entity('camera');
camera.addComponent('camera', { clearColor: new Color(0.1, 0.2, 0.3) });
app.root.addChild(camera);
camera.setPosition(0, 0, 3);

// 创建方向光实体
const light = new Entity('light');
light.addComponent('light');
app.root.addChild(light);
light.setEulerAngles(45, 0, 0);

// 每帧按时间差旋转立方体
app.on('update', dt => box.rotate(10 * dt, 20 * dt, 30 * dt));

app.start();
```

想在线玩这段代码？官方 CodePen：https://codepen.io/playcanvas/pen/NPbxMj

## 本地开发构建（想改引擎源码时）

需要 Node.js 18+：

```bash
npm install        # 装依赖
npm run build      # 构建所有引擎变体 + 类型声明 → build 目录
npm run docs       # 构建 API 文档 → docs 目录
```

## 学习资源

- 用户手册：https://developer.playcanvas.com/user-manual/engine/
- API 参考：https://api.playcanvas.com/engine/
- 官方示例库：https://playcanvas.com/examples/
- 官方中文 README：https://github.com/playcanvas/engine/blob/main/README-zh.md
- 精选项目合集：https://github.com/playcanvas/awesome-playcanvas
- 论坛：https://forum.playcanvas.com

## 相关链接

- 原仓库：https://github.com/playcanvas/engine
- 协议：MIT，本项目中文整理仅作学习用途，版权归原作者所有
