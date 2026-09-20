# 悬浮详情摄影图生成方案 v3

## 生成方式

- 工具：Codex 内置 ImageGen
- 用例：`photorealistic-natural`
- 数量：17 张
- 输出：1024 × 1536，2:3 竖幅；接入页面前转为 WebP
- 位置：`instances/zenithcare/secret-pathways-assets/generated/details/`

## 共同提示

> 为臻晨健康网页悬浮详情框生成高真实感编辑摄影。空间统一为现代中国高端健康中心：浅灰石材、曲面玻璃、深绿色软装、自然植物、明亮中性日光。画面克制、真实、有生活感，不摆拍。人物和关键动作保持在中央 60% 安全区，使桌面端能完整显示竖幅，手机端横向裁切仍能看清主题。医生均为男性，女性工作人员均为深绿色制服护士。客户以举止、剪裁和从容感体现高端，不出现名表、豪车或品牌服装。无文字、Logo、水印、可读报告和悬浮界面。

共同排除：夜景、暖黄酒店感、蓝色科幻界面、机器人医生、夸张微笑、拥挤候诊、可读医疗数据、畸形手部与设备。

## 分镜提示

| 文件 | 内容与人物动作 |
| --- | --- |
| `detail-service-consult.webp` | 中年夫妻与女护士坐在咨询区沟通到店前最关心的问题，护士用平板记录。 |
| `detail-service-assessment.webp` | 女护士为约 60 岁女性测量血压并询问基础情况。 |
| `detail-service-arrival.webp` | 中年夫妻到店，女护士手持平板边走边引导。 |
| `detail-service-report.webp` | 男医生向年长夫妻当面说明影像资料。 |
| `detail-service-followup.webp` | 母亲、女儿与女护士一起查看后续安排。 |
| `detail-service-record.webp` | 女护士与中年男性在平板上回顾连续健康记录。 |
| `detail-service-review.webp` | 男医生与年长男性比较两次健康趋势。 |
| `detail-health-metabolic.webp` | 男医生陪同中年女性完成体成分评估并说明生活管理方向。 |
| `detail-health-cardio.webp` | 男医生为年长男性进行心脏超声，女护士协助。 |
| `detail-health-immunity.webp` | 女护士为年长女性进行常规采样，成年女儿陪伴。 |
| `detail-health-brain.webp` | 男医生陪同年长女性完成简单认知卡片评估，成年儿子在旁。 |
| `detail-health-lung.webp` | 女护士指导中年男性完成肺功能呼吸测试。 |
| `detail-health-digestive.webp` | 男医生借助简洁消化系统模型与夫妻沟通症状。 |
| `detail-health-musculoskeletal.webp` | 年长女性完成低台阶动作评估，男医生观察、女护士保护。 |
| `detail-health-reproductive.webp` | 夫妻在私密咨询室与男医生讨论不同人生阶段的健康需要。 |
| `detail-health-oral.webp` | 男牙医为男孩进行常规检查，父亲陪伴，女护士协助。 |
| `detail-health-eye.webp` | 男眼科医生为年长女性进行裂隙灯检查，成年女儿陪伴。 |

## 验收

- 17 张图片均为 1024 × 1536 WebP，文件名与页面内容键一一对应。
- 同一品牌空间与光线下，人物组合、设备、动作和景别不重复。
- 桌面端完整显示竖幅；390px 手机端裁切后仍能看清人物、设备或服务动作。
- 页面不再让多个详情复用同一张主页照片。
