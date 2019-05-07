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

