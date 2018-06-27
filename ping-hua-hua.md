# 平滑化处理

#### 1. 双边滤波

> 该滤波器可以在保证**边界清晰**的情况下有效的**去掉噪声**。它的构造比较复杂，既考虑了图像的**空间关系**，也考虑图像的**灰度关系**。双边滤波同时使用了空间高斯权重和灰度相似性高斯权重，确保了边界不会被模糊掉。

```
 `cv2.bilateralFilter(src, d, sigmaColor, sigmaSpace[, dst[, borderType]])`
 `cv2.bilateralFilter(image, 9, 90,16)`
```

cv2.bilateralFilter(img,d,’p1’,’p2’)函数有四个参数需要，d是领域的直径，后面两个参数是空间高斯函数标准差和灰度值相似性高斯函数标准差。

`cv2.bilateralFilter(img, 9, 90,16)`

#### 2. 高斯滤波

必须是奇数

```python
   img = cv2.GaussianBlur(image,(7,7),0)
```
