# Theano


Theano 是一个让你可以定义、优化和评估数学表达式的 Python 库。它包含许多用于深度神经网络的构造模块。Theano 是类似于 TensorFlow 的低级别库。更高级别的库包括Keras 和 Caffe。

- 自动求导
- 加速矩阵运算
- 声明时不赋值，编译时可以无值，使用时有值。

 解决了theano failed to compile cuda_ndarray.cu的问题：在/etc/ld.so.conf.d/libc.conf 里加上cuda路径/usr/local/cuda/lib64/，再sudo ldconfig

## Reference Books 

- Theano Documentation Release 0.7
- Deep Learning Tutorial Release 0.1

### 机器学习中矩阵约定

行是水平的列是垂直的。每一行是一个实例。因此，输入[10,5]是一个10个实例，每个实例的维数是5的一个矩阵。如果这个成为一个神经网络的输入，那么从输入到第一隐藏层的权重将表示一个矩阵的大小（5,#hid）。

  ```
  >>> numpy.asarray([[1.,2],[3,4],[5,6]])

  array([[1.,
  2.],


  [3.,
  4.],


  [5.,
  6.]])

  >>> numpy.asarray([[1.,
  2],[3,4],[5,6]]).shape

  (3,2)
  ```

在Theano中，所有的符号必须是有**类型**的。特别地，`T.dscalar`是我们分配给双精度（doubles）的"0-维"数组（标量）的类型。它是一个Theano类型。