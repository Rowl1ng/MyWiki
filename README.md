* 字符串分割是split，分成字符是strip
 大乌龙：filepath.replace(‘lung’,’nodule’)想的是只替换文件名，结果前面的路径里也出现了’lung’，就找不到文件了
* numpy.zeros((size),dtype=XXX)
* numpy乘法，
* functools.partial()相当于对函数和传入的关键字参数进行“打包”，返回一个可调用对象

## Anaconda

```
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
conda config --set show_channel_urls yes
```

安装使用：conda install keras会自动安装其他依赖包

conda的env可以建多个python环境，在interpreter里把python路径配到envs目录下的python即可。
多个python环境：

```
conda create --name det3 python=3.5
conda info --envs

source activate det3
pip install -r requirements.txt
```
代码规范：Readable、reusable、robust；
- 代码做好单元测试、变量命名 减少混淆；
- 不要用from mypkg import *;
- 利用中间变量控制行的长度，使用'\’换行； 
- with open(‘file.txt) as f: for ling in f: print line使用context manager减少资源消耗；
- 函数定义input和output，参数类型、内容；
- avoid giant function，each function should do one thing and do it well；
- avoid duplicate code；
- unittest，有明确输入输出的；
- 开源代码质量往往很高，学着写
