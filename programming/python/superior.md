# 装饰器（decorator）

python的@property；装饰器（decorators）：函数的函数

[理解 Python 装饰器看这一篇就够了](https://foofish.net/python-decorator.html)

```
@a
@b
@c
def f ():
    pass
```
等同于
```
f = a(b(c(f)))
```

# 迭代

通常我们使用`enumerate`：

```python
Count = -1
for count,line in enumerate(open(thefilepath,‘rU’))： 
    pass
    Count += 1
```

如果给定一个list或tuple，我们可以通过for循环来遍历这个list或tuple，这种遍历我们称为迭代（Iteration）。
在for循环中，Python将自动调用工厂函数iter()获得迭代器，自动调用next()获取元素，还完成了检查StopIteration异常的工作。
常用的几个内建数据结构tuple、list、set、dict都支持迭代器，字符串也可以使用迭代操作。你也可以自己实现一个迭代器，如上所述，只需要在类的'_iter_'方法中返回一个对象，这个对象拥有一个next()方法，这个方法能在恰当的时候抛出StopIteration异常即可。

```python
    group = self.group_sets[nx][ny]
    group_member = next(iter(group))  
```

# 脚本编程与系统调用

## 调用shell脚本
比如这里通过在主函数中获取模型名来调用相应的测试脚本：
```python
import sys
model_name = main()
os.system('sh test_sample.sh {}'.format(model_name))
```
##　执行外部命令并获取它的输出

```
import subprocess
result = subprocess.check_output([
                "python", 
                "dejavu.py",
                '-r',
                'file', 
                self.test_folder + "/" + f])
```

这段代码执行一个指定的命令并将执行结果以一个字节字符串的形式返回。 如果你需要文本形式返回，加一个解码步骤即可。例如：

```
result = result.decode('utf-8')
```

