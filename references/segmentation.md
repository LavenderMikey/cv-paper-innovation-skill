# 图像分割（Segmentation）领域常识

> 用途：判断现有 SOTA 大致水平、新提案该与谁对比、用哪些数据集和指标。
> ⚠️ 具体数值随版本/训练设置波动，作为量级参考；涉及精确数值请提示用户核实。

## 任务划分
- **语义分割**：逐像素分类，不区分实例。
- **实例分割**：区分同类不同个体（含框+mask）。
- **全景分割**：语义（stuff）+ 实例（things）统一。
- **视频分割 / VOS**：跨帧分割与传播（记忆机制是高频可迁移模块）。
- **开放词表 / 可提示分割**：文本或点/框提示驱动。

## 经典 baseline / 里程碑方法
**语义分割**
- FCN：全卷积开山之作。
- U-Net：编码-解码 + skip connection，医学图像与小数据集常用 baseline，skip 结构高频被迁移。
- DeepLab 系（v3/v3+）：空洞卷积 + ASPP（多尺度上下文，常被迁移的组件）。
- PSPNet：金字塔池化。
- SegFormer、Segmenter、SETR：Transformer 语义分割。

**实例/全景分割**
- Mask R-CNN：Faster R-CNN + mask 分支，实例分割经典 baseline。
- Mask2Former、MaskFormer：统一 mask 分类范式，跨语义/实例/全景通用，当前强 baseline。

**通用/基础模型**
- SAM / SAM 2：可提示分割基础模型（SAM 2 扩展到视频），常作为零样本 mask 来源被迁移。

**视频对象分割**
- STM / XMem：记忆网络读写历史帧（记忆机制是经常被迁移到跟踪等任务的模块）。

## 常用数据集
- **Cityscapes**：街景语义分割，19 类，报 mIoU。
- **ADE20K**：场景解析，150 类，mIoU。
- **PASCAL VOC / Context**：语义分割。
- **COCO**：实例/全景分割（AP、PQ）。
- 视频：DAVIS、YouTube-VOS（J&F）。
- 医学：常用 Dice 系数。

## 评价指标
- **mIoU**：语义分割核心。
- **AP（mask）**：实例分割。
- **PQ（Panoptic Quality）**：全景分割。
- **Dice / J&F**：医学 / 视频分割。
- 边界质量：Boundary IoU。

## 常见可挖掘的痛点
- 边界/细节模糊（→ 边界监督、高分辨率特征、边界 IoU 优化）。
- 小目标/细长结构掉点。
- 标注昂贵（→ 半/弱/无监督、点监督、用 SAM 生成伪标签）。
- 类别不平衡与长尾。
- 视频时序一致性、记忆效率（→ 稀疏/压缩记忆）。
- 域适应、实时分割（移动端）。
