# 车道线识别

## 1. 任务的背景和意义
### 1.1 背景
 - 总体需求
对CiCS设备采集的路面图像中的道路标志标线进行识别及位置标记。
 - 路面图像
路面图像包括CiCS两种照明模式下的图片，需要对两种照明下的标线进行识别，尽可能统一一个接口调用执行，如一个算法有困难，可通过参数调用不同算法针对不同照明图像。
 - 标线识别结果标记
生成一个新的大小与输入图像一致的图像，在新图像中对识别出的标线像素用白色灰度（255）表示，对于其余位置像素用黑色表示（0）。
 - 识别要求
1）标线上的开裂裂缝算作标线的一部分，需要算作标线整体的一部分，不能分成小块识别出。
2）标线由于光照条件及自身颜色，在灰度上会有不同标线，要求能自适应各种灰度分布下的标线。
3）对于污损或磨损严重的标线，要求能够尽可能的将标线识别的完整。
 - 指标
对于正常的洁净的路面图像上无明显或轻微污损的标志标线，要求识别的完整性>=80%。
对于污损较严重的标线，要求对无污损的部分能完整识别（>=80%），污损的部分尽可能补全。
### 1.2 意义
本任务的成果将帮助公路维护部门审查路面维护状况：通过对路面图像的处理帮助发现标线的污损状况，以及时做出处理。
## 2. 主要问题 
 - 对不同亮度的图片能统一处理，且达到较高的准确率，以下是几种解决方案：

    * 检测图像白色（255）像素的比例，如果过高的话，调整相应阈值。或者，换句话是检测亮度；
	* 循环内判断，跳出循环的条件就是亮度/比例；
	* 对所有图片进行归一化处理，使得亮度在同一个范围内；
    * 增加图片对比度。
## 3. 完成方法
开发语言选择使用Python，借助OpenCV的Python包完成对图像的基础操作，并通过PyQt4编写界面，以提高程序的可操作性。
### 3.1 配置开发环境 
这里的使用的编程环境是Eclipse+Python2.7,但想要支持图像处理的各种功能还需要安装各种Python组件才行.

 - 安装numpy、scipy、PIL（Pillow）、matplotlib：
这些包用来支持OpenCV的功能（PIL是Python“嫡系”的图像处理包，可以不安装），matplotlib时很有用的python作图组件，比如我们在实时调节参数时要使用的trackbar就是它的功能之一。 
 - 安装openCV：	
    - 进入OpenCV的安装目录下找到：\build\python\2.7\cv2.pyd
    - 将cv2.pyd复制到Python的子目录下：\Lib\site-packages\
 - 安装pyQt4；
 - 安装pydev(Eclipse)：在Eclipse下写Python的必备插件。
 
----------
## 4. 任务步骤

### 4.1 基础操作
#### 1） 读入、输出jpg图片
```python
    for infile in  glob.glob('../photos/test/*.jpg'):
         out=processImage(infile)
         outfile=str(infile).split('\\')[1]
         outfile=outfile.split('.')[0]
         cv2.imwrite('../result/normlaneresult/'+outfile+'.jpg',out)
```
#### 2） **trackbar**实时调节参数
通过调节trackbar实时观察输出的图片结果更直观、有效，节省调参的时间。
创建trackbar：

```python 
    cv2.createTrackbar('thrs1', 'fill', 2000, 10000, nothing)
```
从trackbar获取参数：
```python
    while True 
        thrs1 = cv2.getTrackbarPos('thrs1', 'fill')
        cv2.imshow('fill', image)
        cv2.waitKey(0)
    cv2.destroyAllWindows()
```
效果见下图：
![filter.png-1143.7kB][1]
这里实际上是在调节双边滤波函数（**平滑化处理**）的一个参数对输出结果的影响（相当于模糊处理）。
#### 3）***** **Scharr**操作
想要明确区分图片中的标线区域和路面，首先想到的是边缘检测之后再进行颜色填充。边缘检测的效果如下：
![black.png-7.5kB][2]
边缘是指图像里那些梯度非常高的点。我们说的梯度是指像素亮度值变化。图像梯度是通过计算x和y方向上的梯度然后使用毕达哥拉斯定理(注:勾股定理)结合得到。虽然通常不需要该值，你可以分别通过y与x的比例的反正切来得到梯度角度值。
Scharr操作：x和y方向上的梯度可以分别用下面的核做卷积计算得到：
$$
\begin{array}{ccc}
        -3 &   0 &  3\\
        -10 & 0 & 10\\
        -3  &  0 &  3   \\   (\text{X 方向})
