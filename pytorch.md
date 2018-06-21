# Pytorch总结

```python
pip install torch==0.3.1 --user
```

## 模型

### 加载模型

```python
checkpoint = torch.load(self.model_path, map_location=lambda storage, loc: storage)
self.model.load_state_dict(checkpoint['model'], strict=False)
```

## 资源

* [pytorch-practice](https://github.com/napsternxg/pytorch-practice)



