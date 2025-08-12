# Optimization Procedure

A good choice for the criterion is maximum likelihood regularized with dropout, possibly also with weight decay.

A good choice for the optimization algorithm for a feed-forward network is usually stochastic gradient descent with momentum.

# Loss Function and Conditional Log-Likelihood

In the 80’s and 90’s the most commonly used loss function was the squared error

  $$
  L({ f }_{ \theta }(x),y)={ ||f\theta(x)-y|| }^{ 2 }
  $$

if f is unrestricted (non-parametric),

  $$
  f(x) = E[y | x = x]
  $$

Replacing the squared error by an absolute value makes the neural network try to estimate not the conditional expectation but the conditional median

分类交叉熵损失（Categorical Cross-Entropy Loss）。

**交叉熵（cross entropy）目标函数 **: 
when y is a **discrete** label, i.e., for classification problems, other loss functions such as the **Bernoulli negative log-likelihood** have been found to be more appropriate than the squared error. ($y\in{ { 0,1 }  }$)


$$
L({ f }_{ \theta }(x),y)=-ylog{ f }_{ \theta }(x)-(1-y)log(1-{ f }_{ \theta }(x))
$$


${f}_{\theta}(x)$ to be strictly between 0 to 1: use the sigmoid as non-linearity for the output layer\(matches well with the binomial negative log-likelihood cost function\)


The mean is halved（$\frac 12$）as a convenience for the computation of the gradient descent, as the derivative term of the square function will cancel out the（$\frac 12$\) term.

# Learning a Conditional Probability Model

负对数似然（NLL：Negative Log Likelihood）

loss function as corresponding to a conditional log-likelihood, i.e., the negative log-likelihood \(NLL\) cost function

$$
{L}_{NLL}({f}_{\theta}(x),y)=-logP(y=y|x=x;\theta)
$$

Example: if y is a continuous random variable and we assume that, given $$x$$, it has a Gaussian distribution with mean ${f}_{\theta}(x)$ and variance ${\sigma}^{2}$

$$
  -logP(y|x;\theta)=\frac { 1 }{ 2 } { ({ f }_{ \theta  }(x)-y) }^{ 1 }/{ \sigma }^{ 2 }+log(2\pi{\sigma}^{ 2 })
$$

Minimizing this negative log-likelihood is therefore equivalent to minimizing the squared error loss.

For discrete variables, the binomial negative log-likelihood cost function corresponds to the conditional log-likelihood associated with the Bernoulli distribution \(also known as cross entropy\) with probability $p = {f}_{\theta}(x)$ of generating $y = 1$ given$x =x$


  $$
  \begin{aligned}
  {L}_{NLL}=-logP(y|x;\theta)={-1}_{y=1}{logp-1}_{y=0}log(1-p)\\ =-ylog{f}_{\theta}(x)-(1-y)log(1-{f}_{\theta}(x))
  \end{aligned}
  $$
  
## 分类交叉熵损失（Categorical Cross-Entropy Loss）

分类交叉熵损失也被称为负对数似然（negative log likelihood）。这是一种用于解决分类问题的流行的损失函数，可用于测量两种概率分布（通常是真实标签和预测标签）之间的相似性。它可用 $L = -\sum(y * \log(y_{prediction}))$ 表示，其中 y 是真实标签的概率分布（通常是一个one-hot vector），$y_{prediction} $是预测标签的概率分布，通常来自于一个 softmax。



## Tukeys Loss


[Robust Optimization for Deep Regression](https://arxiv.org/pdf/1505.06606.pdf)

[TukeysBiweight](http://mathworld.wolfram.com/TukeysBiweight.html)



## Dice Loss

常用于图像分割任务
[Pytorch实现](https://github.com/milesial/Pytorch-UNet/blob/master/dice_loss.py)

# Perceptual Loss

感知损失：可以将卷积神经网络提取出的feature，作为目标函数的一部分，通过比较待生成的图片经过CNN的feature值与目标图片经过CNN的feature值，使得待生成的图片与目标图片在语义上更加相似(相对于Pixel级别的损失函数)。

# Focal loss

看ICCV那篇focal loss的论文[《Focal Loss for Dense Object Detection》](http://openaccess.thecvf.com/content_ICCV_2017/papers/Lin_Focal_Loss_for_ICCV_2017_paper.pdf).

不过这个pytorch版detectron还没实现，官方Detectron是集成在Caffe2里。可参考[Pytorch实现](https://github.com/marvis/pytorch-yolo2/blob/master/FocalLoss.py)。

$$
Loss(x, class) = - \alpha (1-softmax(x)_{[class]})^\gamma \log(softmax(x)_{[class]})
$$

```python
def focal_loss(inputs, targets):
    gamma = 2
    N = inputs.size(0)
    C = inputs.size(1)
    P = F.softmax(inputs) # softmax(x)

    class_mask = inputs.data.new(N, C).fill_(0)
    class_mask = Variable(class_mask)
    ids = targets.view(-1, 1)
    class_mask.scatter_(1, ids, 1.)
    # print(class_mask)

    probs = (P * class_mask).sum(1).view(-1, 1)# softmax(x)_class

    log_p = probs.log()
    # print('probs size= {}'.format(probs.size()))
    # print(probs)

    batch_loss = -(torch.pow((1 - probs), gamma)) * log_p
    # print('-----bacth_loss------')
    # print(batch_loss)

    loss = batch_loss.mean()

    return loss
```

- $$\alpha$$(1D Tensor, Variable) : the scalar factor for this criterion
- $$\gamma$$(float, double) : $$\gamma > 0$$; reduces the relative loss for well-classiﬁed examples (p > .5), putting more focus on hard, misclassiﬁed examples
- size_average(bool): By default, the losses are averaged over observations for each minibatch. However, if the field size_average is set to False, the losses are instead summed for each minibatch.
                                

## Huber Loss

$$
\begin{aligned}
\text{loss}(x, y) = \frac{1}{n} \sum_{i} z_{i} \\

z_{i} =
\begin{cases}
0.5 (x_i - y_i)^2, & \text{if } |x_i - y_i| < 1 \\
|x_i - y_i| - 0.5, & \text{otherwise }
\end{cases}
\end{aligned}
$$

[機器/深度學習: 損失函數(loss function)- Huber Loss和 Focal loss](https://medium.com/@chih.sheng.huang821/%E6%A9%9F%E5%99%A8-%E6%B7%B1%E5%BA%A6%E5%AD%B8%E7%BF%92-%E6%90%8D%E5%A4%B1%E5%87%BD%E6%95%B8-loss-function-huber-loss%E5%92%8C-focal-loss-bb757494f85e)


# Inbox

  * Efficient Optimization for Rank-based Loss
  * FID：本质上是利用训好的特征提取器的最后一层输出embedding的分布来计算相似度，2-Wassertein distance
  * Wassertein distance/EMD：distance metric defined between probability distributions. 基于微积分的“运土”代价计算，起源于optimal transport planning of good and materials（也许是苏联计划经济时期？）