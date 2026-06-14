# CV 通用常识（跨方向）

> 用途：骨干网络、训练范式、常见可迁移组件和评估共性。多数任务都值得搭配主方向参考一起读。
> ⚠️ 具体数值作为量级参考；涉及精确数值请提示用户核实。

## 常用骨干网络（backbone）
- **CNN**：ResNet（最常用 baseline backbone）、ResNeXt、Res2Net、ConvNeXt（现代化 CNN，可与 ViT 媲美）。
- **Transformer**：ViT、DeiT、Swin Transformer（层级 + 窗口注意力，密集预测常用）、PVT、MViT。
- **轻量**：MobileNet 系、EfficientNet、GhostNet、ShuffleNet（移动端/实时）。
- **混合**：CoAtNet、MaxViT（卷积 + 注意力）。

## 高频可迁移的"模块/机制"（做模块迁移时的素材库）
- 注意力：self/cross-attention、deformable attention、窗口注意力、轴向注意力、线性注意力。
- 多尺度：FPN、ASPP、BiFPN、PPM。
- 记忆机制：STM/XMem 式记忆读写（视频任务跨帧信息传递，常迁移到跟踪）。
- 损失：Focal Loss（类不平衡）、IoU/GIoU/DIoU/CIoU（框回归）、对比损失（表征学习）。
- 训练范式：自监督预训练（MAE、DINO、MoCo、CLIP）、知识蒸馏、半/弱监督、伪标签自训练。
- 提示/基础模型：SAM（零样本 mask）、CLIP（开放词表/文本对齐）。

## 常见训练技巧
- 数据增强：Mosaic、MixUp、CutMix、Copy-Paste、RandAugment。
- 优化：AdamW、cosine 学习率、warmup、EMA。
- 正则：DropPath（stochastic depth）、label smoothing。
- 长尾：重采样、重加权、logit adjustment、解耦表征与分类器。

## 评估与对比的共性原则
- **公平对比**：同 backbone、同输入分辨率、同训练数据/轮次下比较，否则涨点可能来自配置而非方法。
- **报全成本**：精度之外报参数量、FLOPs、FPS/延迟，尤其当卖点是效率。
- **消融充分**：逐组件验证贡献来源，避免"多个改动一起上、不知道谁在起作用"。
- **统计稳健**：跟踪/检测多次实验或多数据集验证，避免单次波动。

## 提创新点时的通用自检
- 这个改动解决的是哪个**真实、具体**的失败模式？（最好能举出失败案例/场景）
- 为什么从机制上它会带来提升，而不只是"更多参数/更多算力"？
- 对比的 baseline 选对了吗？涨点能归因到这个改动吗？
- 实现代价（数据、算力、工程）是否在合理范围？
