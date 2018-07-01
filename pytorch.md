# Pytorch总结

```python
pip install torch==0.3.1 --user
```

## debug

```python
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
## cuda

```python
model.cuda() #RAM + 0.9G
```

如果输入图片大小一致：

```python
torch.backends.cudnn.benchmark = True
```

## 资源

* [pytorch-practice](https://github.com/napsternxg/pytorch-practice)



