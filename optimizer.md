# 优化算法

深度学习的优化算法，说白了就是梯度下降。每次的参数更新有两种方式：

1.遍历全部数据集算一次损失函数，然后算函数对各个参数的梯度，更新梯度。这种方法每更新一次参数都要把数据集里的所有样本都看一遍，计算量开销大，计算速度慢，不支持在线学习，这称为Batch gradient descent，批梯度下降。The gradients are accumulated over the entire traing set.

2.每看一个数据就算一下损失函数，然后求梯度更新参数，这个称为随机梯度下降，stochastic gradient descent。这个方法速度比较快，但是收敛性能不太好，可能在最优点附近晃来晃去，hit不到最优点。两次参数的更新也有可能互相抵消掉，造成目标函数震荡的比较剧烈。

为了克服两种方法的缺点，现在一般采用的是一种折中手段，**mini-batch gradient decent**，小批的梯度下降，这种方法把数据分为若干个批，按批来更新参数，这样，**一个批中的一组数据共同决定了本次梯度的方向**，下降起来就不容易跑偏，减少了随机性。另一方面因为批的样本数与整个数据集相比小了很多，计算量也不是很大。

## 随机梯度下降（SGD：Stochastic Gradient Descent）

随机梯度下降是一种被用在训练阶段学习网络参数的基于梯度的优化算法。梯度通常使用反向传播算法计算。在实际应用中，人们使用微小批量版本的 SGD，其中的参数更新基于批案例而非单个案例进行执行，这能增加计算效率。vanilla SGD 存在许多扩展，包括动量（Momentum）、Adagrad、rmsprop、Adadelta 或 Adam。

论文：用于在线学习和随机优化的自适应次梯度方法（Adaptive Subgradient Methods for Online Learning and Stochastic Optimization）  
技术博客：斯坦福CS231n：优化算法（[http://cs231n.github.io/neural-networks-3/）](http://cs231n.github.io/neural-networks-3/）)  
技术博客：梯度下降优化算法概述（[http://sebastianruder.com/optimizing-gradient-descent/）](http://sebastianruder.com/optimizing-gradient-descent/）)

此处主要说下SGD的缺点：（正因为有这些缺点才让这么多大神发展出了后续的各种算法）

- 选择合适的learning rate比较困难
- 对所有的参数更新使用同样的learningrate。对于稀疏数据或者特征，有时我们可能想更新快一些对于不经常出现的特征，对于常出现的特征更新慢一些，这时候SGD就不太能满足要求了
- SGD容易收敛到局部最优，并且容易被困在鞍点



损失平面等高线：

![损失平面等高线][19]

在鞍点处的比较：

![在鞍点处的比较][20]

在Deep Learning中，往往loss function是非凸的，没有解析解，我们需要通过优化方法来求解。solver的主要作用就是交替调用前向（forward)算法和后向（backward)算法来更新参数，从而最小化loss，实际上就是一种迭代的优化算法。

### 动量（Momentum）

动量是梯度下降算法（Gradient Descent Algorithm）的扩展，可以加速和阻抑参数更新。在实际应用中，在梯度下降更新中包含一个动量项可在深度网络中得到更好的收敛速度（convergence rate）。

* It damps oscillation in directions of high curvature by combining gradients with opposite signs.
* It builds up speed in directions with a gentle but consistent gradient.

$$\mathrm v(t)=\alpha \mathrm v(t-1)-\varepsilon \frac {\partial E}{\partial \mathrm w}(t)$$

* increment the previous velocity
* decays by momentum $$\alpha(\alpha<1)$$ 

$$\Delta \mathrm w(t)=\mathrm v(t)=\alpha \Delta \mathrm w(t-1)-\varepsilon \frac {\partial E}{\partial \mathrm w}(t)$$

terminal velocity:$$\frac 1{1-\alpha}(-\varepsilon \frac{\partial E}{\partial \mathrm w})$$

$$\alpha=0.9$$ correspond to multiplying the maximum speed by 10 relative to the gradient descent algorithm. 

It is less important to adapt $$\alpha$$ over time than to shrink $$\varepsilon$$ over time.

#### Nesterov Momentum

First make a jump

Then measure the gradient, make a correction.

"It turns out ,if you're going to gamble, it's much better to gamble and then make a correction, than to make a correction and then gamble.

论文：通过反向传播（back-propagating error）错误学习表征

### Adadelta

 Adadelta 是一个基于梯度下降的学习算法，可以随时间调整适应每个参数的学习率。它是作为 Adagrad 的改进版提出的，它比超参数（hyperparameter）更敏感而且可能会太过严重地降低学习率。Adadelta 类似于 rmsprop，而且可被用来替代 vanilla SGD。

To determine the individual learning rates:

* Start with a local gain of 1 for every weight.
* $$+\delta$$ : Increase the local gain if the gradient for that weight does not change sign.
* $$\times(1-\delta)$$ : Use small additive increases and multiplicative decreases.

Tricks:

* Limit the gains to lie in some reasonable range.
* Use full batch learning or very big mini-batches.
* Adaptive learning rates can be combined with momentum.
* Adaptive learning rates only deal with axis-aligned effects.