\end{array}
\begin{array}{ccc}
       -3 & -10  & -3\\
         0  &  0   &  0\\
        3   & 10   & 3    \\ (\text{Y 方向})
\end{array}
$$


任务中并**没用到**这个操作，因为尝试后发现：在标识线不清晰的情况下，Scharr操作只能突出密密麻麻的裂缝（这也就是后面选择使用**平滑化处理**的原因了），优点是能有效区别开没有污损的标识线（平滑）和路面上的白色印记（粗糙）。
具体操作代码：
```python
    gradX = cv2.Sobel(gray, ddepth = cv2.CV_32F, dx = 1, dy = 0, ksize = -1)
    gradY = cv2.Sobel(gray, ddepth = cv2.CV_32F, dx = 0, dy = 1, ksize = -1)
    gradient = cv2.subtract(gradX, gradY)
    gradient = cv2.convertScaleAbs(gradient)
```
### 4.2 平滑化处理
吸收前面使用Scharr操作尝试边缘检测的失败经验，改变思路，选择使用平滑化处理以尽可能消除路面上裂缝的影响。考虑到最终输出是二值化图片，这里主要参考了StackOverflow上对图像进行二值化处理的一种[解决方案][3]。即最终的解决思路是：

st=>start: 开始:>https://www.row1ng.com
io1=>inputoutput: 输入图片（灰度图）
op1=>operation: 双边滤波(模糊化)
op2=>operation: 增强对比度
op3=>operation: 全局阈值
io2=>inputoutput: 输出图片（黑白二值）
e=>end: 结束

st->io1->op1->op2->op3->io2->e

#### 1） 双边滤波

> 该滤波器可以在保证**边界清晰**的情况下有效的**去掉噪声**。它的构造比较复杂，即考虑了图像的**空间关系**，也考虑图像的**灰度关系**。双边滤波同时使用了空间高斯权重和灰度相似性高斯权重，确保了边界不会被模糊掉。



 `cv2.bilateralFilter(src, d, sigmaColor, sigmaSpace[, dst[, borderType]])`
 `cv2.bilateralFilter(image, 9, 90,16)`
cv2.bilateralFilter(img,d,’p1’,’p2’)函数有四个参数需要，d是领域的直径，后面两个参数是空间高斯函数标准差和灰度值相似性高斯函数标准差。
    `cv2.bilateralFilter(img, 9, 90,16)`
#### 2） 高斯滤波（*这个好像没有太大影响。。。*）
- 这里需要注意的是参数必须是奇数，因此在使用trackbar进行调节时需要用trackbar*2+1来作为参数值。
```python
    img = cv2.GaussianBlur(image,(7,7),0)
```
### 4.3 ***** 对比度
OpenCV的[Histograms][4]
- [ ] 用trackbar调节参数
```python
    img2 = cdf[img]
    res = np.hstack((img,equ)) #stacking images side-by-side
```
![Image.png-7.9kB][5]
用cdf增强对比度后发现路面上稍亮一些的印记也被强化了,并未达到理想效果，因此放弃。
### 4.4 阈值处理
任务进行到这里就到了收尾阶段，处理完的图片需要输出为二值化的图片，因此需要阈值处理操作。
> 一般使得图像的像素值更单一、图像更简单。阈值可以分为全局性质的阈值，也可以分为局部性质的阈值，可以是单阈值的也可以是多阈值的。

![Image1.png-210kB][6]
#### 1）自适应的阈值（右下角图片）

> 通过某种算法分别为不同的区域计算不同的阈值(自适应的阈值)，然后再根据每个区域的阈值具体地去处理每个区域


