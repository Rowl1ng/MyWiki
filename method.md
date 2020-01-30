# 常用操作

## 上采样

```python
image_tensor = image_tensor.view(1, 1, img_d, img_h, img_w)
resize_tensor = F.upsample(image_tensor, new_shape, mode='trilinear').data[0, 0]
```
```python
torch.max(input, axis) #return the max value

torch.flatten(input, start_dim, end_dim)# flatten a continuous rang of dims in a tensor
```

