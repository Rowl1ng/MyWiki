# \# 非线性最优化——说人话

# 

# 标签（空格分隔）： 上课

# 

# ---

# 

# koucx@bupt.edu.cn

# optim\_grs@163.com

# 寇彩霞

# 

#  \[TOC\]

#  

# \#\# 凸优化与非凸优化\[^footnote1\]

# 

# 凸优化：

# 

# !\[此处输入图片的描述\]\[1\]

#  

#  非凸优化：

#  

#  !\[此处输入图片的描述\]\[2\]

#  

#  对于凸优化来说，局部最小就是全局最小，因此问题变得简单。

#  

#  \#\# 平滑与非平滑

#  

#  平滑：

#  

#  !\[此处输入图片的描述\]\[3\]

#  处处可导

#  

#  非平滑：

#  

#  !\[此处输入图片的描述\]\[4\]

#  

# \#\# 第一章 概论

# 

# \#\#\# 评价函数

# 

# - 对于无约束，越是迭代，评价函数下降即可

# - 有约束，就得加上限制：是\*\*可行点\*\*

# 

# \#\#\# 收敛

# 

# 若不可能有限步找到最优解，就要顾及到收敛速度

# 

# $$

# e\_k=x\_k-x^\*\\

# \lim \_{k \rightarrow \infty} \frac {\|\|e\_{k+1}\|\|}{\|\|e\_k\|\|}=C

# $$

# 

# 这叫\*\*以$C$为因子$r$阶收敛于$x^\*$\*\*。

# 

# - 线性收敛：$\|\|e\_{k+1}\|\|\leq C\|\|e\_k\|\|$

# - 超线性收敛：比前者快，多数如此。

# 

# $$

# \lim \_{k \rightarrow \infty} \frac {\|\|e\_{k+1}\|\|}{\|\|e\_k\|\|^n}=C\\

# \lim \_{k \rightarrow \infty} \frac {\|\|e\_{k+1}\|\|}{\|\|e\_k\|\|}=C \times \lim \_{k \rightarrow \infty} {\|\|e\_k\|\|}^{n-1}

# $$

# 

# 1. local minimun $\Longrightarrow \nabla f\(x^\*\)=0,\nabla ^2 f\(x^\*\) \geq 0$ 

# 2. $\nabla f\(x^\*\)=0 \& \nabla ^2 f\(x^\*\) &gt;0 \Longrightarrow  $local minimum ,eg. $y=x^3$

# 

# 

# \#\#\# 迭代下降优化算法

# 

# 寻找一个搜索方向$d\_k$使得每次迭代时函数值减小。

# 

# 首先选取初始点$x\_0$，

# loop until $x\_{k+1}$满足终止条件：

# 

# - 构造搜索方向$d\_k$

# - 根据$d\_k$确定步长$\lambda\_k$

# - $x\_{k+1}=x\_k+\lambda\_k d\_k$

# 

# 需要考虑的问题：

# 

# 1. 往哪个方向走？$d\_k$

# 2. 走多远？$\lambda\_k$

# 3. 能找到$x\_\*$吗？

# 4. 多快能找到？

# 

# 下面从典型的无优化约束算法开始：

# 

# \#\# 第二章 无约束优化（线搜索框架下）

# 

# \#\#\# 1. 梯度法（最速下降法）

# 

# 梯度（gradient）：增长最快的方向，导数的高维扩展

# 

# $$

# g\(x\)=\nabla f\(x\)=\[\frac {\partial f\(x\)}{\partial x\_1},\dots,\frac {\partial f\(x\)}{\partial x\_n}\]^T

# $$

# 

# 先回答需要考虑的问题：

# 

# 1. 负梯度方向，“速”非快

# $$

# d\_k=-\frac {\nabla f\(x\_k\)^T}{\|\|\nabla f\(x\_k\)\|\|}

# $$

# 2. 一维搜索求$\lambda\_k$，使得

# $$

# \varphi\(\lambda\)=f\(x\_k+\lambda d\_k\) \\

# f\(x\_k+\lambda\_kd\_k\)=\min\_{\lambda\geq 0}f\(x\_k+\lambda  d\_k\)

# $$

# 

# - 最速下降法适用于寻优过程的前期迭代或作为间插步骤，当接近极值点时，宜选用收敛快的算法.

# 

# 

# \#\#\# 3. 共轭梯度法

# 

# !\[此处输入图片的描述\]\[6\]

