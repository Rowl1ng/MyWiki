# 阈值处理

> 一般使得图像的像素值更单一、图像更简单。阈值可以分为全局性质的阈值，也可以分为局部性质的阈值，可以是单阈值的也可以是多阈值的。

#### 1.全局性的阈值

#### 2.自适应的阈值

> 通过某种算法分别为不同的区域计算不同的阈值(自适应的阈值)，然后再根据每个区域的阈值具体地去处理每个区域

![](http://static.zybuluo.com/sixijinling/8dy1l0qe9n3y7tjgukglu51t/Image1.png)

可以看到上述窗口大小使用的为11，当窗口越小的时候，得到的图像越细。想想一下，如果把窗口设置足够大以后（不能超过图像大小），那么得到的结果可能就和第二幅图像的相同了。

```python
binImg = cv2.adaptiveThreshold( img , 1 , cv2.ADAPTIVE_THRESH_MEAN_C , cv2.THRESH_BINARY, 11 , 2 ) 
```

该函数需要填6个参数：

- 原始图像
- 像素值上限
- 自适应方法Adaptive Method: 
	- cv2.ADAPTIVE\_THRESH\_MEAN\_C ：领域内均值
	- cv2.ADAPTIVE\_THRESH\_GAUSSIAN\_C ：领域内像素点加权和，权 重为一个高斯窗口
- 值的赋值方法：只有cv2.THRESH\_BINARY 和cv2.THRESH\_BINARY\_INV
- Block size:规定领域大小（一个正方形的领域）
- 常数C，阈值等于均值或者加权值减去这个常数（为0相当于阈值 就是求得领域内均值或者加权值） 

这种方法理论上得到的效果更好，相当于在动态自适应的调整属于自己像素点的阈值，而不是整幅图像都用一个阈值。

```python
~~     cv2.THRESH_BINARY # 黑白二值
~~     binImg = cv2.adaptiveThreshold(img, 1, cv2.ADAPTIVE_THRESH_MEAN_C, cv2.THRESH_BINARY, 55, -3)#cv2.bilateralFilter(binImg, 9, 90,16)
~~     #binImg = cv2.GaussianBlur(binImg, (3,3), 0)
~~     #ret, binImg = cv2.threshold(img, 35000, 1, cv2.THRESH_BINARY+cv2.THRESH_OTSU)
~~     plt.imshow(binImg, cmap = 'gray', interpolation = 'bicubic')
~~     plt.xticks([]), plt.yticks([])  # to hide tick values on X and Y axis
```
