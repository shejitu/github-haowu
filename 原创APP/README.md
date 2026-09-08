# 原创APP（original-apps）

> 这里收录自己原创创作的安卓 APP，全部由 AI 辅助开发、GitHub Actions 云端编译 APK。
> 三个 APP 各自有独立源码仓库，本页做统一汇总介绍。

| APP | 客户/用途 | 技术架构 | 源码仓库 |
|---|---|---|---|
| 海帝门窗门店展示 | 海帝门窗门店离线展示 + 选配报价 | 原生 Kotlin | [shejitu/HaidiDoorApp](https://github.com/shejitu/HaidiDoorApp) |
| 广西品硕展示 | 广西品硕建筑装配科技公司展示 | WebView 混合（HTML/CSS 本地打包） | [shejitu/GuangxiPinshuoApp](https://github.com/shejitu/GuangxiPinshuoApp) |
| 云尚幕墙维保 | 云尚幕墙维保公司离线展示 | WebView 混合（HTML/CSS 本地打包） | [shejitu/YunshangWallApp](https://github.com/shejitu/YunshangWallApp) |

---

## 1. 海帝门窗门店展示 APP

**[源码仓库](https://github.com/shejitu/HaidiDoorApp)** · 原生 Kotlin · 完全离线运行

给海帝门窗做的门店平板 APP：产品展示 + 定制选配 + 实时报价，一台平板全部搞定，不需要联网。

### 主要功能

- **首页**：品牌区 + 轮播图 + 四大入口 + 热门产品推荐
- **产品展示**：分类 Tab + 网格列表 + 详情页（图片画廊/简介/规格/视频入口）
- **视频中心**：分类浏览 + 本地 MP4 全屏播放
- **定制选配器**：型材/玻璃/五金/密封/颜色/纱窗逐项选配，价格实时联动
- **报价单**：完整明细 + 截图保存 + 分享微信
- **后台管理**：密码保护（初始密码 `0000`），可改密码、增删改产品、在线调价格

### 数据与媒体（完全离线）

APP 在平板 `内部存储/haidi_door/` 下自动建目录并播种默认配置，店员用数据线把图片/视频拷进对应目录即可：

```
haidi_door/
├── config/        # 产品/分类/视频/价格等 JSON 配置
├── media/
│   ├── images/    # 产品图 | 轮播图 | 工厂图
│   └── videos/    # 工厂 | 工艺 | 案例 (MP4/H.264)
└── data/quotes/   # 报价单截图
```

### 技术栈

Gradle 8.7 + AGP 8.5.2 + JDK 17 + Kotlin 1.9.24，minSdk 21 / targetSdk 34。

---

## 2. 广西品硕建筑装配科技 展示 APP

**[源码仓库](https://github.com/shejitu/GuangxiPinshuoApp)** · WebView 混合架构 · 离线展示

给广西品硕建筑装配科技做的公司形象展示 APP，把公司资料、业绩、证书全部装进一台平板，业务员谈客户时随时翻给客户看。

### 主要模块

- **公司简介**：含"一套团队 · 多块牌子"业务关系说明
- **服务能力**：设计服务（全过程）+ 施工服务（六大业务、七项保障、五项服务承诺）
- **精选业绩 + 奖项荣誉证书**：电子证书墙，高清大图浏览
- **合作流程 / 为什么选择我们**：标准化销售话术展示
- **后台管理**：内置管理入口，内容可维护

### 技术架构

本地 HTML/CSS/图片资源打包进 APK 的 `assets/www`，WebView 加载，无任何网络依赖；GitHub Actions 自动编译 APK。

---

## 3. 云尚幕墙维保 离线展示 APP

**[源码仓库](https://github.com/shejitu/YunshangWallApp)** · WebView 混合架构 · 离线展示

给云尚幕墙维保公司做的业务展示 APP，聚焦幕墙维保专业能力呈现，工地谈单、投标演示都用得上。

### 主要模块

- **业务范围 + 标准化服务流程**：专业维保能力全展示
- **成功案例集锦 + 客户评价**：实景对比图（施工前/后特写）
- **专业施工要点**：高空外墙清洁（按材料精准作业）、玻璃更换施工要点
- **安全管理与法规遵循**：检修流程、安全性评估服务
- **幕墙知识问答 / 公司实力 / 组织架构**：信任状内容一应俱全

### 技术架构

同广西品硕：本地 HTML/CSS 资源打包 + WebView 加载，完全离线；GitHub Actions 自动编译 APK。

---

## 共性说明

- **三个 APP 全部离线运行**，不依赖网络，适合门店/工地/演示等无网环境
- **全部通过 GitHub Actions 云端编译**，本机无需安装 Android 开发环境，push 即出 APK
- 各仓库的 `apk` 分支可直接下载最新 APK
