
# Python魔法书

欢迎来到Python的魔法世界~撒花~

内容更新往往赶不上技术进化的速度，这本书更像是一册棵整理知识体系的航海图，请尽情地结合AI找到最新的解决方案。这里也列出一些学习资源：

- 鸢尾花系列-《编程不难》：[视频](https://space.bilibili.com/513194466/lists/2742325?type=season)+[代码]()
- [莫烦Python](https://mofanpy.com/)
- 《python数据处理》，《Python核心编程》OOP部分
- [The Hitchhiker’s Guide to Python](https://docs.python-guide.org/)
- [Automate the Boring Stuff with Python](https://automatetheboringstuff.com/)
- [Hands-on Machine Learning with Scikit-Learn, Keras and TensorFlow (3rd edition)](https://github.com/ageron/handson-ml3)

如果有更新建议和资源推荐，欢迎联系：ling.rowling.luo@gmail.com

## Python魔法可以干什么

拿我自己做过的项目举例子：
- AI算法：Pytorch、TensorFlow
  - [电信诈骗识别](https://rowl1ng.com/blog/tech/phoneme.html)
  - [智能客服/微信chatbot](https://rowl1ng.com/blog/tech/chatbot.html)：从训练语言模型到和微信公众号的绑定，基于sanic的python web端作为公众号后台 
  - 路面裂缝识别
  - 医疗影像分析
  - 检索三维模型，甚至和VR结合，Unity和Python之间的通信：Unity（C#语言）负责游戏环境，Python负责算法推理。
- 利用专业包和其他领域的交叉：
  - 文本处理：
    - python的json库读取jquery
    - 读取context、question、answer并进行分词
  - 图像处理：OpenCV，image augmentation：python包imgaug
  - 音频分析：Dejavu那个是landmark的方法，对固定音频有效。
  - 图形学处理：
    - 用C++对三维模型进行处理：
      - C++ extension for Python
      - libigl 
    - 操作三维建模软件Blender：
      - depth render with Python script： blender xxx.blend xxx.py
      - Blender Python教程：Scripting for Artists
- 可视化：
  - 用python+matplotlib做可视化的动画，这样即便是复杂的数学概念也可以深入浅出地展示出来。
  - matplotlib画3D point cloud+制作gif。安装PyGEL3D用来可视化3D mesh。plotly可视化3d line。
- 数据分析：jupyter notebook+pandas，用pandas库整理好几个Excel表格的数据，根据条件筛选再整合。
  - 专业评估材料：写python整理学生荣誉表格，遇到好几个坑：python版本老了pandas读不了子表，“XX学院”的学生获奖，后来进入计算机系出现在学生名单里，得按学院先过滤获奖名单。

## 关键概念

- 解释器 Interpreter

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

一个环境往往没法应付所有情况，面临“版本地狱”：代码A要2.7版本的python，代码B要3.7版本的python。这种情况就只能分而治之：给小A、小B安排互不打扰的单间，也就是我们说的“虚拟环境”。

花絮：python3和python2的`/`除法不一样，3是浮点数，2是整数。

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

conda的env可以建多个python环境，在interpreter里把python路径配到envs目录下的python即可

此外，强烈建议通过Jupyter Notebook实现交互更强的编程，搭配MarkDown语法，所写即所得（这个文档也是用MarkDown写的）。

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
- 有用的python代码段写在笔记里，方便搜索。



# Inbox

这里存放还没来得及整理的咒语。

- functools.partial()相当于对函数和传入的关键字参数进行“打包”，返回一个可调用对象
- python的map(func, seq)方法：对seq中的item依次执行func，结果也作为一个list返回
- python中文注释： # -*- coding: utf-8 -*
- Python：用.env来存全局环境变量