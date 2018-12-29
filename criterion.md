# 评价指标

## FROC曲线

先来回顾一下精确率（precision）和召回率（recall）：

实际上非常简单，精确率是针对我们预测结果而言的，它表示的是预测为正的样本中有多少是真正的正样本。那么预测为正就有两种可能了，一种就是把正类预测为正类(TP)，另一种就是把负类预测为正类(FP)，也就是
$$\frac {TP}{TP+FP}$$
而召回率是针对我们原来的样本而言的，它表示的是样本中的正例有多少被预测正确了。那也有两种可能，一种是把原来的正类预测成正类(TP)，另一种就是把原来的正类预测为负类(FN)。
$$\frac {TP}{TP+FN}$$

![](http://static.zybuluo.com/sixijinling/19kbgb66gbhj9yzropa03qm7/tp.png)
FROC曲线（Free-response ROC）要从[ROC曲线](https://zh.m.wikipedia.org/wiki/%E9%9D%88%E6%95%8F%E5%BA%A6%E5%92%8C%E7%89%B9%E7%95%B0%E5%BA%A6)说起，和我们常用的accuracy和recall也可以联系起来看。主要是为了平衡两者：

- TPR（True Positive Rate）/ **灵敏度** （Sensitivity） :在所有真的得了癌症的人中，我们要尽最大努力把这些病人都找出来，等同于正例的召回率。
    - 所有 test
scans 中检测到的真结节的比例： TP/n 其中n是所有scans中真结节的总数, so n = 207 in this stud 
$$\frac {TP}{TP+FN}$$
- FPR（False Positive Rate）/(1 - Specificity)：在所有健康人中，我们要尽最大努力**避免误判**成癌症患者，或者说，是尽最大努力把这些健康人都找出来，等同于反例的召回率。
    -  FP/m 其中m是scans的总数。 so m = 50 in this study $$\frac{FP}{FP+TN}$$

这样每一个decision threshhold，都有各自的TP、TN、FP、FN，都对应着曲线上的一个点

[Luna官方说明][15]：The evaluation is performed by measuring the detection sensitivity of the algorithm and the corresponding false positive rate per scan. 
-  TP判定（hit criterion）：candidate必须是在standard reference 中以nodule center为中心的半径R（结节直径除2）之内。hit到一个正例之后就将这个例子从表中除去，保证不重复计数。也就是说，又有第二个预测点hit到这个结节时就被忽略，不算TP。
-  FP判定：在设定的半径距离内没有hit到任何reference结节. 
-  忽略的情况：Candidates that are detecting irrelevant findings (see Data section in this page for definition) are ignored during the evaluation and are not considered either false positives or true positives.

最终的分数是取7个横座标点对应的纵座标（TPR）均值：

- 横座标（FPR）： 1/8, 1/4, 1/2, 1, 2, 4, and 8 FPs per scan
-  完美是1，最低是0. Most CAD systems in clinical use today have their internal threshold set to operate somewhere between 1 to 4 false positives per scan on average. Some systems allow the user to vary the threshold. To make the task more challenging, we included low false positive rates in our evaluation. This determines if a system can also identify a significant percentage of nodules with very few false alarms, as might be needed for CAD algorithms that operate more or less autonomously.

论文里关于FROC的说明：

sensitivity是关于FPR平均值的函数：

 This means that the sensitivity (y) is plotted as a function of the average number of false positive markers per scan (). 

- 计算FROC曲线：阈值为t，概率p≥ t则判定为结节，由此计算TPR、FPR，得到曲线上的一个点。
- 图像ID号（seriesuid）：发现结果所在的scan的名字。
- 三维坐标：浮点数（用小数点，不要用逗号）。注意：我们提供的数据的第一个体素（voxel） 的坐标是 (-128.6,-175.3,-298.3). Coordinates are given in world coordinates or millimeters. You can verify if you use the correct way of addressing voxels by checking the supplied coordinates for the example data, the locations should refer to locations that hold a nodule.
- probability : 描述为真结节的可疑程度，浮点数。通过调节对它设定的**阈值**来计算 **FROC曲线**。