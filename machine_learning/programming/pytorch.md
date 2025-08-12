# Preparation

安装

```python
pip install torch==0.3.1 --user
```

查看版本

```
import torch
print(torch.__version__)
```

    * - pytorch-ligntning：打包好的训练框架 

## Variable

* Pytorch手册：Variable的两个属性：requires_grad和volatile；cuda sematics

## Dataloader

```python
DataLoader(dataset, batch_size=1, shuffle=False, sampler=None,
           batch_sampler=None, num_workers=0, collate_fn=None,
           pin_memory=False, drop_last=False, timeout=0,
           worker_init_fn=None, *, prefetch_factor=2,
           persistent_workers=False)
```

## 矩阵相关

新建tensor
```python
shape = torch.randn([3,2,3])
```

numpy转tensor： 
```python
torch.Tensor()
```

扩充维度：
```python
x1 = torch.zeros(10, 10)
x2 = x1.unsqueeze(0)
>>> print(x2.size())
torch.Size([1, 10, 10])
```

复制元素：
```python
B = 3

torch.repeat_interleave(shape, torch.ones(B, dtype=torch.long)*B, dim=0) #[A,A,A,B,B,B,C,C,C]

shape.repeat(B, 1, 1) # [A,B,C,A,B,C,A,B,C]
```

选择元素：
```python
index = torch.tensor([[0], [1], [2]])
tensor_p = tensor_0.gather(1, index)
print(tensor_p)
```

变形
```python
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

## 其他

## Debug


查看feature map大小

```python
import torch
from torch.autograd import Variable

fms = model(Variable(torch.randn(1,1,256,256)))
for fm in fms:
    print(fm.size())
```

# 模型

## 加载模型

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
## optimizer
    * multiple optimizer分组更新
## test model

记得设置 `volatile=True`，否则容易爆显存。

```python
input_img_var = torch.autograd.Variable(images.cuda(), volatile=True)
input_mask_var = torch.autograd.Variable(masks.cuda(), volatile=True)
```
    * model.eval()的作用只是调整model中受eval影响的层，（比如Adaptive batchnorm？），gradient还是会正常更新
## cuda memory

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

[Memory Leakage with PyTorch](https://medium.com/@raghadalghonaim/memory-leakage-with-pytorch-23f15203faa4)

## 资源

* [pytorch-practice](https://github.com/napsternxg/pytorch-practice)
* [各种评价指标（meter）的pytorch实现](https://github.com/pytorch/tnt/tree/master/torchnet/meter)




# Pytorch lightning

## pl.LightningModule

```python
def configure_optimizers(self):
    #     # Make sure to filter the parameters based on `requires_grad`
    optimizer = torch.optim.SGD(filter(lambda p: p.requires_grad, self.parameters()), lr=self.hparams.lr, momentum=0.9)
    lr_scheduler = {
         'scheduler': torch.optim.lr_scheduler.ExponentialLR(optimizer, gamma=0.9995),
         'name': 'lr'
         }
    return [optimizer], [lr_scheduler]
```