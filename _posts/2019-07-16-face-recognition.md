## 人脸识别中的算法研发
应用场景：1. 人证合一、智能门禁、一地三检、智慧工地、明星脸识别

## 算法
- 场景：配合、非配合; 1比1; 1比n; m比n;
- 任务抽象：传统分类属于close set classification，测试时处理的类别都在训练集中出现过; 人脸：open set classification;难以穷尽可能出现的所有人; 方法：学习一种Encoding方式，将人脸映射到固定长度的向量（Embedding）;

### 人脸检测
- 输入：图片 / 视频帧
- 输出：图片中的人脸框坐标

- Viola Jones
- RCNN、Fast-RCNN、Faster-RCNN
- SSD、MTCNN
- Speed up: we don't need as many layers as general object detection. Channel halfing; Prior on face shape
属性：传统做法：直接用最后一层做微调; 复用检测的网络特征;因为网络对特征的学习抽象程度随层数递增，因此将人脸姿态、是否戴眼镜、性别放在网络降低层，将颜值、年龄放在较高层（猫咪：103岁）
- 颜值：爬明星脸; 标注

### 人脸对齐
- Landmark localization
- Similarity Transform: translation + rotation + scale
- Cropping
- 灰度化 + 均衡化:规避夜间（红外）问题;认为彩色信息是不利模型鲁棒性的;
- CLAHE: 受限的局部直方图均衡化;
- Adapter for different detectors: another regression

### 人脸识别
- 输入：对齐过的人脸
- 输出：特征向量（Embedding）
- 评价指标：人脸验证 TAR@FAR=1e-3  闭集人脸识别：Top-K Accuracy 开集：TAIR@FAIR=1e-3

#### Backbone
- VGG
- Inception
- ResNet
- Inception-ResNet
- FaceResNet
- MobileNet

#### Metric Learning
- Contrastive Loss
- Triplet Loss: A well-known but useless algorithm
- Center Loss

#### Softmax Based
- Softmax
- Large Margin Softmax
- Angular Softmax
- SphereFace
- ArcFace

### 上下文的利用
- 学术界进展：主要以端到端为主。1. 用GAN直接根据序列图片生成一张脸; 2. 用Attention机制自动为各帧打分;
- 我们的做法：定义质量分，通过标注数据，给质量分模型以明确的监督; 配合分辨率、人脸pose 进行top-k选择和融合;
- 属性：滑窗众数平滑

### 3D
- 结构光/ToF
- 对人脸姿态建模更加精确
- 对光照、妆容更鲁棒

## 工程
方法论：《Machine Learning Yearning》斯坦福cs231n 找圈子
与研究的区别：不局限于算法模型本身的提升，还可以通过数据增益和系统设计，上线模型相当于直接拟合测试集;需求有时和学术目标不同：比如我们不希望检出小脸模糊脸;

### Tricks
- mirror face & test-time augmentation：无需重训练模型，仅推理时对输入进行翻转或扰动，在很多情况下能够获得小幅提升; 普遍用于ImageNet图像分类刷榜;
- Multi-stage training: (类似于深度学习早期用layer-wise pretraining做模型参数初始化)对于难以收敛的网络，通过交替地仅train模型的部分层的参数，通常可以拟合到更低的损失;
- patch-based ensemble：对人脸的不同部分分别训练模型，最后将每个部分的Embedding融合;
- SSD硬盘: IO bound、多卡数据并行

### 开源复现
- 依赖未发布的版本：给作者写邮件
- 依赖未发布的数据集：没辙
- 跨框架性能损失：尽量不要跨框架
- 依赖未公开的tricks：Analyse
- 开源错了：track issues and discussion

### 数据处理
- 公开数据集验证;
- 自己数据价值验证;
- 半自动价值验证;
- 人工清洗价值验证;
- 数据层次化处理：先快后慢;
- 数据质量及采样设计;
- 弱标签场景数据获取;
- 无标签场景数据获取;
- 标注交互设计及质量管理;
- 快速难样本生成;

### 测试
- AI Hasn’t Found Its Isaac Newton. --- GaryMarcus
- 维度1：数据集维度：拍摄、人种、场景、年龄
- 维度2：流水线维度：检测、对齐、识别适配问题
- 维度3：开源与友商对比
- 协议及中间结果序列化、错误归因、算法、流水线
- 模型提交规范
- 一致化接口:抽象行为：模型加载、预测、指定设备
- 自建基于视频的benchmark
- 错误分析

### 部署
- C++ openmp、TensorRT、distill、docker
- 标注工具、策略及交互设计;
