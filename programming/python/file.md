
# 创建


- 新建文件夹：os.makedirs(ret_dir)
```python
def mkdir_safe(d):
    sub_dirs = d.split('/')
    cur_dir = ''
    max_check_time = 5
    sleep_seconds_per_check = 0.001
    for i in range(len(sub_dirs)):
        cur_dir += sub_dirs[i] + '/'
        for check_iter in range(max_check_time):
            if not os.path.exists(cur_dir):
                try:
                    os.mkdir(cur_dir)
                except Exception as e:
                    print('[WARNING] ', str(e))
                    time.sleep(sleep_seconds_per_check)
                    continue
            else:
                break
```

# 移动
```
# 文件→文件夹
shutil.copy(file_A, dir_B)

# 复制目录A_dir下的所有文件到B_dir，B_dir必须不存在
shutil.copytree(A_dir, B_dir)
```

参考：[Python 文件夹及文件操作](https://www.cnblogs.com/feeland/p/4463682.html)


```python
import shutil

if not os.path.exists(target_path):
    os.makedirs(target_path)

name = target_path + '/{}'.format(view)
shutil.copyfile(dicom_path, name)
```

# 删除


```python
## 删除文件:  
os.remove()
## 删除文件夹
imprt shutil
shutil.rmtree()
```

# 重命名

```python
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

用变量命名：
ouput=open(str1+".txt",'w')

# 读写

写入list到txt中：
```python
import codecs
def save_string_list(file_path, l, is_utf8=False):
    if is_utf8:
        f = codecs.open(file_path, 'w', 'utf-8')
    else:
        f = open(file_path, 'w')
    for item in l[:-1]:
        f.write(item + '\n')
    if len(l) >= 1:
        f.write(l[-1])
    f.close()
```
   
小trick：用read读txt的时候，去掉换行符：line.strip(‘\n’)

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
# 常用文件类型
## json

读入
```python
import json
def read_json(json_file):
    f = open(json_file, 'r')
    data = json.load(f)
    f.close()
    return data
```
写文件
```python
with open(os.path.join(save_dir, a_index+'.txt'), 'w') as outfile:
    json.dump(data, outfile)
```

## pickle
```python
import pickle as pkl
with open(result, 'rb') as f:
    data = pkl.load(f)
```

