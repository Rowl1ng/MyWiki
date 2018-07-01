# 调试

pdb ： python debugger，c是下一步

python的浮点数算数错误

```
>>> 1.1 + 2.2
3.3000000000000003
```
使用pdb.set_trace()下断点

```python
import pdb
pdb.set_trace()
```

## assert

```python
assert expression
```
等价于
```python
if not expression:
    raise AssertionError
```