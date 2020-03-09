# 准备工作

安装

```python
pip install torch==0.3.1 --user
```

查看版本

```
import torch
print(torch.__version__)
```

# 矩阵相关

扩充维度：
```
x1 = torch.zeros(10, 10)
x2 = x1.unsqueeze(0)
```

变形

```
imgs = image.view(-1,1,256,256).repeat(1,3,1,1)
```
## 上采样

```python
image_tensor = image_tensor.view(1, 1, img_d, img_h, img_w)
resize_tensor = F.upsample(image_tensor, new_shape, mode='trilinear').data[0, 0]
```
```python
torch.max(input, axis) #return the max value

torch.flatten(input, start_dim, end_dim)# flatten a continuous rang of dims in a tensor
```

# 其他

## debug

查看feature map大小

```python
import torch
from torch.autograd import Variable

fms = model(Variable(torch.randn(1,1,256,256)))
for fm in fms:
    print(fm.size())
```

## 模型

### 加载模型

针对`invalid device ordinal`错误，解决方案是`map_location`

```python
checkpoint = torch.load(self.model_path, map_location=lambda storage, loc: storage)
self.model.load_state_dict(checkpoint['model'], strict=False)
```
针对python2到python3的迁移：

```python
from functools import partial
import pickle
pickle.load = partial(pickle.load, encoding="latin1")
pickle.Unpickler = partial(pickle.Unpickler, encoding="latin1")
model = torch.load(model_file, map_location=lambda storage, loc: storage, pickle_module=pickle)
```
### test model

记得设置 `volatile=True`，否则容易爆显存。

```python
input_img_var = torch.autograd.Variable(images.cuda(), volatile=True)
input_mask_var = torch.autograd.Variable(masks.cuda(), volatile=True)
```

## cuda

设置GPU的方法

```python
torch.cuda.set_device(7)
```

```python
model.cuda() #RAM + 0.9G
```

如果输入图片大小一致：

```python
torch.backends.cudnn.benchmark = True
```

僵尸进程占用gpu，kill的方法：

```shell
ps x |grep python|awk '{print $1}'|xargs kill
```

## 资源

* [pytorch-practice](https://github.com/napsternxg/pytorch-practice)
* [各种评价指标（meter）的pytorch实现](https://github.com/pytorch/tnt/tree/master/torchnet/meter)



