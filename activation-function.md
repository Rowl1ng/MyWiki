# 激活函数（Activation Function）

为了让神经网络能够学习复杂的决策边界（decision boundary），我们在其一些层应用一个非线性激活函数。最常用的函数包括  sigmoid、tanh、ReLU（Rectified Linear Unit 线性修正单元） 以及这些函数的变体。

* **motivation**: to compose _simple transformations_ in order to obtain 
  _highly non-linear_ ones
* (MLPs compose affine transformations and element-wise non-linearities)
* hyperbolic tangent activation functions:

  $$
  { h }^{ k }=tanh({ b }^{ k }+{ W }^{ k }{ h }^{ k-1 })
  $$
  ![](https://upload.wikimedia.org/wikipedia/commons/thumb/7/76/Sinh_cosh_tanh.svg/256px-Sinh_cosh_tanh.svg.png)

* the input of the neural net: $${ h }^{ 0 }=x$$
* theoutputofthe k-th hidden layer: $${ h }^{ k }$$

* affine transformation $$a = b+Wx$$, elementwise


  $$
  h=\phi (a)⇔{ h }_{ i }=\phi ({ a }_{ i })=\phi ({ b }_{ i }+{ W }_{ i,: }x)
  $$

* non-linear neural network activation functions:

