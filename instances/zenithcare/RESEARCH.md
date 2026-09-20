# Kage 研究索引

这个 fork 保留 `MengTo/kage` 的完整提交历史，并增加一套面向工程复盘与原创重建的中文研究资料。研究基线为上游提交 `4399487d2fb42bce39c7b032fbbb50d230bf4f0b`。

## 从这里开始

1. [深度分析](docs/research/deep-analysis.zh-CN.md) — 视觉分层、镜头、鼠标反馈、卡片织物、后期与性能降级。
2. [实现解剖](docs/research/implementation-anatomy.zh-CN.md) — 从启动到渲染循环的源码级数据流与状态机。
3. [原创标准库方案](docs/research/clean-room-standard-library.zh-CN.md) — 如何把机制抽象成可用于客户项目的新实现。
4. [使用与许可边界](docs/research/USAGE-NOTICE.zh-CN.md) — fork、研究、商用和再分发分别意味着什么。
5. [实测记录](docs/research/verification-record.zh-CN.md) — 本地运行、画布数量、交互状态和验证证据。
6. [知识图谱 JSON](docs/research/knowledge-graph.json) — 21 个文件节点、3 个架构层与 7 步阅读导览。

## 一句话结论

Kage 不是“把三维背景塞进 HTML”，而是让一个固定 WebGL 世界、HTML 编辑排版、透明前景、独立卡片画布和后期调色共同服从滚动摄影机的网页镜头系统。

## 研究范围

- 程序化寺院、灯光、水面、雾雨、余烬与落叶；
- 六机位 Catmull–Rom 摄影机轨道与滚动映射；
- DOM/Three.js 混合层级和章节前景接管；
- 鼠标光标、镜头视差、粒子轨迹与世界照明联动；
- 97×97 节点卡片织物模拟；
- bloom、grain、vignette、色调与自适应渲染；
- 响应式、减少动画、粗指针、低性能和 WebGL 失败路径；
- 可商用原创标准库所需的 clean-room 边界。

## 重要边界

GitHub 的公开仓库功能允许用户查看和 fork，但这不等于获得作品的商用许可证。上游 README 明确写明原始 Kage 代码和美术未授予复用或再分发许可。任何客户项目都应先取得书面授权，或者依照本研究进行独立重写并使用全新美术资产。

官方依据：

- [上游 Kage License 声明](https://github.com/MengTo/kage#license)
- [GitHub：Licensing a repository](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/licensing-a-repository)

本目录是技术研究记录，不构成法律意见。
