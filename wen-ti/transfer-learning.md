# Transfer Learning

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

## Reference

- How transferable are features in deep neural networks，Jason Yosinski关于transfer learning的论文，2014，NIPS
-  Tzeng, Eric, et al. Simultaneous deep transfer across domains and tasks. ICCV. 2015.