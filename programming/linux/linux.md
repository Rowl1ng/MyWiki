# Linux 工具

查看显卡情况：nvidia-smi

vim的替换操作：:%s#A#B#(把所有行的A替换成B)

## 执行

同时执行多个命令，使用`&`

## IO

### 输入
比如`sh test_sample.sh Dec10-13-51-44_cac8_step`。

参数可通过`$1`的方式读取到，`$0`是脚本的名字。

```
model_name=$1
echo "Model: $model_name"
```

### 输出
输出到文件 
```
sh XXX.sh > XXX.txt
```
输出到文件的同时打印到terminal：
```
sh XXX.sh | tee XXX.txt
```

## 文件

### Move & Rename

```
mv (option) filename1.ext filename2.ext
```

### 查看文件大小

```
sudo du -sh * | sort -n
```
查看磁盘空间：df -hl
查看当前文件夹大小：du -sh
按大小排序：

```shell
du --max-depth=1 --human-readable / | sort --human-numeric-sort
```
  * trashbin没法清空（可能是空间塞满了）：直接查看文件大小发现/.Trash-290037最大，直接删掉文件夹就好
  
### 获取最新文件（夹）

```
#!/bin/sh
filename=`ls -t |head -n1|awk '{print $0}'`
echo $filename
```
获取最新文件夹
```
ls mydir -l |tail -n 1| awk '{print $9}'
```

### 传输

复制：cp
    * 目录：-ae
    * 目标目录存在时是把源目录拷过去
    * 目标目录不存在时是拷文件过去

1、mac上传文件到Linux服务器

scp 文件名 用户名@服务器ip:目标路径
如：scp /Users/test/testFile test@www.linuxidc.com:/test/
2、mac上传文件夹到Linux服务器，与上传文件相比多加了-r

scp -r 文件夹目录 用户名@服务器ip:目标路径
如：scp -r /Users/test/testFolder test@www.linuxidc.com:/test/
pscp 命令 上传

3、Linux服务器下载文件到mac

scp 用户名@服务器ip:文件路径 目标路径
如：scp test@www.linuxidc.com:/test/testFile /Users/test/

4、Linux服务器下载文件夹到mac，与下载文件相比多加了-r

scp -r 用户名@服务器ip:文件路径 目标路径
如：scp -r test@www.linuxidc.com:
/test/testFolder /Users/test/

从ftp服务器下载
```
wget -m -t0 http://XXX
```
### 解压

.tar ：

- 解包：tar xvf FileName.tar
- 打包：tar cvf FileName.tar DirName
（注：tar是打包，不是压缩！）

.gz：

- 解压1：gunzip FileName.gz
- 解压2：gzip -d FileName.gz
- 压缩：gzip FileName

.tar.gz 和 .tgz

- 解压：tar zxvf FileName.tar.gz
- 压缩：tar zcvf FileName.tar.gz DirName

tar.xz文件：
```
$xz -d ***.tar.xz
$tar -xvf ***.tar
```
## 时间

```
date
```
## 其他

nohup:

* 回车，回到命令行

## Reference

- [速查工具tldr](https://tldr.ostera.io/scp)

# Tmux

启动新会话：

```
tmux [new -s 会话名 -n 窗口名]
```
列出所有会话：

```
tmux ls
```

恢复会话：

```
tmux at [-t 会话名]
```

关闭会话：

```
tmux kill-session -t 会话名
```

以下操作需先按下ctrl+b：

- 新建新窗口：c
- 切换窗口：前p 后n
- 关闭窗口：&

窗格

- 纵向分割：%

```
c  创建新窗口
w  列出所有窗口
n  后一个窗口
p  前一个窗口
f  查找窗口
,  重命名当前窗口
&  关闭当前窗口
%  垂直分割
"  水平分割
o  交换窗格
x  关闭窗格
⍽  左边这个符号代表空格键 - 切换布局
q 显示每个窗格是第几个，当数字出现的时候按数字几就选中第几个窗格
{ 与上一个窗格交换位置
} 与下一个窗格交换位置
z 切换窗格最大化/最小化
```

# git

遇到 server certificate verification failed. CAfile: /etc/ssl/certs/ca-certificates.crt CRLfile: none

```
git config --global http.sslverify false
```
git 退出nano界面：Ctrl + X然后输入y

## clone

查看远程仓库地址

```
git remote -v
```

clone branch

```
git clone -b breast https://.....
```

Git下放另一个git，在clone的时候：

```
git clone --recursive
```

## push更新


```
git pull origin breast
git add .

# remove git history
git rm -r —cached ./

git commit -m 'message'
git push origin breast
```
## pull

```
git pull
```
如果更新不能merge，需要stash
```
git stash
```

## .gitignore

[A collection of useful .gitignore templates](https://github.com/github/gitignore)

## diff

## reset
```
git log
git reset --hard XXXXXXX
```

```
#查看远程分支
git branch -a
#查看当前分支所属
git branch -vv
git push origin main

# 将远程主机origin的master分支拉取过来，与本地的branchtest分支合并。
git pull origin master:branchtest
```

