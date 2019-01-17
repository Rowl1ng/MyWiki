# 基本矩阵操作 

```python
## 增加维度：
x2 = x[:, np.newaxis]
## 变形
>>> a = np.arange(10).reshape(2, 5)
array([[ 0,  1,  2,  3,  4],
       [ 5,  6,  7,  8,  9])
## padding
np.pad(a,[(10,10),(10,10)],constant)
## 拍平
a.flatten()
## 逻辑判断
a = np.array(randn(4, 4))
b = 1 * (a > 0) # 返回0、1矩阵
a_list = a.ravel().tolist() # 返回list
```