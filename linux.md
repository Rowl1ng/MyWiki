# 内存管理

查看磁盘空间：df -hl
查看当前文件夹大小：du -sh
按大小排序：
```shell
du --max-depth=1 --human-readable / | sort --human-numeric-sort
```