# list

找list中最大元素的索引：

```python
aa = [1,2,3,4,5]
aa.index(max(aa))
```

# numpy


## 中位数

```python
#求数组a的中位数
np.median(a)

#求数组a的四分位数
np.percentile(a, [25, 50, 75])
```


## 找极值+索引

```python
np.where(arr==3)
```
### 寻找矩阵中最大的几个元素的index

```python
def largest_indices(ary, n):
    """Returns the n largest indices from a numpy array."""
    flat = ary.flatten()
    indices = np.argpartition(flat, -n)[-n:]
    indices = indices[np.argsort(-flat[indices])]
    return np.unravel_index(indices, ary.shape)
```

## 寻址、索引和遍历：

```python
>>> x = np.array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])
>>> x[1:7:2]
array([1, 3, 5])
>>> x[-2:10]
array([8, 9])
>>> x[-3:3:-1]
array([7, 6, 5, 4])
```

## 打印

完整打印：

```python
numpy.set_printoptions(threshold='nan')
```

