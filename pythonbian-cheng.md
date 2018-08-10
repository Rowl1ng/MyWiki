## 常用图像操作
判断bbox是否重合
```python
def if_in(predict, mask):
    x1, y1, w1, h1 = predict
#     print mask[x1:x1+w1, y1:y1+h1, :].sum()
    if mask[y1:y1+h1,x1:x1+w1, :].sum() > 0:
        return True
    return False
def if_overlap(predict, label, cutoff=.1):
    x1, y1, w1, h1 = predict
    x2, y2, w2, h2 = label
    predict_area = w1 * h1
    roi_area = w2 * h2
    dx = min(x1 + w1, x2 + w2) - max(x1, x2)
    dy = min(y1 + h1, y2 + h2) - max(y1, y2)
    if dx > 0 and dy > 0:
        inter_area = dx * dy
    else:
        return False
    return inter_area * 1.0/roi_area > cutoff or inter_area * 1.0/predict_area > cutoff
```