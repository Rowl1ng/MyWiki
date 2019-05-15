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

## DeepLabv3

  [5]: https://github.com/torrvision/crfasrnn