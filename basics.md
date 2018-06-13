## 基础
---

- `pass` ：占位的，啥也不干
- `：` 切片用，比如`[:-1]`就是从第一个到倒数第二个

###  赋值、引用、拷贝、作用域
#### 赋值 引用

![赋值][2]
![循环][3]

#### 拷贝

要得到 `[0, [0, 1, 2], 2]` 这个对象，你不能直接将` values[1]` 指向 `values` 引用的对象本身，而是需要吧 `[0, 1, 2]` 这个对象「复制」一遍，得到一个新对象，再将` values[1]` 指向这个复制后的对象。Python 里面复制对象的操作因对象类型而异，复制列表 `values `的操作是
```
values[:] #生成对象的拷贝或者是复制序列，不再是引用和共享变量，但此法只能顶层复制
```
所以你需要执行
```
values[1] = values[:]
```
Python 做的事情是，先 dereference 得到 values 所指向的对象` [0, 1, 2]`，然后执行 `[0, 1, 2][:] `复制操作得到一个新的对象，内容也是 [0, 1, 2]，然后将 `values` 所指向的列表对象的第二个元素指向这个复制二来的列表对象，最终 values 指向的对象是` [0, [0, 1, 2], 2]`。过程如图所示：
![此处输入图片的描述][4]
#### 复制嵌套元素
浅复制
![浅复制][5]
深复制
![此处输入图片的描述][6]

### 函数值传递
对于可变对象，对象的操作不会重建对象，而对于不可变对象，每一次操作就重建新的对象。
```
def func_int(a):
    print id(a) # output: 44787728 
    a += 4
    print id(a) # output: 44787632
    
t = 0
print id(t) # output: 44787728
func_int(t)
print t # output: 0

```

### 字符串

字符串匹配（）
[正则表达式][7]
字符串连接
Python: TypeError: cannot concatenate 'str' and 'int' objects

- str()

### 元组

不可变（只读）

### 列表 list

使用切片操作更新一部分元素
不定长append：固定size的numpy比灵活list存取快得多
zip转list才能正常sort

```
## 串联list
list_ = []
list_.extend(a_list)
```

### 字典

这里的defaultdict(function_factory)构建的是一个类似dictionary的对象，其中keys的值，自行确定赋值，但是values的类型，是function_factory的类实例，而且具有默认值。比如default(int)则创建一个类似dictionary对象，里面任何的values都是int的实例，而且就算是一个不存在的key, d[key] 也有一个默认值，这个默认值是int()的默认值0.