可以看到上述窗口大小使用的为11，当窗口越小的时候，得到的图像越细。想想一下，如果把窗口设置足够大以后（不能超过图像大小），那么得到的结果可能就和第二幅图像的相同了。<
这种方法理论上得到的效果更好，相当于在动态自适应的调整属于自己像素点的阈值，而不是整幅图像都用一个阈值。
 但经过实践发现并不适合车道标线这种强调轮廓，忽视细节的对象。

#### 2）全局性的阈值（右上角图片）
最终选择使用的是全局性阈值(参见花絮\[1\])。    
对在两种照明情况下拍摄的图片取了两个不同的全局性阈值（thresh）:
明亮环境（阈值147）：
![light.png-6.6kB][7]
昏暗环境（阈值93）：
![dark.png-17.5kB][8]
```python
    cv2.THRESH_BINARY # 黑白二值
    binImg = cv2.adaptiveThreshold(img, 1, cv2.ADAPTIVE_THRESH_MEAN_C, cv2.THRESH_BINARY, 55, -3)#cv2.bilateralFilter(binImg, 9, 90,16)
    #binImg = cv2.GaussianBlur(binImg, (3,3), 0)
    #ret, binImg = cv2.threshold(img, 35000, 1, cv2.THRESH_BINARY+cv2.THRESH_OTSU)
    plt.imshow(binImg, cmap = 'gray', interpolation = 'bicubic')
    plt.xticks([]), plt.yticks([])  # to hide tick values on X and Y axis
```
----------
###4.5 GUI部分（PyQt4）
[参考代码][9]
### 1）界面
![gui.png-6.8kB][10]
```python
    # -*- coding: utf-8 -*-
    
    import sys
    
    from PyQt4 import QtCore, QtGui 
    
    try:
        _fromUtf8 = QtCore.QString.fromUtf8
    except AttributeError:
        def _fromUtf8(s):
            return s
     
    try:
        _encoding = QtGui.QApplication.UnicodeUTF8
        def _translate(context, text, disambig):
            return QtGui.QApplication.translate(context, text, disambig, _encoding)
    except AttributeError:
        def _translate(context, text, disambig):
            return QtGui.QApplication.translate(context, text, disambig)
    
    class Ui_Dialog(object):
        def setupUi(self, Dialog):
            Dialog.setObjectName(_fromUtf8("Dialog"))
            Dialog.resize(400, 400)
            Dialog.setMinimumSize(QtCore.QSize(400, 400))
            Dialog.setMaximumSize(QtCore.QSize(400, 400))
           
            #GroupBox1
            self.groupBox = QtGui.QGroupBox(Dialog)
            self.groupBox.setGeometry(QtCore.QRect(10, 20, 381, 101))
            self.groupBox.setObjectName(_fromUtf8("groupBox"))
            self.label_2 = QtGui.QLabel(self.groupBox)
            self.label_2.setGeometry(QtCore.QRect(20, 62, 54, 12))
            self.label_2.setObjectName(_fromUtf8("label_2"))
            #Choose Input Path
            self.InputChoose = QtGui.QPushButton(self.groupBox)
            self.InputChoose.setGeometry(QtCore.QRect(320, 28, 40, 25))
            self.InputChoose.setFocusPolicy(QtCore.Qt.StrongFocus)
            self.InputChoose.setObjectName(_fromUtf8("InputChoose"))
            self.txtPath1 = QtGui.QLineEdit(self.groupBox)
            self.txtPath1.setGeometry(QtCore.QRect(70, 30, 241, 20))
            self.txtPath1.setFocusPolicy(QtCore.Qt.StrongFocus)
            self.txtPath1.setReadOnly(True)
            self.txtPath1.setObjectName(_fromUtf8("txtPath1"))
            #Choose Output Path
            self.OutputChoose = QtGui.QPushButton(self.groupBox)
            self.OutputChoose.setGeometry(QtCore.QRect(320, 58, 40, 25))
            self.OutputChoose.setFocusPolicy(QtCore.Qt.StrongFocus)
            self.OutputChoose.setObjectName(_fromUtf8("OutputChoose"))
            self.txtPath2 = QtGui.QLineEdit(self.groupBox)
            self.txtPath2.setGeometry(QtCore.QRect(70, 60, 241, 20))
            self.txtPath2.setFocusPolicy(QtCore.Qt.StrongFocus)
            self.txtPath2.setReadOnly(True)
            self.txtPath2.setObjectName(_fromUtf8("txtPath2"))
            
            #选择两种处理模式中的一个：
            self.btnLight = QtGui.QPushButton(Dialog)
            self.btnLight.setGeometry(QtCore.QRect(10, 280, 381, 51))
            self.btnLight.setObjectName(_fromUtf8("btnLight"))
            
            self.btnDark = QtGui.QPushButton(Dialog)
            self.btnDark.setGeometry(QtCore.QRect(10, 340, 381, 51))
            self.btnDark.setObjectName(_fromUtf8("btnDark"))
        
            self.retranslateUi(Dialog)
            QtCore.QMetaObject.connectSlotsByName(Dialog)
        
        def retranslateUi(self, Dialog):
            Dialog.setWindowTitle(_translate("Dialog", "车道标线识别程序 by Rowling", None))
            self.btnLight.setText(_translate("Dialog", "光亮模式", None))
            self.btnDark.setText(_translate("Dialog", "昏暗模式", None))
            self.groupBox.setTitle(_translate("Dialog", "请选择需要处理的图片文件夹以及输出图片的存放位置", None))
            self.label_2.setText(_translate("Dialog", "输出", None))
            self.InputChoose.setText(_translate("Dialog", "...", None))
            self.OutputChoose.setText(_translate("Dialog", "...", None))
        
       
           
    if __name__ == '__main__':
        app = QtGui.QApplication(sys.argv)
        Dialog = QtGui.QDialog()
        ui = Ui_Dialog()
        ui.setupUi(Dialog)
        Dialog.show()
        sys.exit(app.exec_())
        #应用程序的主事件循环，事件处理从这里开始
```
- exec是Python的关键字，因此，用 exec_() 来取代它。

