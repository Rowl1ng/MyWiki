# 正则化/结构风险最小化

降低过拟合风险

## 范数

对$$w$$,正则化参数$$\lambda>0$$，正则项通常用于对模型的训练施加某种约束

* $$L_0$$范数：非零分量的个数
  * 不连续，难以优化求解，因此常使用$$L_1$$范数来近似。
* $$L_1$$范数：绝对值，亦称**LASSO**（最小绝对收缩选择算子）
  * 使被约束矩阵/向量更稀疏。 比$$L_2$$范数更易获得稀疏解，即它求得的$$w$$会有更少的非零分量
* $$L_2$$范数：相当于**模**，亦称**Tikhonov正则化**
  * “岭回归”（ridge regression）
  * 使被约束的矩阵/向量更平滑，因为它对脉冲型的值有很大的惩罚 

## Dropout

Dropout 是一种用于神经网络防止过拟合的正则化技术。它通过在每次训练迭代中随机地设置神经元中的一小部分为 0 来阻止神经元共适应（co-adapting），Dropout 可以通过多种方式进行解读，比如从不同网络的指数数字中随机取样。Dropout 层首先通过它们在卷积神经网络中的应用而得到普及，但自那以后也被应用到了其它层上，包括输入嵌入或循环网络。

论文：Dropout: 一种防止神经网络过拟合的简单方法（Dropout: A Simple Way to Prevent Neural Networks from Overfitting）  
论文：循环神经网络正则化（Recurrent Neural Network Regularization）

梯度爆炸问题（Exploding Gradient Problem）

梯度爆炸问题是梯度消失问题（Vanishing Gradient Problem）的对立面。在深度神经网络中，梯度可能会在反向传播过程中爆炸，导致数字溢出。解决梯度爆炸的一个常见技术是执行梯度裁剪（Gradient Clipping）。

论文：训练循环神经网络的困难之处（On the difficulty of training Recurrent Neural Networks）



梯度裁剪（Gradient Clipping）

梯度裁剪是一种在非常深度的网络（通常是循环神经网络）中用于防止梯度爆炸（exploding gradient）的技术。执行梯度裁剪的方法有很多，但常见的一种是当参数矢量的 L2 范数（L2 norm）超过一个特定阈值时对参数矢量的梯度进行标准化，这个特定阈值根据函数：新梯度=梯度_阈值/L2范数（梯度）{new\_gradients = gradients _ threshold / l2\_norm\(gradients\)}确定。

论文：训练循环神经网络的困难之处（On the difficulty of training Recurrent Neural Networks）

