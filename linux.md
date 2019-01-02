# 时间

```
date
```

# 文件
获取最新文件

```
#!/bin/sh
filename=`ls -t |head -n1|awk '{print $0}'`
echo $filename
```
# 内存管理

查看磁盘空间：df -hl
查看当前文件夹大小：du -sh
按大小排序：

```shell
du --max-depth=1 --human-readable / | sort --human-numeric-sort
```

# 解压

tar   zxvf    test.tgz  -C  指定目录

tar.xz文件：
```
$xz -d ***.tar.xz
$tar -xvf  ***.tar
```

 nohup:
* 回车，回到命令行

复制：cp
    * 目录：-ae
    * 目标目录存在时是把源目录拷过去
    * 目标目录不存在时是拷文件过去
查看文件大小：
* sudo du -sh * | sort -n

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