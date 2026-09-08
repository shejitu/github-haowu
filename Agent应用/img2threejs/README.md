# img2threejs — 图片一键重建为 Three.js 3D 模型（AI Agent 技能）

> ⭐ 15.5k+ Stars | Python | Apache-2.0 | [原仓库](https://github.com/img2threejs/img2threejs) | [在线演示](https://img2threejs.io/)

## 一句话简介

给它一张物体的参考图片，它让 AI 编程助手（Claude Code / Codex / OpenCode）**用纯代码**把图中的物体重建成一个程序化生成的 Three.js 3D 模型——不是照片建模、不是网格提取、不是下载素材包，而是一段可以直接跑在浏览器里的 TypeScript 代码。

## 核心亮点

- **纯代码重建**：输出可 diff、可版本管理的 TypeScript 代码 + JSON 规格文件，而不是几 MB 的网格文件
- **动画就绪**：生成的模型自带枢轴（pivots）、插槽（sockets）、碰撞体，角色还带骨骼和蒙皮，拿来就能做动画
- **质量门控**：分阶段流水线（粗坯 → 结构 → 外形 → 材质 → 表面 → 光照 → 交互 → 优化），每一步都拿"参考图 vs 渲染图"对比打分，不过关不放行
- **极致省 token**：所有机械工作（校验、打分、打包）都由 Python 脚本完成，模型只干"看图判断像不像"这一件事
- **零依赖**：核心脚本纯 Python 3.10+ 标准库，不用 pip 装任何东西，PNG 读写都是手搓的 `struct` + `zlib`
- **领域插件生态**：CS2 武器皮肤、动画角色、导出 GLB 等都是独立插件（`img2 add` 安装），还能自己写插件
- **诚实面对局限**：单张图看不到的背面就用镜像推断并标记低置信度，绝不假装精确

## 安装部署

放进你的 AI 助手技能目录即可（无其他依赖）：

```bash
# Claude Code 用户
git clone https://github.com/img2threejs/img2threejs.git ~/.claude/skills/img2threejs

# Windows 下通常在（Git Bash 路径写法）
# git clone https://github.com/img2threejs/img2threejs.git /c/Users/<用户名>/.claude/skills/img2threejs
```

如果同时用多个助手（Claude Code + Codex），保持一份检出，其他入口用符号链接指向它，避免版本漂移：

```text
~/.claude/skills/img2threejs -> <你的检出目录>
~/.codex/skills/img2threejs  -> <你的检出目录>
```

可选：安装插件框架和领域插件（比如 CS2 皮肤专用管线）：

```bash
npx github:img2threejs/img2 install     # 安装 img2 插件框架
img2 add img2threejs/plugin-cs2         # 添加 CS2 武器皮肤插件
img2 doctor                             # 体检：审计所有已装插件
```

官方插件一览：

| 插件 | 增加的能力 | 安装命令 |
|---|---|---|
| plugin-cs2 | CS2 武器皮肤重建（家族适配器、涂装规则、专属审查门） | `img2 add img2threejs/plugin-cs2` |
| plugin-character | 动画角色（骨骼绑定 + 动画门控） | `img2 add img2threejs/plugin-character` |
| plugin-img2glb | 经云端 TRELLIS 导出 GLB 文件 | `img2 add img2threejs/plugin-img2glb` |
| plugin-hello-cube | 最小参考插件，照着写自己的 | `img2 add img2threejs/plugin-hello-cube` |

## 使用方法

### 最简单用法

在 Claude Code 里附加一张物体图片，然后说：

```
/img2threejs 把这个物体重建为 Three.js 模型，保持比例、角度和颜色。
```

技能会自动完成：物体分类 → 细节清单 → 分阶段生成 → 每步图文对比审查，直到渲染结果和参考图对上。

### 高级控制（Prompt 模板，中文版）

想精确控制质量标准，可以这么说（每一行都对应流水线里真实存在的门控，不是空话）：

```
/img2threejs 把图中的主体重建为程序化 Three.js 模型。

保真度    严格按参考图保持轮廓和比例。先列出所有"身份定义级"细节——倒角圆角、
          板缝、螺丝铆钉、刻线涂装线条、亮面/哑光分区、磨损——凡是落不到真实
          部件上的细节宁可删掉，不许造假。
材质      根据参考图像素推导涂装类别和渐变色标，不许凭记忆猜。会经不起
          色调映射的颜色要标记出来。
运行时    为需要动的部分暴露枢轴和插槽，加一个 userData.tick 做循环待机动画。
门控      跑 --strict-quality，图文对比审查不过关就不进入下一阶段。
          图上看不出来的区域要报告逐区域置信度。
```

### 多会话续做（大工程分次跑）

```bash
# 初始化本地状态（记录清单和证据）
python3 forge/state.py init --reference <参考图> --profile character --spec object-sculpt-spec.json

# 从上次进度继续
python3 forge/next.py --state .img2threejs/state.json
```

### 产物是什么

- 一个 `ObjectSculptSpec` JSON：完整部件树、材质、重复系统、插槽，以及每个阶段的审查记录
- 一个 TypeScript 工厂函数 `createObjectNameModel(spec, options)`，返回 `THREE.Group`，运行时数据挂在 `root.userData.sculptRuntime`
- 角色类还额外带 `root.userData.rig`（骨骼、共享 Skeleton、骨骼索引表）
- 每阶段的渲染图和对比图，完整记录还原度

## 适用与不适用

| 适合 | 不适合 |
|---|---|
| 硬表面物体（道具、武器、载具、家居） | 追求照片级人物复刻（角色是风格化重建） |
| 游戏资产、可动画角色、等距小场景 | 需要精确毫米级几何的工程用途 |
| 想要可维护的代码资产而非二进制网格 | 图片信息不足时（它会直接告诉你"这张图达不到要求的保真度"） |

## 相关链接

- 原仓库：https://github.com/img2threejs/img2threejs
- 在线演示画廊：https://img2threejs.io/ （所有模型都是生成代码，浏览器里直接跑）
- 展示画廊源码：https://github.com/img2threejs/img2threejs-showcase
- 插件框架：https://github.com/img2threejs/img2
- 协议：Apache License 2.0，本项目中文整理仅作学习用途，版权归原作者所有