#### 2） 自定义槽

```python
        # -*- coding: utf-8 -*-
 
import sys

from PyQt4 import QtGui
from PyQt4 import QtCore
 
import Fill
from GUI import Ui_Dialog
 
class ImageProcess(QtGui.QDialog, Ui_Dialog):
    def __init__(self, parent = None):
        QtGui.QDialog.__init__(self, parent)
        self.setupUi(self)
        self.exclude_list = []
    
    @QtCore.pyqtSignature("")
    def on_InputChoose_clicked(self):
        self.indir = QtGui.QFileDialog.getExistingDirectory(self,  u"选择目录","../photos/light_lane").replace("\\","/")
        self.txtPath1.setText(self.indir)
    
    @QtCore.pyqtSignature("")
    def on_OutputChoose_clicked(self):
        self.outdir = QtGui.QFileDialog.getExistingDirectory(self,  u"选择目录","../result/laneresult").replace("\\","/")
        self.txtPath2.setText(self.outdir)
 
   
    @QtCore.pyqtSignature("")
    def on_btnLight_clicked(self):
        Fill.processLightLane(str(self.indir),str(self.outdir))
        QtGui.QMessageBox.warning(self, '完成', '处理完毕，请到输出目录下查看')
    
     
    @QtCore.pyqtSignature("")
    def on_btnDark_clicked(self):
        Fill.processDarkLane(str(self.indir),str(self.outdir))
        QtGui.QMessageBox.warning(self, '完成', '处理完毕，请到输出目录下查看')
        
    def get_formated_path1(self):
        return self.format_path(self.txtPath1.text())
        
    def get_formated_path2(self):
        return self.format_path(self.txtPath2.text())
        
    def format_path(self, path
        return str(path).rstrip('\\') + '\\'
 
 
if __name__ == "__main__":
    reload(sys)
    app = QtGui.QApplication(sys.argv)
    dlg = ImageProcess()
    dlg.show()
    sys.exit(app.exec_())
 
```
### 4.6 打包成EXE
将Python程序打包成exe有好几种选择,
- py2exe 
- cx_Freeze
- pyinstaller
具体操作可以看这个[视频][11]。
这里选择的是较为方便的pyinstaller：
首先按Documentation的要求安装pyinstaller,第一步就是要安装pywin32,选择合适的版本即可（比如我的Python是2.7，系统是64位）。
安装完后，直接用powershell(管理员模式下打开)，运行`pip install PyInstaller`。
之后切到要打包的python程序的文件夹（参见花絮\[2\]）运行`pyinstaller XXX.py`即可，目标exe保存在dist目录下。
命令行参数补充：