# !\[此处输入图片的描述\]\[7\]

# 

# 正如从上面例子中看到的，简单梯度下降算法的一个问题是，它试着摇摆穿越峡谷，每次跟随梯度的方法，以便穿越峡谷。\*\*共轭梯度\*\*通过添加\*\*摩擦力\*\*项来解决这个问题: 每一步依赖于前两个值的梯度然后急转弯减少了。

# 

# 首先理解特征值：

# 

# - 线性变换$\mathcal L$ 

# - $n \times 1$向量$u$ $\Longrightarrow$ 特征向量

# - 比例引子$\lambda$ $\Longrightarrow$ 特征值

# $$

# \mathcal Lu=\lambda u，u\neq 0

# $$

# 也就是经过一个线性变换后，在沿着$u$的方向上只发生了缩放，原来的方向不变，就好像这个方向是枚“定海神针”，牢牢定义了这个线性变换。

# !\[此处输入图片的描述\]\[8\]

# !\[此处输入图片的描述\]\[9\]

# !\[此处输入图片的描述\]\[10\]

# !\[此处输入图片的描述\]\[11\]

# !\[此处输入图片的描述\]\[12\]

# 

# $$

# M=U \Sigma U^T

# $$

# 

# - 第一步：旋转 $U^T$

# - 第二步：$\Sigma$对角矩阵，相当于进行缩放

# - 第三步：旋转 $U$，转回去

# 

# \#\#\#\# 什么是共轭

# 

# - $G$:  $n \times n$对称正定矩阵

# - $\{p\_0,p\_1,\dots,p\_k\}$: 非零向量组合

# 

# 若满足

# 

# $$

# p\_i^T A p\_j=0,\forall i\neq j

# $$

# 

# 说明这一组向量有$A$-\*\*正交性\*\*/\*\*共轭性\*\*，或者说是\*\*线性无关\*\*（n维至多n个），从这儿也能看出来，共轭是正交的扩展：当$A$为单位向量$I$时即是正交，就好像梯度是导数的扩展（？）

# 

# - 所谓共轭方向方法即是所有使用共轭向量作为梯度更新方向的算法。

# - 共轭梯度算法则是在迭代过程中利用梯度下降法来更新共轭向量组。

# 

# 从任意点$x\_0$出发，沿任意下降方向$p\_i$做直线搜索得到$x\_1$，接着沿与$p\_i$共轭的$p\_j$方向搜索，反复迭代

# 

# \#\#\# 2. Newton Method

# 

# 牛顿法使用\*\*局部二元近似\*\*来计算跳跃的方向。为了这个目的，他们依赖于函数的\*\*前两个导数梯\*\*度和\*\*Hessian\*\*。

# 

# $$

# f\(x\)=\frac 12 x^TAx+b^Tx 

# $$

# $g\(x\)$是quadratic$f\(x\)$的导数，求$f\(x\)$的极小点则是求$g\(x\)$的零点，于是有：

# 

# \#\#\#\# 一维

# 

# 割线法\(secant\)

# !\[牛顿法\]\[13\]

# 

# $$

# x\_{k+1}=x\_k-\frac {g\(x\_k\)}{g'\(x\_k\)}

# $$

# 

# 

# \#\#\#\# 推广至高维

# 

# Hessian矩阵$\mathbf H=\nabla^2 f\(\mathbf x\)$

# $$

# \mathbf x\_{k+1}=\mathbf x\_k-\frac {\nabla f\(\mathbf x\_k\)}{\nabla^2f\(\mathbf x\_k\)}

# $$

# 

# \#\#\#\# Lipschitz连续

# 

# \#\#\#\# 优点

# 

# - 二阶收敛

# - 也可能不收敛

# $$

# x\_1=1,f'\(x\_1\)=-2 ,f''\(x\_1\)=1 \\

# x\_2=1,f'\(x\_2\)=2 ,f''\(x\_1\)=1 \\

# x\_3=-1,\dots

# $$

# 

# \#\#\#\# 缺点

# 

# - 要求Hessian矩阵要\*\*可逆\*\*

# - 需要计算二阶导数和逆矩阵，计算量和存储量

# 开销较大

# 

# \#\#\# 2. Quasi-Newton Method

# 

# 近似Hessian的逆矩阵

# 

# $$

# d\_k^{QN}=-H\_kg\_k\\

# \{H\_0,\dots,H\_n\}

# $$

