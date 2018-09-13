## 移动文件

[Python 文件夹及文件操作](https://www.cnblogs.com/feeland/p/4463682.html)

```python
import shutil

if not os.path.exists(target_path):
    os.makedirs(target_path)
name = target_path + '/{}'.format(view)
print name
shutil.copyfile(dicom_path, name)
```
复制
```python
# 复制目录A_dir下的所有文件到B_dir，B_dir必须不存在
shutil.copytree(A_dir, B_dir)
```

# 文件

- 删除文件:  os.remove()

- 文件重命名：

```
import os
path = 'D:\\图片\\'
for file in os.listdir(path):
    if os.path.isfile(os.path.join(path,file))==True:
        if file.find('.')<0:
            newname=file+'rsfdjndk.jpg'
            os.rename(os.path.join(path,file),os.path.join(path,newname))
            print file,'ok'
#        print file.split('.')[-1]
```

- 新建文件夹：os.makedirs(ret_dir)

用变量命名：
ouput=open(str1+".txt",'w')

```python
#write(string)
f = open("readwrite.txt",'w')
f.write("Welcome to this file\nThere is nothing here except\nThis stupid haiku\n")
f.close()

#writelines(list)
f = open("readwrite.txt")#默认为读
lines = f.readlines()
f.close()

lines[0] = "#-----writelines(list)-------#\nWelcome to this file\n"
f = open("readwrite.txt",'a')#a模式在原文件上继续添加
f.writelines(lines)
f.close()

#read(n)读取前n个字符
f = open("readwrite.txt","r")
print "#-------------------------read(n)----------------------------#"
print f.read(4)
f.close()

#readline()按行读取
f = open("readwrite.txt")
print "#-------------------------readline()----------------------------#"
for i in range(3):
    print str(i) + ": " + f.readline() #str(i)将object转为string
f.close()

#readlines()返回列表，每项为一行
import pprint
f = open("readwrite.txt")
l = f.readlines()#返回列表
print l#输出列表
print "---------------------------------------"
pprint.pprint(l)#每项为一行显示，而且是文件对象自动关闭的方法，所以无f.close()
```

## json

```python
def read_json(json_file):
    f = open(json_file, 'r')
    data = json.load(f)
    f.close()
    return data
```