>  - -c, --console, --nowindowed  	Open a console window for standard i/o (default)
>  - -w, --windowed, --noconsole  	Windows and Mac OS X: **do not** provide a **console window** for standard i/o. On Mac OS X this also triggers
> building an OS X .app bundle.This option is ignored in *NIX systems.
>  - -F, --onefile	Create a one-file bundled executable.
>  - -D, --onedir	Create a one-folder bundle containing an executable (default)

所以最后的执行语句是： 
   ` pyinstaller -F -D --noconsole -n Lane G:\C\Lane\Lane\file_ui.py`


----------

## 5. 参考资料
清华大学出版社的《图像处理与计算机视觉算法及应用(第0版) 》TP391.41/P121.

*图书馆还有一本但就是找不到。。。*

      [1]: http://stackoverflow.com/questions/26145806/filling-contours-in-opencv
      [2]: http://opencv-python-tutroals.readthedocs.org/en/latest/py_tutorials/py_imgproc/py_histograms/py_histogram_equalization/py_histogram_equalization.html
      [3]: http://www.pythoner.com/176.html
      [4]: https://www.youtube.com/watch?v=11Q2QADsAEE

## 6.	任务完成体会
从零开始学习OpenCV图像处理，并且使用刚上手的Python,对自己而言无疑是一个巨大的挑战。好在过程还算顺利，虽然会不停地遇到问题，但经过不断尝试总能或多或少地缓解“燃眉之急”。就目前来看，解决方案还有很大提升空间，譬如自动化识别，要解决这个或者得彻底改变思路（譬如充分模糊化之后再做边缘检测），即对不同亮度的图片能达到同样高的正确率。
在请教别人后会有人建议去看Photoshop怎么做，忽然意识到成熟的商业软件背后蕴藏着许许多多人智慧的结晶，而自己还有很多很多需要理解掌握的东西。
### 花絮 
> - \[1\] 这次用的是全局性阈值，自适应阈值能平衡细节，但对有裂缝的车道来说并不适用。因为最后的结果强调标志线与路面的黑白对比，所以只是通过一个全局的阈值“筛出”白色标志线。不过这时遇到一个问题：对于光照条件不同的路面照片，阈值是不同的,甚至在一种光照条件（暗光），路面照片也不一样，这个时候开始考虑归一化和增强对比度的问题。
- \[2\] 真傻还是假傻。。。。在powershell里执行命令，默认都是在system32文件夹里啊。。。。得切到工程文件夹里才好啊啊啊啊


  [1]: http://static.zybuluo.com/sixijinling/kw38t2wd0qhj5u5a34o32v9u/filter.png
  [2]: http://static.zybuluo.com/sixijinling/sktq295xvyxkgv92gbjm0ypi/black.png
  [3]: http://stackoverflow.com/questions/26145806/filling-contours-in-opencv
  [4]: http://opencv-python-tutroals.readthedocs.org/en/latest/py_tutorials/py_imgproc/py_histograms/py_histogram_equalization/py_histogram_equalization.html
  [5]: http://static.zybuluo.com/sixijinling/b5ze3we16g5z2t61qf6k5uwd/Image.png
  [6]: http://static.zybuluo.com/sixijinling/8dy1l0qe9n3y7tjgukglu51t/Image1.png
  [7]: http://static.zybuluo.com/sixijinling/3eum3iqvfyaygtr0dayhr9lg/light.png
  [8]: http://static.zybuluo.com/sixijinling/9ljvyh7kv2vbu5opcrkjueue/dark.png
  [9]: http://www.pythoner.com/176.html
  [10]: http://static.zybuluo.com/sixijinling/fq1y0onoen7c8hr6x2n0yzed/gui.png
  [11]: https://www.youtube.com/watch?v=11Q2QADsAEE