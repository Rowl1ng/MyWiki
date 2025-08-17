# 卷积神经网络（CNN/ConvNet：Convolutional Neural Network）

  * 卷积网络和傅立叶变换之间的联系
  
CNN 使用卷积连接从输入的局部区域中提取的特征。大部分 CNN 都包含了卷积层、池化层和仿射层的组合。CNN 尤其凭借其在视觉识别任务的卓越性能表现而获得了普及，它已经在该领域保持了好几年的领先。

推荐[可视化卷积过程：stride+padding+dilation](https://github.com/vdumoulin/conv_arithmetic)

技术博客：斯坦福CS231n类——用于视觉识别的卷积神经网络（http://cs231n.github.io/neural-networks-3/）
技术博客：理解用于自然语言处理的卷积神经网络（http://www.wildml.com/2015/11/understanding-convolutional-neural-networks-for-nlp/）

# 卷积神经网络(CNN)

## 概念

卷积神经网络是一种前馈神经网络，它的人工神经元可以响应一部分覆盖范围内的周围单元，对于大型图像处理有出色表现。
它有5个特点：
1.局部感知；
2.参数共享；
3.采样；
4.多卷积核；
5.多卷积层。
其中前3个特征是CNN较少参数数目的利器。

## 网络结构

CNN由输入层、**卷积层**和**抽样层**的交替组合、**全连接层**、输出层组成。
其中：

- 输入层一般是二维的，如64*64的图像，输入量可以看做是一个矩阵，其大小等于图像的像素个数。
- 卷积层是依次将矩阵的区域与卷积核作卷积运算，从而产生一个新的矩阵；而矩阵的区域是从左上角开始依次向右或向下移动一维产生的，直到碰到右端或下方边界；而卷积核可以有多个，因此原矩阵经过卷积层的处理后会产生**多个新矩阵**。
- 采样层是将原矩阵的区域抽象成一个数字表示，从而极大减少特征的维数；而矩阵的区域的选择与卷积层相同；矩阵区域抽象成数字的方法：取**最大值，或最小值，或平均值**等；抽象层的处理不会引起矩阵个数的变化，而只是改变矩阵的**规模**。
- 全连接层是指将卷积层和抽样层的交替组合产生的矩阵映射成**一维**的向量，然后采用**BP神经网络**的结构和处理方式。输出层神经元的个数等于分类的个数，用于表示分类的输出值。
         
![此处输入图片的描述][13]

##学习过程

前向传播计算
卷积层：依次将矩阵的区域与卷积核作卷积运算，从而产生一个新的矩阵
![此处输入图片的描述][14]
![此处输入图片的描述][15]
采样层：将原矩阵的区域抽象成一个数字表示，从而极大减少特征的维数
![此处输入图片的描述][16]
![此处输入图片的描述][17]


## Methods
---

### Pooling 池化


![pooling]( http://static.zybuluo.com/sixijinling/oo489jzvkk9odwbk4vz9djr1/convolution.png)

#### 最大池化（Max-Pooling）

池化（Pooling）操作通常被用在卷积神经网络中。一个最大池化层从一块特征中选取最大值。和卷积层一样，池化层也是通过窗口（块）大小和步幅尺寸进行参数化。比如，我们可能在一个 10×10 特征矩阵上以 2 的步幅滑动一个 2×2 的窗口，然后选取每个窗口的 4 个值中的最大值，得到一个 5×5 特征矩阵。池化层通过只保留最突出的信息来减少表征的维度；在这个图像输入的例子中，它们为转译提供了基本的不变性（即使图像偏移了几个像素，仍可选出同样的最大值）。池化层通常被安插在连续卷积层之间。

#### 平均池化（Average-Pooling）

平均池化是一种在卷积神经网络中用于图像识别的池化（Pooling）技术。它的工作原理是在特征的局部区域上滑动窗口，比如像素，然后再取窗口中所有值的平均。它将输入表征压缩成一种更低维度的表征。

### Inception模块（Inception Module）

Inception模块被用在卷积神经网络中，通过堆叠 1×1 卷积的降维（dimensionality reduction）带来更高效的计算和更深度的网络。

论文：使用卷积获得更深（Going Deeper with Convolutions）

## Existed Work
---

### Unet

常用于图像分割任务

[Pytorch 实现](https://github.com/milesial/Pytorch-UNet)

### Alexnet

Alexnet 是一种卷积神经网络架构的名字，这种架构曾在 2012 年 ILSVRC 挑战赛中以巨大优势获胜，而且它还导致了人们对用于图像识别的卷积神经网络（CNN）的兴趣的复苏。它由 5 个卷积层组成。其中一些后面跟随着最大池化（max-pooling）层和带有最终 1000 条路径的 softmax (1000-way softmax)的 3个全连接层。Alexnet 被引入到了使用深度卷积神经网络的 ImageNet 分类中。

### Google LeNet

GoogleLeNet 是曾赢得了 2014 年 ILSVRC 挑战赛的一种卷积神经网络架构。这种网络使用 Inception 模块（Inception Module）以减少参数和提高网络中计算资源的利用率。

论文：使用卷积获得更深（Going Deeper with Convolutions）

### VGG

VGG 是在 2014 年 ImageNet 定位和分类比赛中分别斩获第一和第二位置的卷积神经网络模型。这个 VGG 模型包含 16-19 个权重层，并使用了大小为 3×3 和 1×1 的小型卷积过滤器。

论文：用于大规模图像识别的非常深度的卷积网络（Very Deep Convolutional Networks for Large-Scale Image Recognition）

### Deep Dream

这是谷歌发明的一种试图用来提炼深度卷积神经网络获取的知识的技术。这种技术可以生成新的图像或转换已有的图片从而给它们一种幻梦般的感觉，尤其是递归地应用时。

代码：Github 上的 Deep Dream（https://github.com/google/deepdream）
技术博客：Inceptionism：向神经网络掘进更深（https://research.googleblog.com/2015/06/inceptionism-going-deeper-into-neural.html）


