# 目标跟踪（Object Tracking）领域常识

> 用途：判断现有 SOTA 大致水平、新提案该与谁对比、用哪些数据集和指标。
> ⚠️ 具体数值随版本/训练设置波动，作为量级参考；涉及精确数值请提示用户核实。

## 任务划分
- **单目标跟踪 SOT**：首帧给定框，后续帧定位同一目标。
- **多目标跟踪 MOT**：检测 + 跨帧关联多个目标，维持 ID。
- **视觉语言跟踪 / VLT**：文本描述辅助跟踪。
- **分割跟踪 / VOS**：输出 mask（与视频分割交叉）。

## 经典 baseline / 里程碑方法
**单目标（SOT）**
- 相关滤波时代：KCF、ECO（轻量、快）。
- Siamese 系：SiamFC → SiamRPN → SiamRPN++ → SiamMask（孪生匹配范式，模板-搜索结构高频被迁移）。
- Transformer 跟踪：TransT、STARK、MixFormer、OSTrack、SeqTrack、ARTrack —— 当前 SOT 强 baseline，多基于 ViT one-stream。
- 记忆/时序：把视频分割的记忆机制引入跟踪是常见思路。

**多目标（MOT）**
- tracking-by-detection：SORT → DeepSORT（卡尔曼 + 外观）→ ByteTrack（用低分框关联，简单强力，常用 baseline）→ OC-SORT、BoT-SORT。
- joint detection & tracking：FairMOT、CenterTrack、TransTrack、MOTR（端到端 Transformer）。

## 常用数据集
**SOT**
- **LaSOT**：大规模长时跟踪，报 AUC / Precision / Norm. Precision；长序列子集常用于"长时跟踪"评测。
- **GOT-10k**：泛化评测（训练/测试类别不重叠），报 AO / SR。
- **TrackingNet**：大规模，报 AUC / P / Pnorm。
- **OTB-100 / OTB-2015**：较老，AUC（成功率曲线）。
- **TNL2K、OTB-Lang**：视觉语言跟踪。
- **VOT**：短时鲁棒性挑战（EAO）。

**MOT**
- **MOT17 / MOT20**：行人，MOT20 更密集拥挤。
- **DanceTrack**：相似外观、复杂运动，考验关联。
- **BDD100K MOT**：自动驾驶多类。
- **TAO**：大词表长尾 MOT。

## 评价指标
**SOT**
- **AUC / Success（成功率曲线下面积）**、**Precision / Norm. Precision**。
- **GOT-10k：AO（平均重叠）、SR0.5/SR0.75**。
- **VOT：EAO（期望平均重叠）**。
- **FPS**：实时性，提"提速"创新时要报。

**MOT**
- **HOTA**（当前主推，平衡检测与关联）、**MOTA**（受漏检/误检主导）、**IDF1**（身份一致性）、**ID Switches、MT/ML、FP/FN**。

## 常见可挖掘的痛点（找创新点的方向）
- **长时跟踪漂移**（→ 记忆机制、模板更新策略、重检测）。
- **遮挡 / 目标消失再现**（→ 轨迹补全、置信度感知更新、re-id 增强）。
- **相似目标/干扰物**（distractor，→ 判别性特征、负样本挖掘）。
- **快速运动、运动模糊**（→ 运动建模、更大搜索域）。
- **实时性 vs 精度**（→ 轻量 one-stream、token 剪枝、蒸馏）。
- **MOT 密集拥挤下的关联错误、ID switch**（→ 更鲁棒的运动/外观融合、图匹配）。
- **跨域/开放场景泛化、视觉语言对齐**。
