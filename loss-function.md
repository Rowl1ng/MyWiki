# Cost Functions

* a good choice for the criterion is maximum likelihood regularized with dropout, possibly also with weight decay.

## 分类交叉熵损失（Categorical Cross-Entropy Loss）

分类交叉熵损失也被称为负对数似然（negative log likelihood）。这是一种用于解决分类问题的流行的损失函数，可用于测量两种概率分布（通常是真实标签和预测标签）之间的相似性。它可用 $$L = -\sum(y * \log(y_{prediction}))$$ 表示，其中 y 是真实标签的概率分布（通常是一个one-hot vector），$$y_{prediction} $$是预测标签的概率分布，通常来自于一个 softmax。

## 负对数似然（NLL：Negative Log Likelihood）

参见分类交叉熵损失（Categorical Cross-Entropy Loss）。

# Optimization Procedure

* a good choice for the optimization algorithm for a feed-forward network is usually stochastic gradient descent with momentum.

# Loss Function and Conditional Log-Likelihood

* In the 80’s and 90’s the most commonly used loss function was the squared error

  $$
  L({ f }_{ θ }(x),y)={ ||fθ(x)−y|| }^{ 2 }
  $$

* if f is unrestricted \(non- parametric\),


  $$
  f(x) = E[y | x = x]
  $$

* Replacing the squared error by an absolute value makes the neural network try to estimate not the conditional expectation but the conditional median

* **交叉熵（cross entropy）目标函数 **: when y is a discrete label, i.e., for classification problems, other loss functions such as the Bernoulli negative log-likelihood4 have been found to be more appropriate than the squared error. \($$y∈{ \left\{ 0,1 \right\}  }$$\)


$$
L({ f }_{ θ }(x),y)=−ylog{ f }_{ θ }(x)−(1−y)log(1−{ f }_{ θ }(x))
$$


* $${f}_{\theta}(x)$$ to be strictly between 0 to 1: use the sigmoid as non-linearity for the output layer\(matches well with the binomial negative log-likelihood cost function\)

##### Learning a Conditional Probability Model

* loss function as corresponding to a conditional log-likelihood, i.e., the negative log-likelihood \(NLL\) cost function

  $$
  { L }_{ NLL }({ f }_{ \theta  }(x),y)=−logP(y=y|x=x;θ)
  $$

* example\) if y is a continuous random variable and we assume that, given x, it has a Gaussian distribution with mean ${f}\_{θ}$\(x\) and variance ${\sigma}^{2}$

  $$
  −logP(y|x;θ)=\frac { 1 }{ 2 } { ({ f }_{ \theta  }(x)−y) }^{ 1 }/{ σ }^{ 2 }+log(2π{ σ }^{ 2 })
  $$

* minimizing this negative log-likelihood is therefore equivalent to minimizing the squared error loss.

* for discrete variables, the binomial negative log-likelihood cost func- tion corresponds to the conditional log-likelihood associated with the Bernoulli distribution \(also known as cross entropy\) with probability $$p = {f}_{θ}(x)$$ of generating y = 1 given x =$$ x$$


  $$
  {L}_{NLL}=−logP(y|x;θ)={−1}_{y=1}{logp−1}_{y=0}log(1−p)\\ =−ylog{f}_{θ}(x)−(1−y)log(1−{f}_{θ}(x))
  $$




[TukeysBiweight](http://mathworld.wolfram.com/TukeysBiweight.html)