# 

# 无记忆拟牛顿法和共轭梯度法联系最紧密

# 

# \#\#\#\# BFGS

# 

# \(Broyden-Fletcher-Goldfarb-Shanno算法\) 改进了每一步对Hessian的近似。

# 

# \*\*L-BFGS\*\*:

# 限制内存的BFGS介于BFGS和共轭梯度之间: 在非常高的维度 \(&gt; 250\) 计算和翻转的Hessian矩阵的成本非常高。L-BFGS保留了低秩的版本。此外，scipy版本, \[scipy.optimize.fmin\_l\_bfgs\_b\(\)\]\[14\], 包含箱边界:

# 

# 

# \#\#\#\# Conjugate Gradients\(CG\)

# 

# 

# 

# \#\#\# 3.线性搜索

# 

# - 搜索方向$d\_k$

# - 步长因子$\alpha\_k$

# $$

# x\_{k+1}=x\_k+\alpha\_k d\_k \\

# \phi\(\alpha\)=f\(x\_k+\alpha d\_k\) 

# $$

# 

# 目标就转化成了$\phi\(\alpha\_k\)&lt;\phi\(0\)$。

# 

# \#\#\#\# 精确线性搜索

# 

# $$

# \alpha\_k=\min\{\alpha&gt;0\|\nabla f\(x\_k+\alpha d\_k\)^Td\_k=0\}

# $$

# 目标在于沿着$d\_k$方向让目标函数达到极小。这样得到的$\alpha\_k$叫\*\*精确步长因子\*\*。

# 

# 

# - $A$ : 半正定对称矩阵

# 

# $$

# f\(x\)=\frac 12 x^TAx+b^Tx 

# $$

# 

# $$

# \phi\(\alpha\)=\frac 12 \(x\_k+\alpha d\_k\)^T A \(x\_k+\alpha d\_k\)+b^T\(x\_k+\alpha d\_k\)\\

# = \frac 12 d\_k^tAd\_k \cdot \alpha^2+\(Ax\_k+b\)^T d\_k \alpha \\

# \phi '\(\alpha\)=d\_k^T A d\_k \cdot \alpha+\(Ax\_k+b\)^Td\_k=0 \\

# \nabla f\(x\)=Ax+b

# $$

# 精确步长（stride）就可以表示出来了：

# $$

# \alpha\_k '=-\frac{\nabla f\(x\_k\)^Td\_k}{d\_k^TAd\_k}

# $$

# 

# 正交条件

# 

# 

# 

# \#\#\#\# 不精确线性搜索

# 

# \#\#\#\#\# 进退法——确定初始搜索区间

# 

# 高-低-高

# 

# 

# 

# 

# 

# \#\#\#\# 二次收敛性

# 

# 当目标函数是二次函数时，共轭方向法最多经过$N$步迭代（$N$=向量维数），就可达到极小值点。

# 

# 

# \#\#\# bb法

# 

# \#\# 第三章 有约束优化

# 

# $$

# L\(x,\mathbf \lambda\)=f\(x\)-\sum\_{i=1}^m\lambda\_i c\_i\(x\)

# $$

# 

# - $\mathbf c\(x\)=\(c\_1\(x\),\dots,c\_n\(x\)\)^\top$

# - $\mathbf \lambda=\(\lambda\_1,\dots,\lambda\_n\)^\top$

# - $\nabla\_xL\(x,\mathbf \lambda\)=\nabla f\(x\)-\mathbf \lambda^\top \nabla \mathbf c\(x\)=0$

# - $\nabla\_\lambda L\(x,\mathbf \lambda\)=-\mathbf c\(x\)=0$

# 

# !\[此处输入图片的描述\]\[15\]

# 

# $-1 &lt; x\_1 &lt; 1$

# $-1 &lt; x\_2 &lt; 1$

# 

# \#\#\# 1. 信赖域

# 

# - 当前点，用二次函数近似当前函数：$q\_k\(\cdot\)\approx f\(x\)$

# - 求解$q\_k$极小值点作为下一次试探点

# - 先确定区域$\Delta k$，可选不同范数

#     - 无穷范数：$\max\{\|x\_i\|\}$ 

# 

# - $s\_k^c=-\tau\_k \frac{\Delta k}{\|\|g\_k\|\|}g\_k$

# - $\tau\_k=\begin{cases}

