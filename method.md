# 常用操作

## 上采样

```python
image_tensor = image_tensor.view(1, 1, img_d, img_h, img_w)
resize_tensor = F.upsample(image_tensor, new_shape, mode='trilinear').data[0, 0]
```