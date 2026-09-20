# Kage 实现解剖

## 1. 总体数据流

```text
DOMContentLoaded
  → boot()
  → 12 个 JOBS 分阶段构建世界
  → start()
  → measure() 计算章节锚点
  → requestAnimationFrame(frame)

scrollY
  → progressFor()
  → RIG.prog
  → 阻尼得到 RIG.smooth
  → applyCamera()
  → updateWorld()
  → render()
  → renderPost()
```

页面从来没有把章节替换成独立 3D 场景。章节只改变同一个世界中的摄影机位置、观察目标、FOV、DOM 前景归属和内容状态。

## 2. 启动系统

`JOBS` 把重建过程拆成 12 个可观察阶段：字体、地面、外壳、寺院、月亮、鸟居、岩石/灯笼、枫树、三维近景、字标、大气、后期/卡片。每完成一个任务就更新预加载进度，阶段之间让出约一帧，避免长时间冻结页面。

若初始化渲染器或场景的早期任务失败，`fallback()` 会：

- 标记 `.no-webgl`；
- 解锁页面滚动；
- 展示 CSS 静态背景和 DOM 字标；
- 强制完成内容 reveal；
- 记录可检查的失败状态。

这使内容阅读不依赖 WebGL 成功。

## 3. 世界构建

核心对象均由运行时代码生成：

| 子系统 | 入口 | 方法 |
|---|---|---|
| 地形/水面/远山 | `buildShell()` | Plane/Box 几何、程序纹理、雾与反射暗示 |
| 大殿 | `buildTemple()` | 柱网、屋顶、障子、平台与台阶组合 |
| 月亮 | `buildMoon()` | 主圆盘、halo 和暖色点光源 |
| 鸟居与灯笼 | `buildTorii()` / `buildLantern()` | 简化几何重复构件 |
| 枫树 | `buildMaple()` | 可复用枝干与叶片群 |
| 三维近景 | `buildForeground()` | 六张程序化 alpha 平面分布在不同 z 深度 |
| 巨型字标 | `buildWordmark()` | 每个字母单独画布纹理与独立 Mesh |

巨型 `KAGE` 不是 HTML 文字。四个字母各自拥有纹理与材质，可从草后升起，再在摄影机靠近时逐字溶解。这样字标能被三维草和岩石真实遮挡。

## 4. 摄影机轨道

六个 `CAM` waypoint 分别保存：

- 摄影机位置 `p`；
- 观察目标 `t`；
- 视场角 `fov`。

位置和观察目标各自形成 Catmull–Rom 曲线。`progressFor(scrollY)` 先根据真实章节中心把滚动位置变成 `0..5` 的分数进度，再由 `damp()` 消除机械跟随。

指针视差在正式机位上只增加小幅偏移：摄影机向指针方向轻移，观察目标反向轻移，形成真实空间的相对视差。竖屏通过 `aspectFix()` 沿视线后退并扩大 FOV，而不是简单裁切桌面镜头。

## 5. 三套“前景”不可混为一谈

### 5.1 Three.js 近景

`buildForeground()` 生成六个带 alpha 的平面，分布在摄影机前方不同深度。摄影机穿过它们时，透明度会根据相机与平面的 z 距离下降，避免近景突然切过镜头。

### 5.2 章节 HTML 前景

每个章节拥有一组真实透明 WebP。`wireForegroundStages()` 的状态机是：

```text
parked in section
  → IntersectionObserver 选出占比最高章节
  → lift: reparent 到 #fg-sky
  → fg-active: 分批上升/淡入
  → 下一个章节激活
  → fg-retiring: 下沉 + scale(.98) + blur(18px) + 淡出
  → 820ms 后 park 回原章节
```

重新挂载到 `#fg-sky` 的原因不是便利，而是突破 `.page` 的 stacking context，使枝叶能真正覆盖固定导航和章节轨道。

### 5.3 粒子近景

雨、落叶、余烬和鼠标微粒使用不同图层。部分对象只对主摄影机可见，不进入卡片镜头或水面反射，避免同一个视觉事件在错误空间重复。

## 6. 鼠标输入总线

一次 pointer move 会写入归一化坐标 `RIG.tmx/tmy`，之后分发到：

1. `wireCursor()`：26px 圆环以 0.18 lerp 追赶鼠标；交互区扩到 52px。
2. `applyCamera()`：摄影机与观察目标产生反向微视差。
3. `updateWisps()`：摄影机坐标系中的冷色微粒按移动距离发射。
4. 卡片 canvas：指针位置被弹簧平滑后写入布料压痕。
5. `wireFocus()`：章节和课程悬停改变世界灯光 focus。

`RIG.focusAmt` 不是开关，而是阻尼到目标值；灯笼、大殿灯、月晕因此不会突然跳亮。

## 7. 卡片织物求解器

每张卡片创建一个 WebGL2 canvas，使用 `CL_SEG=96`，即 `97×97=9,409` 个模拟节点。求解流程包括：

- 高度场双缓冲；
- 邻点有限差分传播；
- 阻尼、风、阵风和垂坠；
- 顶边固定；
- 鼠标 brush imprint；
- 法线估算、折痕受光、sheen、边缘光、接触阴影；
- 圆角和边框在 shader 内随布面一起变形。

卡片原图及两层暗部 scrim 先烘焙到同一 plate，保证所有视觉元素共同起伏。ResizeObserver 负责重建纹理尺寸，IntersectionObserver 停止屏外求解，页面隐藏时暂停 raf。

## 8. 大气系统

- 雾：若干 additive billboard 缓慢横移并面向摄影机。
- 余烬：点精灵在高度区间循环上升，大小按景深缩放。
- 雨：LineSegments 在 shader 中循环下落。
- 水波：透明环形平面在水面随机重生、扩散和淡出。
- 落叶：InstancedMesh；每片叶子独立下落、摆动、滚转，并在摄影机前方回收。
- 鼠标微粒：挂在摄影机下，保持屏幕空间跟手，不随世界镜头漂走。

落叶重生区位于摄影机视线前方，而不是简单以摄影机为圆心；同样粒子数能在屏幕中保持更高有效密度。

## 9. 后期管线

```text
scene + card viewports
  → half-float scene target
  → brightness extraction
  → 4 级降采样
  → 每级横向/纵向 blur
  → 从小到大 additive upsample
  → scene + bloom compose
  → 冷阴影/暖高光 + vignette + grain + gamma + contrast + fade
```

四级 bloom 同时提供小范围强光和大范围空气辉光。它只提取亮部，使障子、灯笼、月亮和粒子发光，而屋檐继续保持剪影。

## 10. 性能治理

- DPR 上限默认 1.8，低质量模式 1.4；
- 统计真实帧耗时，平均慢于约 23ms 时降低 render scale；
- render scale 最低约 0.55；
- 性能下降时优先取消多重采样；
- 阴影只烘焙一次，因为投影物不移动；
- 卡片视口按 dirty 状态和轮转刷新，不每帧全部重绘；
- 低质量模式减少雨、雾、余烬、水波、落叶与微粒；
- coarse pointer 不创建织物、鼠标微粒和自定义光标；
- `prefers-reduced-motion` 取消动画但不隐藏内容；
- document hidden 时暂停主循环。

## 11. 可维护性判断

优点是零构建、资产本地化、容易托管和完整可携带；缺点是 4,800 多行单文件把排版、世界、shader、交互和性能策略全部耦合。若要做标准库，应保留机制，重新设计模块边界，不能继续复制单文件结构。