# 1, &g\_k^\top B\_kg\_k\leq0\cr \min\{\frac{\|\|g\_k\|\|^3}{\Delta kg\_k^\top B\_kg\_k},1\}, &otherwise

# \end{cases}$

# 

# 动态调整

# 

# \#\#\#\# 折线法

# 

# $\Delta\_k=\frac 12$，第$k$步求解子问题时用双折线法求解

# 

# $$

# f\(x\)=x\_1^4+x\_1^2+x\_2^2

# $$

# 

# 由于$\mathbf x\_k=\(1,1\)^\top$，则$g\_k=\nabla f\(\mathbf x\_k\)=\left\[

# \begin{matrix}

# 4x\_1^3+2x\_1\\2x\_2

# \end{matrix}

# \right\]$，从而得到$g\_k=\(6,2\)^\top$。

# 同理可得：

# 

# $$

# G\_k=\nabla^2 f\(\mathbf x\_k\)=\left\[

# \begin{matrix}

# 12x\_1^2+2&0\\

# 0&2

# \end{matrix}

# \right\]=\left\[

# \begin{matrix}

# 14&0\\

# 0&2

# \end{matrix}

# \right\]\\

# s\_k^N=-G\_k^{-1}g\_k=\left\[

# \begin{matrix}

# - \frac 37\\

# -1

# \end{matrix}

# \right\]\\

# s\_k^c=-\frac {\|\|g\_k\|\|\_2^2}{g\_k^\top G\_kg\_k} g\_k\approx\(-0.469,-0.156\)^\top

# $$

# 

# 由于$\|\|s\_k^c\|\|\cong 0.496 &lt; \Delta\_k$，从而需要计算牛顿步$s\_k^{\hat N}$，有

# 

# $$

# \gamma=\frac {\|\|g\_k\|\|\_2^4}{\(g\_k^\top G\_kg\_k\)\(g\_k^\top G\_k^{-1}g\_k\)}=\frac{\(40\)^2}{\(512\)\(\frac{32}7\)}\cong 0.684\\

# \eta=0.8\gamma +0.2\cong 0.747,\\

# s\_k^{\hat N}=\eta s\_k^N \cong \left\[

# \begin{matrix}

# - 0.320\\

# - 0.747

# \end{matrix}

# \right\]

# $$

# 

# 由于$\|\|s\_k^{\hat N}\|\|&gt;\Delta\_k$，故取双折线步长为$s\_k=s\_k^c+\lambda\(s\_k^{\hat N}-s\_k^c\),\lambda \in \(0,1\)$，使得$\|s\_k^{\hat N}\|\|=\Delta\_k$。

# 解二次方程：

# 

# $$

# \|\|s\_k^c+\lambda\(s\_k^{\hat N}-s\_k^c\)\|\|\_2^2=\Delta\_k^2

# $$

# 

# 得$\lambda \cong 0.876$。因此，

# 

# $$

# s\_k=s\_k^c+\lambda\(s\_k^{\hat N}-s\_k^c\)=\left\[

# \begin{matrix}

# - 0.340\\

# - 0.669

# \end{matrix}

# \right\]\\

# \mathbf x\_{k+1}=\mathbf x\_k+s\_k=\left\[

# \begin{matrix}

# - 0.660\\

# - 0.331

# \end{matrix}

# \right\]

# $$

# 

# \#\#\# 2. 积极集法

# 

# - $y\in \{y\|\nabla\_ic\(x^\*\)^\top y=0,\forall i\}$在$c\(x\)=0$这个超平面上，等价于在无约束情况$y\in \mathbb R^n$。

# - 用脚标的集合表示哪些到达边界约束。

#     - $x^\*$处的积极集：$I\(x^\*\)=\{i\|c\_i\(x^\*\)=0\}\Longleftrightarrow \exists \lambda\_0,\dots,\lambda\_n\geq0  s.t. \lambda\_0\nabla f\(x^\*\)-\sum\_{i=1}^m \lambda\_i^\* \nabla\_ic\_i\(x^\*\)=0$ 

#     - 如果$\nabla c\_i\(x^\*\)\(i\in f\(x^\*\)\)$线性无关，则存在向量$\mathbf \lambda^\*$，使得$\nabla f\(x^\*\)-\sum\_{i=1}^m \lambda\_i^\* \nabla\_ic\_i\(x^\*\)=0,\lambda\_i^\*\geq0对偶变量,\lambda\_i^\*c\_i\(x^\*\)=0互补松弛条件$

