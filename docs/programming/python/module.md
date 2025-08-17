## 1.模块 Module

内置类属性：

    __dict__ : 类的属性（包含一个字典，由类的数据属性组成）
    __doc__ :类的文档字符串
    __name__: 类名
    __module__: 类定义所在的模块（类的全名是'__main__.className'，如果类位于一个导入模块mymod中，那么className.__module__ 等于 mymod）
    __bases__ : 类的所有父类构成元素（包含了以个由所有父类组成的元组）


### 1.1`__name__`

模块是对象，并且所有的模块都有一个内置属性 `__name__`。一个模块的  `__name__` 的值取决于您如何应用模块。如果 import 一个模块，那么模块 `__name__` 的值通常为模块文件名，不带路径或者文件扩展名。但是您也可以像一个标准的程序样直接运行模块，在这 种情况下,  `__name__` 的值将是一个特别缺省 `"__name__“`。

```python
    if __name__ == '__main__':
        main()
```
### 1.2  `__init__.py`

像Java的Package一样，将多个.py文件组织起来，以便在外部统一调用，和在内部互相调用呢？答案是有的。
python在执行import语句时，到底进行了什么操作，按照python的文档，它执行了如下操作：
第1步，创建一个新的，空的module对象（它可能包含多个module）；
第2步，把这个module对象插入sys.module中
第3步，装载module的代码（如果需要，首先必须编译）
第4步，执行新的module中对应的代码。
Python中的package定义很简单，其层次结构与程序所在目录的层次结构相同，这一点与Java类似，唯一不同的地方在于，python中的package必须包含一个`__init__.py`的文件。

    package1/
        __init__.py
        subPack1/
            __init__.py
            module_11.py
            module_12.py
            module_13.py
        subPack2/
            __init__.py
            module_21.py
            module_22.py
        ……

### 1.3 名称空间（Namespaces）

- **名称空间**：名字和对象间的映射
- **作用域**(Scope)：哪些物理位置可以访问这些名字

加载顺序：
内建名称空间（`__builtins__`模块）->全局名称空间->局部
查找顺序相反。

#### globals() locals()

### python package

当你import的时候，python只会在sys.path这个变量（一个list，你可以print出来看）里面的路径中找可能匹配的package和module。


  
而一个package跟一个普通文件夹的区别在于，package的文件夹中多了一个`__init__.py`文件。换句话说，如果你在某个文件夹中添加了一个`__init__.py`文件，则python就认为这个文件夹是一个package。

`__init__.py`文件可以是空的（也推荐这么做），它只是告诉python当前文件夹是一个package。当然，也可以在里面添加一些代码，这些代码会在import这个包的时候运行。

- 专门写一个`_init_paths.py`来初始化模块调用路径：尝试_init_path.py的方式增加python包的路径到PYTHONPATH中
```python
import os.path as osp
import sys

def add_path(path):
    if path not in sys.path:
        sys.path.insert(0, path)

this_dir = osp.dirname(__file__)

# Add lib to PYTHONPATH
lib_path = osp.join(this_dir, 'lib')
add_path(lib_path)

coco_path = osp.join(this_dir, 'data', 'coco', 'PythonAPI')
add_path(coco_path)
```


# Inbox

    * 根据类名调用方法