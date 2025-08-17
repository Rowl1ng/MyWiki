# RoadNN

## 识别裂缝

《*One Application of Neural Networks for Detection of Defects using Video Data Bases: Identification of Road Distresses*》
主要是预处理部分：从阈值到像素组合成object；NN只是用来分类纵横裂纹。

由于图片中不同位置的背景路面的灰度不尽相同, 需要通过局部阈值来提取裂缝；
将图片分割为40*40 pixels的小块，称小块的灰度平均值为$m$、灰度标准差为$\sigma$，
$$e=\frac {\sum{e_c}\times v}{\sum e_c}$$
$e_c$为小块内每个像素与它邻近像素的最大偏差值，$v$为中央像素的值；
使用两个阈值：
$$
\begin{aligned}
v_1=\min(m-a_1 \times \sigma,a_2 \times e)\\
v_2=\min(m-b_1 \times \sigma,b_2 \times e)
\end{aligned}
$$
系数$$a_1, a_2, b_l, b_2$$根据经验设置; $v_1$用于选择暗像素，$v_2$用于选择黑色或很暗的像素。

$$neighborhood=d\times \frac a{size_1 \times size_2}$$
根据距离聚合像素，组成object：

![object.png-75.7kB][3]

之后对裂缝类型进行分类：纵、横、龟裂

![crack1.png-150.1kB][4]
![crack2.png-326.1kB][5]





  [3]: http://static.zybuluo.com/sixijinling/0g3xcicmpt505osca78vshsr/object.png
  [4]: http://static.zybuluo.com/sixijinling/ypqo55ite92kx3udk6zdpkl8/crack1.png
  [5]: http://static.zybuluo.com/sixijinling/foqyudl4n0f1kodujkcp30rq/crack2.png