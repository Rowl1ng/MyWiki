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

### Iterable Mean-field = RNN

- $Q_{in}$：an estimation marginal probability from the previous iteration:

$$
\begin{aligned}
f_{\theta}(U,Q_{in},I) \\
\theta=\{\omega^{(m)},\mu(l,l')\},\\
m\in \{1,\ldots,M\},l,l' \in \{l_1,\ldots,l_L\}
\end{aligned}
$$

迭代结构等同于RNN，假设迭代T次：

- t=0,使用softmax（U）来初始化
- t>1,每轮$f_{\theta}$中的$Q_{in}$为上一轮输出
- 输出$Y$
- 学习算法：BP through time
    - 5次迭代即可收敛，再增加可能导致vanishing和exploding gradient problems。
: 实现Bilateral filter 的bilateral_lattices_和实现Spatial filter的spatial_lattice

### 6. Related Work

- 《*Efficient Inference in Fully Connected CRFs with Gaussian Edge Potentials*》

## DeepLabv3

  [5]: https://github.com/torrvision/crfasrnn