# multi-class

cross entropy loss

# Models

## 场景分割

PSPNet

## CRF

主要参考论文：**《Conditional Random Fields as Recurrent Neural Networks》**

- Contribution：这篇论文最重要的工作就是**end-to-end**训练。CNN负责产生unary potential，intuitively上更合理，实验效果也证明有提高。具体通过两方面来实现：
    - mean-field CRF 的RNN实现：将mean-field算法的步骤分解为CNN，这样通过迭代就成了RNN。
    - error differential 的反向传递，CRF的error derivative直接传到了CNN。
- 关键问题：将 label assignment problem 转化为 probability inference problem ，融入对相似像素的**标签一致性**的推断。
- 目标：从 coarse 到 fine-grained segmentation ，弥补 CNN 在 pixel-level labeling task 上的不足。

Github : [CRF-RNN for Semantic Image Segmentation][5]

下面来讲 CRF 用于 pixel-wise 的图像标记（其实就是图像分割）。当把像素的label作为形成马尔科夫场的随机变量，且能够获得全局观测时，CRF便可以对这些label进行建模。

![CRF][11]
> 图中的$$y_i$$为观测，即像素。

定义随机变量$$X_i$$是像素$$i$$的标签。

$$
X_i \in L={l_1,l_2,...,l_L}
$$

- $$X$$：由$$X_1,X_2,...,X_N$$组成的随机向量；
- $$N$$就是图像的像素个数。

假设图$$G=(V,E)$$，其中$$V={X_1,X_2,\dots,X_N}$$，全局观测为（image）$$I$$。$$(I,X)$$即基于Gibbs分布的CRF模型：

$$
P(X=x|I)=\frac 1{Z(I)}exp(-E(x|I))
$$

$$Z(I)$$是所有可能labeling的集合，保证$$P(X=x|I)$$在0~1。

条件随机场图像分割能量函数的构造：

$$
E(x)=\sum _i \varphi_u(x_i)+\sum _{i<j} \varphi_p(x_i.x_j)
$$

这里的$$E$$可以理解为能量，也就是cost。其中$$\varphi_u(x_i)$$是将像素$$i$$标记为$$x_i$$的inverse likelihood，$$\varphi_p(x_i.x_j)$$是将$$i$$、$$j$$同时标记为$$x_i$$、$$x_j$$的能量。


## DeepLabv3

  [5]: https://github.com/torrvision/crfasrnn