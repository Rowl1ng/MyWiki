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

* consider the gradient with respect to the scores $$a$$.

  $$
  \frac { ∂ }{ ∂{ a }_{ k } } { L }_{ NLL }(p,y)=\frac { ∂ }{ ∂{ a }_{ k } } (−log{ p }_{ y })=\frac { ∂ }{ ∂{ a }_{ k } } ({ −a }_{ y }+log\sum _{ j }^{  }{ { e }^{ { a }_{ j } } } )\\ ={ −1 }_{ y=k }+\frac { { e }^{ { a }_{ k } } }{ \sum _{ j }^{  }{ { e }^{ { a }_{ j } } }  } ={ p }_{ k }-{1}_{y=k}
  $$


  or

  $$
  \frac { ∂ }{ ∂{ a }_{ k } } { L }_{ NLL }(p,y)=(p-{e}_{y})
  $$


在实现softmax函数的时候，为了防溢出有一个小trick：减去最大值。softmax函数如下：
$$
f(x)_i=\frac{e^{x_i}}{\sum_{j=1}^ne^{x_j}},j=1,2,\dots,n
$$

通常情况下，计算softmax函数值不会出现什么问题，如下图所示：

![](/assets/softmax.jpg)

但是，当某些情况发生时，计算函数值就出问题了：

 - $$c$$极其大，导致分子计算$$e^c$$时上溢出
 - $$c$$ 为负数，且$$|c|$$很大，此时分母是一个极小的正数，有可能四舍五入为0，导致下溢出

解决方法是，令$$M=max(x_i),i=1,2,⋯,n$$，即$ M$ 为所有 $$x_i$$ 中最大的值，那么我们只需要把算 $f(x)_i$的值，改为计算$$f(x_i−M)$$的值，就可以解决上溢出、下溢出的问题了，并且，计算结果理论上仍然和 $$f(x)_i$$保持一致。

在实现时也犯了一个低级错误：激活函数一开始弄错成了$$1／1-e^{-x}$$，发现交叉熵是26上下，忽略了全蒙（weight、bias全零）情况是$$\log(250)\approx 5$$。

### ReLU

即线性修正单元（Rectified Linear Unit）。ReLU 常在深度神经网络中被用作激活函数。它们的定义是 $$f(x) = \max(0, x)$$ 。ReLU 相对于 tanh 等函数的优势包括它们往往很稀疏（它们的活化可以很容易设置为 0），而且它们受到梯度消失问题的影响也更小。ReLU 主要被用在卷积神经网络中用作激活函数。ReLU 存在几种变体，如Leaky ReLUs、Parametric ReLU (PReLU) 或更为流畅的 softplus近似。

论文：深入研究修正器（Rectifiers）：在 ImageNet 分类上超越人类水平的性能（Delving Deep into Rectifiers: Surpassing Human-Level Performance on ImageNet Classification）  
论文：修正非线性改进神经网络声学模型（Rectifier Nonlinearities Improve Neural Network Acoustic Models ）  
论文：线性修正单元改进受限玻尔兹曼机（Rectified Linear Units Improve Restricted Boltzmann Machines  ）

### 激活函数
用来限制神经元的输入输出振幅。激活函数也称为压制函数，因为它的输出信号压制（限制）到允许范围之内的一定值。
常用激活函数有

- 阈值函数
$$
\varphi(v)=
\begin{cases}
1, &v\geq0 \cr 0, &v<0
\end{cases}
$$
![ScreenShot_20160426153615.png-8.5kB][7]
- 2. sigmoid函数
$$
\varphi(v)=\frac{1}{1+\exp(-av)}
$$
![ScreenShot_20160426153626.png-26.2kB][8]

非线性且处处可微

- Logistic函数
- 双曲正切

- Logistic函数

逻辑函数或逻辑曲线是一种常见的S形函数，它是皮埃尔·弗朗索瓦·韦吕勒在1844或1845年在研究它与人口增长的关系时命名的。广义Logistic曲线可以模仿一些情况人口增长（P）的S形曲线。起初阶段大致是指数增长；然后随着开始变得饱和，增加变慢；最后，达到成熟时增加停止。

一个简单的Logistic函数可用下式表示：$P(t)=\frac 1{1+e^-t}$
![此处输入图片的描述][9]

- 3. tanh函数

$$
\varphi(v)=\tanh(v)
$$

#### Why use？

Activation functions for the hidden units are needed to introduce **nonlinearity** into the network. Without nonlinearity, hidden units would not make nets more powerful than just plain perceptrons (which do not have any hidden units, just input and output units). The reason is that a linear function of linear functions is again a linear function. However, it is the nonlinearity (i.e, the capability to represent nonlinear functions) that makes multilayer networks so powerful. Almost any nonlinear function does the job, except for polynomials. For backpropagation learning, the activation function must be differentiable, and it helps if the function is bounded; the sigmoidal functions such as logistic and tanh and the Gaussian function are the most common choices. Functions such as tanh or arctan that produce both positive and negative values tend to yield faster training than functions that produce only positive values such as logistic, because of better numerical conditioning (see ftp://ftp.sas.com/pub/neural/illcond/illcond.html).
For hidden units, sigmoid activation functions are usually preferable to threshold activation functions. Networks with threshold units are difficult to train because the error function is stepwise constant, hence the gradient either does not exist or is zero, making it impossible to use backprop or more efficient gradient-based training methods. Even for training methods that do not use gradients--such as simulated annealing and genetic algorithms--sigmoid units are easier to train than threshold units. With sigmoid units, a small change in the weights will usually produce a change in the outputs, which makes it possible to tell whether that change in the weights is good or bad. With threshold units, a small change in the weights will often produce no change in the outputs.

For the output units, you should choose an activation function suited to the distribution of the target values:

- For binary (0/1) targets, the logistic function is an excellent choice (Jordan, 1995).
- For categorical targets using 1-of-C coding, the softmax activation function is the logical extension of the logistic function.
- For continuous-valued targets with a bounded range, the logistic and tanh functions can be used, provided you either scale the outputs to the range of the targets or scale the targets to the range of the output activation function ("scaling" means multiplying by and adding appropriate constants).
- If the target values are positive but have no known upper bound, you can use an exponential output activation function, but beware of overflow.
- For continuous-valued targets with no known bounds, use the identity or "linear" activation function (which amounts to no activation function) unless you have a very good reason to do otherwise.

[7]: http://static.zybuluo.com/sixijinling/zcs7oxqe75t3aebzyld5mmu1/ScreenShot_20160426153615.png
[8]: http://static.zybuluo.com/sixijinling/os6tp2kzrwe1yfbgmzi42548/ScreenShot_20160426153626.png
[9]: https://upload.wikimedia.org/wikipedia/commons/thumb/8/88/Logistic-curve.svg/320px-Logistic-curve.svg.png
