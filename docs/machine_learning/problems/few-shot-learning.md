# Meta
描述数据的数据

# Meta Learning

- Learning Algorithm: Learn from historical data and make predictions given new examples of data
- Meta Learning Algorithm: Learn the output of learning algorithms and make a prediction given predictions made by other models

# Few-shot Learning 

## Support Set vs. Training Set

## Few-shot Learning vs. Supervised Learning

- Supervised Learning
    - query样本类别在训练集中出现
- Few-shot Learning
    - query样本来自未知类别
$$
\begin{aligned}
\mathcal{T} \sim P(\mathcal{T}) \\
\mathcal{T} = (S_{\mathcal{T}}, Q_{\mathcal{T}}) \\
S_{\mathcal{T}} = S_{\mathcal{T}}^s \cup S_{\mathcal{T}}^u \\
Q_{\mathcal{T}} = Q_{\mathcal{T}}^s \cup Q_{\mathcal{T}}^u \\
\end{aligned}
$$