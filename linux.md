查看显卡情况：nvidia-smi

vim的替换操作：:%s#A#B#(把所有行的A替换成B)

# 执行

同时执行多个命令，使用`&`
# IO

## 输入
比如`sh test_sample.sh Dec10-13-51-44_cac8_step`。

参数可通过`$1`的方式读取到，`$0`是脚本的名字。

```
model_name=$1
echo "Model: $model_name"
```

## 输出
输出到文件 
```
sh XXX.sh > XXX.txt
```
输出到文件的同时打印到terminal：
```
sh XXX.sh | tee XXX.txt
```

# 文件

## 查看文件大小：

```
sudo du -sh * | sort -n
```
查看磁盘空间：df -hl
查看当前文件夹大小：du -sh
按大小排序：

```shell
du --max-depth=1 --human-readable / | sort --human-numeric-sort
```

## 获取最新文件（夹）

```
#!/bin/sh
filename=`ls -t |head -n1|awk '{print $0}'`
echo $filename
```
获取最新文件夹
```
ls mydir -l |tail -n 1| awk '{print $9}'
```

## 解压

tar   zxvf    test.tgz  -C  指定目录

tar.xz文件：
```
$xz -d ***.tar.xz
$tar -xvf  ***.tar
```
## 传输

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
## 解压
linux下解压命令大全

.tar 
解包：tar xvf FileName.tar
打包：tar cvf FileName.tar DirName
（注：tar是打包，不是压缩！）
———————————————
.gz
解压1：gunzip FileName.gz
解压2：gzip -d FileName.gz
压缩：gzip FileName
.tar.gz 和 .tgz
解压：tar zxvf FileName.tar.gz
压缩：tar zcvf FileName.tar.gz DirName
———————————————
.bz2
解压1：bzip2 -d FileName.bz2
解压2：bunzip2 FileName.bz2
压缩： bzip2 -z FileName
.tar.bz2
解压：tar jxvf FileName.tar.bz2
压缩：tar jcvf FileName.tar.bz2 DirName

# 时间

```
date
```
# 其他

nohup:
* 回车，回到命令行

# Reference

- [速查工具tldr](https://tldr.ostera.io/scp)
