## 人脸识别中的算法研发

## 算法
- 场景：配合、非配合; 1比1; 1比n; m比n;
- 任务抽象：open set classification;
- 与word embedding对比：

### 人脸检测
- 输入：图片 / 视频帧
- 输出：图片中的人脸框坐标

- Viola Jones
- RCNN、Fast-RCNN、Faster-RCNN
- SSD、MTCNN

### 人脸识别
- 输入：矫正过的人脸
- 输出：特征向量（Embedding）
- 评价指标：TAR@FAR=1e-3
#### Backbone
- VGG、Inception、ResNet、Inception-ResNet
- FaceResNet
- MobileNet

#### Metric Learning
- Contrastive Loss

- Triplet Loss: A well-known but useless algorithm


#### Softmax Based
- Large Margin Softmax
- 


## 工程
### 开源复现
- 依赖未发布的版本
- 依赖未发布的数据集
- 跨框架性能损失
- 依赖未公开的tricks
- 开源错了


### 数据处理


- 公开数据集验证
- 自己数据价值验证;
- 半自动价值验证;
- 人工清洗价值验证;
- 数据质量及采样设计;


### 测试
- AI Hasn’t Found Its Isaac Newton. --- GaryMarcus
- 维度1：数据集维度：拍摄、人种、场景、年龄
- 维度2：流水线维度：检测、对齐、识别不适配问题
- 模型提交规范


### 部署与迭代
- C++ openmp、TensorRT、distill
- 标注工具、策略及交互设计;
- 快速难样本生成;
- 
