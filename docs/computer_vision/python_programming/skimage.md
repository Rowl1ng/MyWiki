
## 安装

```
pip install scikit-image
```

## threshold（二值化）

```
from skimage import filters
```
### Li

Compute threshold value by Li’s iterative Minimum Cross Entropy method.

```
thresh = filters.threshold_li(img)
```

### Ostu

```
thresh = filters.threshold_otsu(img)
```
# 连通区域：regionprops

```
属性名称    类型  描述
area    int     区域内像素点总数
bbox    tuple   边界外接框(min_row, min_col, max_row, max_col)
centroid    array　　     质心坐标（y,x）
convex_area     int     凸包内像素点总数
convex_image    ndarray     和边界外接框同大小的凸包　　
coords  ndarray     区域内像素点坐标
Eccentricity    float   离心率
equivalent_diameter     float   和区域面积相同的圆的直径
euler_number    int　　   区域欧拉数
extent      float   区域面积和边界外接框面积的比率
filled_area     int     区域和外接框之间填充的像素点总数
perimeter   float   区域周长
label   int     区域标记
```

找最大连通区域：
```python
from skimage import measure
def get_max_bbbox(mask):
    single_labels = measure.label(mask, connectivity=2)
    single_regions = measure.regionprops(single_labels)
    if not single_regions:
        return None
    area = [ele.area for ele in single_regions]
    largest_blob_ind = np.argmax(area)
    bbox = single_regions[largest_blob_ind].bbox #(y1,x1,y2,x2)
    return box
```

# Reference

- [https://www.cnblogs.com/denny402/p/5166258.html](https://www.cnblogs.com/denny402/p/5166258.html)