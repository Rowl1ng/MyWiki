# 第一章 概论

## 评价函数

* 对于无约束，越是迭代，评价函数下降即可
* 有约束，就得加上限制：是**可行点**

## 收敛

若不可能有限步找到最优解，就要顾及到收敛速度


$$
\begin{aligned}
e_k=x_k-x^*\\
\lim _{k \rightarrow \infty} \frac {||e_{k+1}||}{||e_k||}=C
\end{aligned}
$$


这叫**以**$C$**为因子**$r$**阶收敛于**$x^*$。

* 线性收敛：$||e_{k+1}||\leq C||e_k||$
* 超线性收敛：比前者快，多数如此。


$$
\begin{aligned}
\lim _{k \rightarrow \infty} \frac {||e_{k+1}||}{||e_k||^n}=C \\
\lim _{k \rightarrow \infty} \frac {||e_{k+1}||}{||e_k||}=C \times    \lim _{k \rightarrow \infty} {||e_k||}^{n-1}
\end{aligned}
$$


1. local minimun $\Longrightarrow \nabla f(x)=0, \nabla ^2 f(x) \geq 0$
2. $\nabla f(x)=\nabla ^2 f(x)\Longrightarrow  $ local minimum ,eg. $y=x^3$

## 迭代下降优化算法

寻找一个搜索方向$d_k$使得每次迭代时函数值减小。

首先选取初始点$x_0$，  
loop until $x_{k+1}$满足终止条件：

* 构造搜索方向$d_k$
* 根据 $ d\_k $ 确定步长 $\lambda \_k$
* $x_{k+1}=x_k+\lambda_k d_k$

 需要考虑的问题：

1. 往哪个方向走？$d\_{k}$
2. 走多远？$\lambda_k$
3. 能找到$x_*$吗？
4. 多快能找到？

下面从典型的无优化约束算法开始：

