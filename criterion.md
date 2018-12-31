# 评价指标

## FROC曲线

先来回顾一下精确率（precision）和召回率（recall）：

实际上非常简单，精确率是针对我们预测结果而言的，它表示的是预测为正的样本中有多少是真正的正样本。那么预测为正就有两种可能了，一种就是把正类预测为正类(TP)，另一种就是把负类预测为正类(FP)，也就是
$$\frac {TP}{TP+FP}$$
而召回率是针对我们原来的样本而言的，它表示的是样本中的正例有多少被预测正确了。那也有两种可能，一种是把原来的正类预测成正类(TP)，另一种就是把原来的正类预测为负类(FN)。

$$\frac {TP}{TP+FN}$$

![FROC](http://static.zybuluo.com/sixijinling/19kbgb66gbhj9yzropa03qm7/tp.png)

FROC曲线（Free-response ROC）要从[ROC曲线](https://zh.m.wikipedia.org/wiki/%E9%9D%88%E6%95%8F%E5%BA%A6%E5%92%8C%E7%89%B9%E7%95%B0%E5%BA%A6)说起，和我们常用的accuracy和recall也可以联系起来看。主要是为了平衡两者：

- TPR（True Positive Rate）/ **灵敏度** （Sensitivity） :在所有真的得了癌症的人中，我们要尽最大努力把这些病人都找出来，等同于正例的召回率。
    - 所有 test
scans 中检测到的真结节的比例： TP/n 其中n是所有scans中真结节的总数, so n = 207 in this stud 
$$\frac {TP}{TP+FN}$$
- FPR（False Positive Rate）/(1 - Specificity)：在所有健康人中，我们要尽最大努力**避免误判**成癌症患者，或者说，是尽最大努力把这些健康人都找出来，等同于反例的召回率。
    -  FP/m 其中m是scans的总数。 so m = 50 in this study $$\frac{FP}{FP+TN}$$

这样每一个decision threshhold，都有各自的TP、TN、FP、FN，都对应着曲线上的一个点

