  * lung nodule detection：[天池比赛](https://rowl1ng.com/blog/tech/luna.html)
  
用到的包：

```
import SimpleITK as sitk
import dicom
```

## 读dcm图像

```
def read_dcm(fname):
    image = sitk.ReadImage(fname)
    image_array = sitk.GetArrayFromImage(image)
    image_array = np.squeeze(image_array)
    return image_array
```

## 读dicom信息

```
def get_info(path):
    ds=dicom.read_file(path)
    pid = ds.PatientID.split('/')[0]
    seriesid = ds.SeriesInstanceUID 
    studyid = ds.StudyInstanceUID
    return year, pid, seriesid, studyid
```

## 调节窗宽窗位

通常dcm文件的值是16bit，也就是在$$[0,2^{16}-1]$$区间，转成png等图片格式的时候要映射到$$[0,255]$$，不可避免地要损失信息，而窗宽窗位则是重要信息的“过滤网”。

```
# wc：window center，可视作“原点”，用来确定“可见部分”的上下边界，比如以window center为中心，[-window width/2,window width/2]的范围即被映射到[0,255]
# ww：window width，最终被放在0~255区间内的“可见部分”
def adjust_wc_ww(dicom_path, wc, ww, vis=False):
    win_up = wc + ww * 7 / 8
    win_down = wc - ww / 8
    img_u16 = read_dcm(dicom_path)
    img_u16[img_u16 > win_up] = win_up
    img_u16[img_u16 < win_down] = win_down
    ww = win_up - win_down
    img_u16 -= win_down
    img_8u = np.array(img_u16 * 255.0 / ww, dtype='uint8')
    if vis:
        fig = plt.figure(figsize=(10,10))
        plt.imshow(img_8u, cmap='bone')
    return img_8u
```
## 对齐左右侧

```python
%env CUDA_DEVICE_ORDER=PCI_BUS_ID
%env CUDA_VISIBLE_DEVICES=2
import os
import SimpleITK as sitk
import cv2
import numpy as np
from skimage.filters.rank import entropy, bottomhat, tophat
from skimage.morphology import disk
from skimage.filters import threshold_li


dcm_path = "/data2/mammogram/by3_b1/dicom/0079148/4889425/1.2.840.113681.2216073793.2474.3665280246.33.1/R_CC_2.dcm"
os.path.exists(dcm_path)
img_itk = sitk.ReadImage(dcm_path)
img_np = sitk.GetArrayFromImage(img_itk).squeeze()
h, w = np.array(img_np.shape)//10
img2 = cv2.resize(img_np, (w, h))
thresh1 = threshold_li(img2)
img_thresh1 = img2> thresh1
out = entropy(img2, disk(5))
thresh2 = threshold_li(out)
img_thresh2 = out> thresh2
```