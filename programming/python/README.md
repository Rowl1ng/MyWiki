
# Python魔法书

欢迎来到Python的魔法世界~撒花~

内容更新往往赶不上技术进化的速度，这本书更像是一册棵整理知识体系的航海图，请尽情地结合AI找到最新的解决方案。这里也列出一些学习资源：

- 鸢尾花系列-《编程不难》：[视频](https://space.bilibili.com/513194466/lists/2742325?type=season)+[代码]()
- [The Hitchhiker’s Guide to Python](https://docs.python-guide.org/)

如果有更新建议和资源推荐，欢迎联系：ling.rowling.luo@gmail.com

## Python魔法可以干什么

拿我自己做过的项目举例子：
- AI算法：检索三维模型，甚至和VR结合，Unity（C#语言）负责游戏环境，Python负责算法推理。
- 数据整理：用pandas库整理好几个Excel表格的数据，根据条件筛选再整合。

## 第一步：运行环境

首先准备魔杖——Python的运行环境。Anaconda。

```
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
conda config --set show_channel_urls yes
```
环境只是第一步，真正发挥作用的是无数的“技能库”，也就是numpy、pandas为代表的“包”，每个“包”背后都是大招集合。只有安装了对应的包才能使用其中的招数。

安装使用：conda install keras会自动安装其他依赖包

### 创建环境

一个环境往往没法应付所有情况，尤其是安装各种包带来的“版本地狱”：代码A要1.0版本的numpy，代码B要2.0版本的numpy。这种情况就只能分而治之：给小A、小B安排互不打扰的单间，也就是我们说的“虚拟环境”。

conda的env可以建多个python环境，在interpreter里把python路径配到envs目录下的python即可。
多个python环境：

```
conda create --name det3 python=3.5
conda info --envs

source activate det3
pip install -r requirements.txt
```
### virtualenv

Windows：



```shell
vitualenv env # create a virtualenv
\path\to\env\Scripts\activate # activate the created virtualenv, usually in \env\Scripts\activate
```
## 第二步：编程环境


主流方案：
- VS Code
- Pycharm

此外，强烈建议通过Jupyter Notebook实现交互更强的编程，搭配MarkDown语法，所写即所得。

## 好习惯

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



# Inbox

这里存放还没来得及整理的咒语。

- functools.partial()相当于对函数和传入的关键字参数进行“打包”，返回一个可调用对象
- python的map(func, seq)方法：对seq中的item依次执行func，结果也作为一个list返回
- python中文注释： # -*- coding: utf-8 -*