论文：Adadelta：一种自适应学习率方法（ADADELTA: An Adaptive Learning Rate Method）  
技术博客：斯坦福 CS231n：优化算法（[http://cs231n.github.io/neural-networks-3/）](http://cs231n.github.io/neural-networks-3/）)  
技术博客：梯度下降优化算法概述（[http://sebastianruder.com/optimizing-gradient-descent/）](http://sebastianruder.com/optimizing-gradient-descent/）)

### Adagrad

Adagrad 是一种自适应学习率算法，能够随时间跟踪平方梯度并自动适应每个参数的学习率。它可被用来替代vanilla SGD \([http://www.wildml.com/deep-learning-glossary/\#sgd\)；而且在稀疏数据上更是特别有用，在其中它可以将更高的学习率分配给更新不频繁的参数。](http://www.wildml.com/deep-learning-glossary/#sgd%29；而且在稀疏数据上更是特别有用，在其中它可以将更高的学习率分配给更新不频繁的参数。)

论文：用于在线学习和随机优化的自适应次梯度方法（Adaptive Subgradient Methods for Online Learning and Stochastic Optimization）  
技术博客：斯坦福 CS231n：优化算法（[http://cs231n.github.io/neural-networks-3/）](http://cs231n.github.io/neural-networks-3/）)  
技术博客：梯度下降优化算法概述（[http://sebastianruder.com/optimizing-gradient-descent/）](http://sebastianruder.com/optimizing-gradient-descent/）)

### Adam

Adam 是一种类似于 rmsprop 的自适应学习率算法，但它的更新是通过使用梯度的第一和第二时刻的运行平均值（running average）直接估计的，而且还包括一个偏差校正项。

论文：Adam：一种随机优化方法（Adam: A Method for Stochastic Optimization）  
技术博客：梯度下降优化算法概述（[http://sebastianruder.com/optimizing-gradient-descent/）](http://sebastianruder.com/optimizing-gradient-descent/）)

### RMSProp

RMSProp 是一种基于梯度的优化算法。它与 Adagrad 类似，但引入了一个额外的衰减项抵消 Adagrad 在学习率上的快速下降。

* The magnitude of the gradient can be very different for different weights and can change during learning.
  * This makes it hard to choose a single global learning rate.
* For **full batch learning**, we can deal with this variation by only using the sign of the gradient.

rprop: This combines the idea of only using the sign of the gradient with the idea of adapting the step size separately for each weight.

RMSProp: A mini-batch version of rprop

* Keep a moving average of the squared gradient for each weight

$$MeanSquare(w,t)=0.9 MeanSquare(w,t-1)+0.1(\frac {\partial E}{\partial w}(t))^2$$

* Dividing the gradient by $$\sqrt{MeanSquare(w,t)}$$ makes the learning work much better.



PPT：用于机器学习的神经网络 讲座6a  
技术博客：斯坦福CS231n：优化算法（[http://cs231n.github.io/neural-networks-3/）](http://cs231n.github.io/neural-networks-3/）)  
技术博客：梯度下降优化算法概述（[http://sebastianruder.com/optimizing-gradient-descent/）](http://sebastianruder.com/optimizing-gradient-descent/）)

## 问题

### 梯度消失问题（Vanishing Gradient Problem）

梯度消失问题出现在使用梯度很小（在 0 到 1 的范围内）的激活函数的非常深的神经网络中，通常是循环神经网络。因为这些小梯度会在反向传播中相乘，它们往往在这些层中传播时「消失」，从而让网络无法学习长程依赖。解决这一问题的常用方法是使用 ReLU 这样的不受小梯度影响的激活函数，或使用明确针对消失梯度问题的架构，如LSTM。这个问题的反面被称为梯度爆炸问题（exploding gradient problem）。

论文：训练循环神经网络的困难之处（On the difficulty of training Recurrent Neural Networks）

### 难例挖掘

大致思路是只选较难的case进行loss计算，较难的定义是：容易错分成另一类的case，具体表现就是loss大，所以体现在代码中就是选loss最大的topk来反传梯度。

最终效果是减少FP（如果对negative sample进行难例挖掘的话）。这里是pytorch实现：[Online Hard Example Mining on PyTorch](http://www.erogol.com/online-hard-example-mining-pytorch/)

```python
import torch as th                                                                 
                                                                                                                                                                      
class NLL_OHEM(th.nn.NLLLoss):                                                     
    """ Online hard example mining. 
    Needs input from nn.LogSotmax() """                                             
                                                                                   
    def __init__(self, ratio):      
        super(NLL_OHEM, self).__init__(None, True)                                 
        self.ratio = ratio                                                         
                                                                                   
    def forward(self, x, y, ratio=None):                                           
        if ratio is not None:                                                      
            self.ratio = ratio                                                     
        num_inst = x.size(0)                                                       
        num_hns = int(self.ratio * num_inst)                                       
        x_ = x.clone()                                                             
        inst_losses = th.autograd.Variable(th.zeros(num_inst)).cuda()              
        for idx, label in enumerate(y.data):                                       
            inst_losses[idx] = -x_.data[idx, label]                                 
        #loss_incs = -x_.sum(1)                                                    
        _, idxs = inst_losses.topk(num_hns)                                        
        x_hn = x.index_select(0, idxs)                                             
        y_hn = y.index_select(0, idxs)                                             
        return th.nn.functional.nll_loss(x_hn, y_hn)  
```

  [19]: http://img.blog.csdn.net/20160824161755284
  [20]: http://img.blog.csdn.net/20160824161815758