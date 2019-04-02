# Data Imbalance

* resampling；cost-sensitive：迭代优化cost matrix，类间损失，根据每个epoch的训练错误调整loss权重（re-weighting），和五元组的combine，考虑sub-cluster的weight，iterative weighting matrix
* under-sampling with bagging；NearMiss（数据清洗）；over-sampling:SMOTE;

## Reference

- [Learning from Imbalanced Classes](https://www.svds.com/learning-imbalanced-classes/)