# scikit-image
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

## 连通区域

```
from skimage import measure
single_labels = measure.label(connected_mask, connectivity=2)
single_regions = measure.regionprops(single_labels)
```

