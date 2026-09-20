# 视觉素材计划

## 素材数量

- 新制内容图：**14 张**
- 场景图：4 张
- 透明前景图：10 张
- 视频：0 条
- 臻晨 Logo：使用现有文件，不计入新制数量

## 视觉方向

- 明亮、安静、有人情味的健康服务空间
- 画面重点是人、沟通、报告和持续服务
- 主色取自臻晨 Logo 与现有 UI：`#0F8F72`
- 深绿：`#0B6F5D`、`#095B50`
- 浅底：`#F5F8F3`、`#F0F4F4`、`#FFFFFF`
- 蓝色只作专业信息点缀：`#2B5E95`、`#2A70B6`
- 暖色只作少量提示：`#8B5B2B`、`#E08300`

## 场景图

| 编号 | 原位文件 | 严格尺寸 | 新画面 | 构图要求 |
| --- | --- | ---: | --- | --- |
| BG-01 | `secret-pathways-assets/generated/kage-sanmon-preview.webp` | 1586 × 992 | 明亮的臻晨接待或咨询空间 | 人物与服务动作偏右，左侧留出干净区域；自然日光，绿色只作识别色 |
| BG-02 | `secret-pathways-assets/generated/kage-approach.webp` | 1536 × 1024 | 工作人员与来访者共同查看报告 | 横向构图，人物偏右；报告可见但不能出现可读隐私内容 |
| BG-03 | `secret-pathways-assets/generated/kage-lantern-court.webp` | 1024 × 1536 | 面对面说明报告或服务安排 | 竖向景深，表情自然；上方与下方都保留呼吸空间 |
| BG-04 | `secret-pathways-assets/generated/kage-moonwater.webp` | 1536 × 1024 | 随访、记录或团队交接 | 明亮诊室、服务台或走廊；画面稳定，不做科技展厅感 |

## 透明前景图

| 编号 | 原位文件 | 严格尺寸 | 新素材 | 复用要求 |
| --- | --- | ---: | --- | --- |
| FG-01 | `secret-pathways-assets/foreground/png/temple-wall.webp` | 1536 × 884 | 浅色服务空间墙体、玻璃隔断或前台边框 | 左右翻转后仍自然；底部承重，边缘干净 |
| FG-02 | `secret-pathways-assets/foreground/png/pine-tree.webp` | 1024 × 1438 | 高挑的室内绿植或窗边树影 | 主体纵向，根部落在底边，右侧进入画面 |
| FG-03 | `secret-pathways-assets/foreground/png/tall-grass.webp` | 1717 × 916 | 横向浅绿植物群 | 可在多章节底部重复使用，不能有明确场所文字 |
| FG-04 | `secret-pathways-assets/foreground/png/sakura-branch.webp` | 1536 × 1024 | 明亮窗边枝叶 | 适合从左侧伸入，也能用于收尾章节 |
| FG-05 | `secret-pathways-assets/foreground/png/maple-leaves.webp` | 1536 × 1024 | 日光下的轻薄绿叶 | 从右侧进入，避免厚重暗部 |
| FG-06 | `secret-pathways-assets/foreground/png/stone-lantern.webp` | 1024 × 1499 | 立式导诊标识或简洁服务终端轮廓 | 不带屏幕文字、品牌或设备型号 |
| FG-07 | `secret-pathways-assets/foreground/png/garden-bush.webp` | 1717 × 876 | 低矮绿植或柔和空间陈设 | 能用于卡片区与页脚，不遮挡中心文字 |
| FG-08 | `secret-pathways-assets/foreground/png/basalt-stones.webp` | 1536 × 996 | 浅色圆润石材或低矮空间摆件 | 适合底部压边，不能形成暗黑景观 |
| FG-09 | `secret-pathways-assets/foreground/png/hill.webp` | 1774 × 887 | 明亮的园区、城市绿地或远景轮廓 | 宽幅低地平线，用于收尾背景下缘 |
| FG-10 | `secret-pathways-assets/foreground/png/shrine-ruins.webp` | 1536 × 1001 | 臻晨服务空间、廊架或建筑剪影 | 轮廓简洁，从左侧进入，不含招牌文字 |

## 现成素材

Logo 直接使用：

`C:\Codexstorage\LClinic\z12b\tenant\imported\design\brand\selected\zhenchen-health-round-tree-logo-512.png`

Logo 不重新生成，不嵌入场景图。

## 制作规则

- 14 张图全部导出为 WebP，并覆盖到原文件路径与原文件名
- 每张图必须与上表像素完全一致，不改变宽高比
- FG-01 至 FG-10 必须保留透明通道
- 所有画面使用 sRGB
- 文字安全区沿用 Kage 原布局，不把人物面部、手部或关键动作放在文字下方
- 图内不放标题、Logo、二维码、机构名、设备型号或可读报告内容
- 不出现夜景、霓虹、蓝色科技网格、DNA、机器人医生或未来实验室
- 原 WebGL 夜景显示层只隐藏，不删除 Three.js、摄影机、滚动和动画代码
- 不照搬参考资料中的人物、场所、设备、页面或其他机构标识
- 不通过前端拉伸、运行时裁切或改宽高比补救素材
- 原 HTML 结构、class、data 属性、摄影机、滚动和动画保持不动；多余视觉元素只隐藏

## 制作顺序

1. 先完成 BG-01，验证整体明度、肤色和品牌绿色
2. 同一人物与空间风格完成 BG-02 至 BG-04
3. 按页面实际叠放位置制作 FG-01 至 FG-10
4. 用原文件名覆盖后检查桌面端与手机端
5. 尺寸、透明通道和加载结果全部通过后，再进入页面验收

## 验收

- 内容图数量为 14 张，页面不请求新增图片路径
- 14 张图的宽度和高度逐一与上表一致
- 10 张前景图透明边缘无白边、黑边和矩形底
- 桌面端与手机端没有拉伸、错位、遮字和关键主体被裁掉
- 所有图片成功加载，无 404
- 页面不再出现寺院、京都、夜游和暗夜氛围素材
- 品牌绿来自 Logo 与 UI，画面保持真实、明亮、克制
