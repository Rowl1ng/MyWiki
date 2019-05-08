# Transfer Learning

Finetune经验
论文：How transferable are features in deep neural networks? 
应用场景： domain A有标注数据， domain B有标注数据，期望在A上训练的模型能帮助B的训练。
经验：
	Fine-tune 对深度迁移有着非常好的促进作用
	迁移特征能够增强模型在大型数据集上的表现
	数据集A和B之间关系越强，迁移效果越好
	随着可迁移层数的增加，模型性能下降。比如论文中使用前三层可以取得不错的迁移效果，但随着迁移层数上升，效果反而下降。通常经验是：使用大型数据集pretrain的模型在迁移到小型数据集时，固定前几个block，之后几层的学习率逐层增加，最后几层正常训练。

迁移学习方法：
Domain adaptation：
论文：
-	Simultaneous Deep Transfer Across Domains and Tasks
-	Adversarial Discriminative Domain Adaptation

应用场景：source domain 有标注数据，target domain无标注数据 
思路：目的类似MMD，手法类似GAN。 加入针对representation的domain classifier，计算domain confusion loss，从而最小化的类别间距离（domain discrepency）。换句话说，就是让target domain和source domain共享同一个映射函数，来和domain classifier对抗
Borrowing data
论文：Borrowing Treasures from the Wealthy: Deep Transfer Learning through Selective Joint Fine-Tuning
应用场景：target domain有标注数据很少，而source domain数据量庞大。Source domain包含target domain的相关内容。
思路：构造特征提取器，度量图片的相似度，从而在source domain找到更适合target domain任务的图片。训练的时候对hard sample就从sourcce domain多选一些和它相似的图。
区别于domain融合的思路，这里更偏重颜色、形态上的相似，更像是数据增强的方法。和domain融合相同的是，中间目的都是让source domain和target domain上当前任务都取得好效果，只不过这里是source domain的subset。


Train deep net on “nearby” task for which it is easy to get labels using standard backprop
● E.g. ImageNet classification
● Pseudo classes from augmented data
● Slow feature learning, ego-motion
Cut off top layer(s) of network and replace with supervised objective for target domain
Fine-tune network using backprop with labels for target domain until validation loss starts to increase
 
Bottom n layers can be frozen or fine tuned.
● Frozen: not updated during backprop
● Fine-tuned: updated during backprop Which to do depends on target task:
● Freeze: target task labels are scarce, and we want to avoid overfitting
● Fine-tune: target task labels are more plentiful
In general, we can set learning rates to be different for each layer to find a tradeoff between freezing and fine tuning

- Lower layers: more general features. Transfer very well to other tasks.
- Higher layers: more task specific.
- Fine-tuning improves generalization when sufficientexamples are available.
- Transfer learning and fine tuning often lead to better performance than training from scratch on the target dataset.
- Even features transferred from distant tasks are often better than random initial weights!

Semi-supervised domain adaptation
 
When some labels are available in the target domain, then we can use these when doing domain adaptation. I.e. combine fine tuning and unsupervised domain adaptation.
Tzeng et al. take this a step further and try to simultaneously optimize a loss that maximizes:
1. classification accuracy on both source and target datasets
2. domain confusion of a domain classifier
3. agreement of classifier score distributions across domains
 

## Reference

- How transferable are features in deep neural networks，Jason Yosinski关于transfer learning的论文，2014，NIPS
-  Tzeng, Eric, et al. Simultaneous deep transfer across domains and tasks. ICCV. 2015.:加上对抗的思路，使不同domain的representation不可分