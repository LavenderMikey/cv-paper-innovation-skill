# 目标检测（Object Detection）领域常识

> 用途：判断现有 SOTA 大致水平、新提案该与谁对比、用哪些数据集和指标。
> ⚠️ 具体数值随版本/训练设置波动，作为量级参考，不要当作精确事实直接写进报告；涉及精确数值请提示用户核实。

## 经典 baseline / 里程碑方法

**两阶段（two-stage）**
- R-CNN → Fast R-CNN → Faster R-CNN：RPN + RoI，精度高、速度较慢，至今仍是常用 baseline。
- FPN：特征金字塔，多尺度检测的标配组件，常作为模块被迁移。
- Cascade R-CNN：多级 IoU 阈值级联，提升高质量框的精度。

**单阶段（one-stage）**
- YOLO 系列（v3/v5/v7/v8/v10 等）：实时检测主力，速度/精度权衡好，工业落地多。
- SSD、RetinaNet（Focal Loss 解决正负样本不平衡，Focal Loss 是高频被迁移的组件）。
- FCOS、CenterNet：anchor-free，简化设计。

**Transformer 检测**
- DETR：集合预测 + 二分匹配，去掉 NMS 和 anchor；收敛慢是其著名局限。
- Deformable DETR：可变形注意力，大幅加速收敛、提升小目标。
- DINO、DN-DETR、Group-DETR：去噪训练/查询设计，DETR 系当前强 baseline。
- Grounding DINO：开放词表/文本引导检测。

## 常用数据集
- **MS COCO**：最主流，80 类，报告 mAP（AP@[.5:.95]）。
- **Pascal VOC**：较老，20 类，报 mAP@0.5。
- **LVIS**：长尾大词表（1000+ 类），考验长尾/稀有类。
- **Objects365**：大规模预训练常用。
- 专用：小目标（VisDrone、TinyPerson）、自动驾驶（BDD100K、nuImages）。

## 评价指标
- **mAP / AP**：核心指标，COCO 用 AP@[.5:.95]，并分 AP_S/AP_M/AP_L（小/中/大目标）。
- **AR**（平均召回）。
- **FPS / 推理延迟 / 参数量 / FLOPs**：落地与实时性指标，提"提速"类创新时要报。

## 常见可挖掘的痛点（找创新点的方向）
- 小目标检测掉点（→ 多尺度、高分辨率特征、上下文建模）。
- 密集/遮挡场景（→ 改进 NMS、query 去重、关系建模）。
- 长尾分布（→ 重采样、loss 重加权、解耦分类与回归）。
- DETR 收敛慢、训练贵（→ 查询初始化、去噪、匹配稳定性）。
- 域偏移/跨域泛化、半监督/少样本、开放词表检测。
- 实时性与精度权衡（→ 轻量 backbone、蒸馏、重参数化）。