#     - 带约束问题$\rightarrow$方程组求解

# - 目标函数下降方向集合$F=\{d\|\nabla f\(x\)^\top d&lt;0\}$

# - 可行方向：$D=\{d\|\nabla c\_i\(x\)^\top d\geq0,i\in I\(x\)\}$

# 

# $$

# \begin{cases}

# \nabla f\(x\)^\top d&lt;0\cr -\nabla c\_i\(x\)^\top d\leq0

# \end{cases}无解\Longleftrightarrow F\_{x^\*}\cap D\_{x^\*}=\phi

# $$

# 

# &gt; Gordan定理：$\mathbf A\mathbf x&gt;0$有解$\Longleftrightarrow$不存在$\mathbf y\geq0$使得$\mathbf A^\top \mathbf y=0$

# 

# 举例：用积极集法求解

# 

# $$

# \min x\_1^2-x\_1x\_2+2x\_2^2-x\_1-10x\_2\\

# s.t. -3x\_1-2x\_2\geq -6\\

# x\_1\geq0,x\_2\geq 0

# $$

# 

# 变为标准型：

# 

# $$

# \min x\_1^2-x\_1x\_2+2x\_2^2-x\_1-10x\_2\\

# s.t. 3x\_1+2x\_2\leq 6\\

# -x\_1\leq0,\\

# -x\_2\leq 0

# $$

# 

# 记上面的约束为1到3

# 

# $$

# G=

# \left\[

# \begin{matrix}

# 2&-1\\ 

# -1&4

# \end{matrix}

# \right\],

# r=

# \left\[

# \begin{matrix}

# -1\\ 

# -10

# \end{matrix}

# \right\],\\

# A=

# \left\[

# \begin{matrix}

# 3&2\\ 

# -1&0\\

# 0&-1

# \end{matrix}

# \right\],

# b=

# \left\[

# \begin{matrix}

# 6\\ 

# 0\\

# 0

# \end{matrix}

# \right\]

# $$

# 

# 计算$\nabla f\(\mathbf x\)=\(2x\_1-x\_2-1,4x\_2-x\_1-10\)^\top$。因为$\mathbf x^1=\(\frac 27,\frac {18}7\)^\top$，所以有效约束$I\(\mathbf x^1\)=\{1\}$，$\nabla f\(\mathbf x^1\)=\(-3,0\)^\top$，相应的等式约束问题为

# 

# $$

# \min d\_1^2-d\_1d\_2+2d\_2^2-d\_1-10d\_2\\

# s.t. 3d\_1+2d\_2= 6

# $$

# 

# 由等式约束问题的一阶必要条件得到

# 

# $$

# \left\[

# \begin{matrix}

# 2&-1&3\\ 

# -1&4&2\\

# 3&2&0

# \end{matrix}

# \right\]

# \left\[

# \begin{matrix}

# d\_1\\d\_2\\\lambda\_1

# \end{matrix}

# \right\]

# =

# \left\[

# \begin{matrix}

# 1\\10\\6

# \end{matrix}

# \right\]

# $$

# 

# 求解方程组得到

# $d\_1=\frac 12,d\_2=\frac 94,\lambda\_1=\frac 34&gt;0$，即$\mathbf \lambda=\(\frac 34,0,0\)^\top$，$\mathbf x^2=\(\frac 12,\frac 94\)^\top$是最优解。

# 

# 

# 

# \#\#\# 3. 直接法

# 

# 1. $x\_1\in \mathbb R^n,d\_1,\dots,d\_n线性无关$

# 2. $x\_1$沿$d\_1$线性搜索得$x\_2$，即$x\_2$是$f\(x\)$在$\{z\|z=x\_1+\alpha\_1 d\_1\}$上的极小值点

# 3. $\beta\_1,\dots,\beta\_n&gt;0,\nu^{\(1\)}=x\_2+\beta\_2d\_2$，令$d\_2=\nu^{\(2\)}-x\_2$，$\nu^{\(2\)}$是$f\(x\)$在$\{z\|z=\nu^{\(1\)}+\alpha\_1 d\_1\}$上的极小值点

#     - $\nabla f\(\nu^{\(1\)}\)^\top d\_1=0,\nabla f\(x\_2\)^\top d\_1=0$ 

#     - $d\_2^\top Hd\_1=0$\(共轭，$d\_2=\nu^{\(2\)}-x\_2$\)

