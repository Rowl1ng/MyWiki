# 函数家族

---

## 神经元模型

从最简单的说起：线性神经元


$$
 { y }=b +\sum_i { x}_{ i }{ w }_{ i }
$$


McCulloch-Pitts接着提出了二值的，必须达到阈值才发送一定量的冲激函数。

之后的Rectified Linear Neurons\(Linear threshold neuron\)超出阈值的部分仍是线性的。

Simoid neurons可以说是用连续函数版的二值，通常使用Logistic函数，这样一来求导方便。

接下来又有了Stochastic的处理，输出（0~1）作为产生冲激（1）的概率



  ###### Rectifier or rectified linear unit \(ReLU\) or positive part

  ###### Hyperbolic tangent

  ###### Sigmoid

  ###### Softmax

  ###### Radial basis function or RBF

  ###### Softplus

  ###### Hard tanh

  ###### Absolute value rectification

  ###### Maxout

* the structure (also called architecture) of the family of input-output functions can be varied in many ways: 
  _convolutional networks_, 
  _recurrent networks_

### Softmax

Softmax 函数通常被用于将原始分数（raw score）的矢量转换成用于分类的神经网络的输出层上的类概率（class probability）。它通过对归一化常数（normalization constant）进行指数化和相除运算而对分数进行规范化。如果我们正在处理大量的类，例如机器翻译中的大量词汇，计算归一化常数是很昂贵的。有许多种可以让计算更高效的替代选择，包括分层 Softmax（Hierarchical Softmax）或使用基于取样的损失函数，如 NCE。

* designed for the purpose of specifying **multinoulli distributions**:

  $$
  p=softmax(a)\Longleftrightarrow { p }_{ i }=\frac { { e }^{ { a }_{ i } } }{ \sum { _{ j }^{  }{ { e }^{ { a }_{ j } } } }  }
  $$

* consider the gradient with respect to the scores $a$.

  $$
  \begin{aligned}
  \frac { \delta }{ \delta{ a }_{ k } } { L }_{ NLL }(p,y)=\frac { \delta }{ \delta{ a }_{ k } } (-log{ p }_{ y })=\frac { \delta }{ \delta{ a }_{ k } } ({ -a }_{ y }+log\sum _{ j }^{  }{ { e }^{ { a }_{ j } } } )\\ ={ -1 }_{ y=k }+\frac { { e }^{ { a }_{ k } } }{ \sum _{ j }^{  }{ { e }^{ { a }_{ j } } }  } ={ p }_{ k }-{1}_{y=k}
  \end{aligned}
  $$


  or

  $$
  \frac { \delta }{ \delta{ a }_{ k } } { L }_{ NLL }(p,y)=(p-{e}_{y})
  $$


在实现softmax函数的时候，为了防溢出有一个小trick：减去最大值。softmax函数如下：
$$
f(x)_i=\frac{e^{x_i}}{\sum_{j=1}^ne^{x_j}},j=1,2,\ldots,n
$$

通常情况下，计算softmax函数值不会出现什么问题，但是，当某些情况发生时，计算函数值就出问题了：

 - $c$极其大，导致分子计算$e^c$时上溢出
 - $c$ 为负数，且$|c|$很大，此时分母是一个极小的正数，有可能四舍五入为0，导致下溢出

解决方法是，令$M=max(x_i),i=1,2,⋯,n$，即$ M$ 为所有 $x_i$ 中最大的值，那么我们只需要把算 $f(x)_i$的值，改为计算$f(x_i-M)$的值，就可以解决上溢出、下溢出的问题了，并且，计算结果理论上仍然和 $f(x)_i$保持一致。

在实现时也犯了一个低级错误：激活函数一开始弄错成了$1／1-e^{-x}$，发现交叉熵是26上下，忽略了全蒙（weight、bias全零）情况是$\log(250)\approx 5$。



[9]: https://upload.wikimedia.org/wikipedia/commons/thumb/8/88/Logistic-curve.svg/320px-Logistic-curve.svg.png
