## Anaconda

```
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
conda config --set show_channel_urls yes
```

安装使用：conda install keras会自动安装其他依赖包

多个python环境：

```
conda create --name det3 python=3.5
conda info --envs

source activate det3
pip install -r requirements.txt
```