# 4. 令$d\_3=\nu^{\(3\)}-x\_3$，则$\nu^{\(3\)}$是$f\(x\)$在$\{z\|z=x\_1+\alpha\_1 d\_1+\alpha\_2 d\_2\}$上的极小值点

# 5. $x\_{k+1}$是$f\(x\)$在$\{z\|z=x\_1+\sum\_{i=1}^k\alpha\_i d\_i\}$上的极小值点，令$d\_{k+1}=\nu^{\(k+1\)}-x\_{k+1}$

#     

# \#\# Reference 

# 

# - 《最优化理论与方法》陈宝林

# - 《非线性优化计算方法》袁亚湘

# -  《Numerical Optimization》Jorge Nocedal

# 

# \[^footnote1\]: 《\[2.7 数学优化：找到函数的最优解\]\[16\]》

# 

# \#\# 术语表

# 

# 二阶（quadratic）

# 

# \#\# 其他

# 

# \#\#\# 0.618法

# 

# 用0.618法寻找最佳点时，虽然不能保证在有限次内准确找出最佳点，但随着试验次数的增加，最佳点被限定在越来越小的范围内，即存优范围会越来越小。用存优范围与原始范围的比值来衡量一种试验方法的效率，这个比值叫精度。用0.618法确定试点时，每一次实验都把存优范围缩小为原来的0.618.因此，n次试验后的精度为：

# 

# $$

# \delta \_n=0.618^n

# $$

# 

# \#\#\# KKT条件

# 

# !\[kkt\]\[5\]

# 

# In mathematical optimization, the \*\*Karush–Kuhn–Tucker\*\* \(KKT\) conditions \(also known as the \*\*Kuhn–Tucker conditions\*\*\) are first order necessary conditions for a solution in \*\*nonlinear\*\* programming to be optimal, provided that some regularity conditions are satisfied. Allowing \*\*inequality constraints\*\*, the KKT approach to nonlinear programming generalizes the method of \*\*Lagrange multipliers\*\*, which allows only \*\*equality constraints\*\*. The system of equations corresponding to the KKT conditions is usually not solved directly, except in the few special cases where a closed-form solution can be derived analytically. In general, many optimization algorithms can be interpreted as methods for numerically solving the KKT system of equations.

# 

# \#\# 编程

# 

# \[Anaconda\]\[17\]

# 

# 

#   \[1\]: http://www.scipy-lectures.org/\_images/plot\_convex\_1.png

#   \[2\]: http://www.scipy-lectures.org/\_images/plot\_convex\_2.png

#   \[3\]: http://www.scipy-lectures.org/\_images/plot\_smooth\_1.png

#   \[4\]: http://www.scipy-lectures.org/\_images/plot\_smooth\_2.png

#   \[5\]: https://upload.wikimedia.org/wikipedia/commons/thumb/5/5d/Inequality\_constraint\_diagram.svg/440px-Inequality\_constraint\_diagram.svg.png

#   \[6\]: http://www.scipy-lectures.org/\_images/plot\_gradient\_descent\_5.png

#   \[7\]: http://www.scipy-lectures.org/\_images/plot\_gradient\_descent\_105.png

#   \[8\]: http://images2015.cnblogs.com/blog/609274/201610/609274-20161005104515332-924434508.png

#   \[9\]: http://images2015.cnblogs.com/blog/609274/201610/609274-20161005104611192-1570042504.png

#   \[10\]: http://images2015.cnblogs.com/blog/609274/201610/609274-20161005104718020-1578473912.png

#   \[11\]: http://images2015.cnblogs.com/blog/609274/201610/609274-20161005104838270-718234550.png

#   \[12\]: http://images2015.cnblogs.com/blog/609274/201610/609274-20161005104907254-412111092.png

#   \[13\]: https://upload.wikimedia.org/wikipedia/commons/thumb/e/e0/NewtonIteration\_Ani.gif/300px-NewtonIteration\_Ani.gif

#   \[14\]: http://docs.scipy.org/doc/scipy/reference/generated/scipy.optimize.fmin\_l\_bfgs\_b.html\#scipy.optimize.fmin\_l\_bfgs\_b

#   \[15\]: http://www.scipy-lectures.org/\_images/plot\_constraints\_1.png

#   \[16\]: https://wizardforcel.gitbooks.io/scipy-lecture-notes/content/12.html

#   \[17\]: http://www.jianshu.com/p/2f3be7781451



