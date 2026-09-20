# 原创网页电影标准库方案

## 目标

把 Kage 值得借鉴的“机制”抽象为一个可在客户项目中使用的新库，同时不复制 Kage 的未授权源码、文案、品牌、场景图、透明素材、几何参数或具体镜头数据。

这是一条 clean-room（独立重写）路线：研究人员记录行为与设计原理，实施者依据公开行为规格重新设计 API、数据结构、代码和资产。

## 禁止直接继承的内容

- Kage `index.html` 的代码表达、shader 文本和具体常量；
- KAGE 名称、字标、文案、日文组合和章节叙事；
- 四张 generated plate 与十张透明前景；
- 原始寺院构图、镜头坐标、色值组合和素材裁切；
- 暗示上游授权、合作或背书的说明。

## 可以研究并重新实现的抽象思想

- 滚动进度映射连续摄影机轨道；
- WebGL 环境层与语义 HTML 内容层组合；
- 章节拥有并临时接管固定近景；
- 一个输入事件驱动局部 UI 和全局世界反馈；
- 可选织物/液体/折射式媒体卡片；
- bloom、颗粒和色调统一异构素材；
- 性能、触屏、减少动画和 WebGL 失败降级。

## 建议模块

```text
packages/
  core/
    StoryEngine
    ChapterTimeline
    CameraTrack
    InteractionBus
    AssetManifest
  three-adapter/
    WorldAdapter
    CameraRig
    LayerMaskRegistry
    PostPipeline
    PerformanceGovernor
  dom-story/
    ForegroundStageController
    RevealController
    NavigationController
    AccessibilityAdapter
  effects/
    PointerTrail
    ParticleField
    ClothCard
  authoring/
    schema
    validators
    shot-preview
  examples/
    original-demo-a/
    original-demo-b/
```

## 核心协议

### `StoryManifest`

```ts
interface StoryManifest {
  id: string;
  palette: ThemeTokens;
  chapters: ChapterSpec[];
  assets: AssetManifest;
  quality: QualityPolicy;
  accessibility: AccessibilityPolicy;
}
```

### `ChapterSpec`

```ts
interface ChapterSpec {
  id: string;
  anchor: string;
  camera: CameraKeyframe;
  foreground?: ForegroundAsset[];
  worldState?: Record<string, number>;
  enter?: TransitionSpec;
  leave?: TransitionSpec;
}
```

### `InteractionBus`

统一发出 `pointer`, `focus`, `chapter`, `quality`, `visibility`, `motion-preference` 事件。DOM、摄影机、灯光、粒子和媒体卡片订阅同一状态，而不是互相直接调用。

## 运行时状态机

```text
BOOTING
  → READY
  → ACTIVE(chapterId)
  ↔ TRANSITION(from, to, progress)
  ↔ SUSPENDED(hidden)
  → FALLBACK(no-webgl)
```

前景层单独使用：

```text
PARKED → ENTERING → ACTIVE → RETIRING → PARKED
```

## 美术资产契约

每个项目必须提供原创资产 manifest：

- 场景 plate：WebP/AVIF，显式宽高、焦点、光源方向和许可来源；
- alpha 前景：透明通道、底边锚点、推荐深度、移动端替代项；
- 字体：名称、字重、格式、许可证；
- 3D 资源：来源、优化级别、纹理预算和归属；
- 每个文件必须有 `license`, `author`, `source`, `clientProject` 元数据。

构建时若缺少许可元数据，应直接失败，不能只警告。

## 质量预算

建议默认预算：

- 首次可见画面 < 2.5s（中档移动网络作为目标，而非硬保证）；
- 初始关键资产 < 3MB；延迟资产按章加载；
- 桌面目标 60fps，移动端目标 30/45fps 可配置；
- 主渲染 DPR 上限 1.5–2.0；
- 同时存活的 WebGL context 数可配置，默认不超过 2；
- 屏外效果必须暂停；
- 所有动画必须提供 reduced-motion 等价阅读状态；
- WebGL 失败时仍能读完整文案和导航。

## 客户项目验收清单

1. 品牌、文案、美术、字体和音乐的权利链完整。
2. 每章有明确镜头目的，不用装饰性滚动填时间。
3. 鼠标/触摸/键盘均能完成核心任务。
4. 前景不会挡住必要按钮，装饰图片全部 inert/aria-hidden。
5. 390×844、768×1024、1440×900 实测。
6. reduced-motion、coarse pointer、低性能和 no-WebGL 状态实测。
7. 控制台无错误，所有资产 200，离线/弱网行为有定义。
8. 性能退化顺序固定：MSAA → DPR → 粒子 → 阴影 → 后期。
9. 不把 Kage 的素材或代码混入发布包。

## 建议实施顺序

1. 先写与品牌无关的行为规格和测试场景。
2. 建立核心状态机、摄影机轨道与章节控制器。
3. 接入 DOM 前景与响应式排版。
4. 加入 post pipeline 和 PerformanceGovernor。
5. 最后把 PointerTrail、ClothCard 等作为可选插件。
6. 用两个完全不同题材的原创示例验证库不是 Kage 专用模板。

最终标准库应以自己的名称、许可证、API、测试、原创 demo 和资产审计工具发布。取得上游书面许可之前，不应把当前 fork 标记为可商用模板。
