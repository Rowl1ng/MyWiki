# 钼钯
- 乳腺超声：高频探头（清晰度高，深度小）；跟年龄、生理周期有关；纵横比大于一很可能恶性（压迫胸大肌）
- 工程化：胸肌线分割+乳腺区域分割；
- 讲座：birads分级是不同类别，不是渐进关系

* mammogram：
  * DDSM数据：
      * 处理数据、做标注一定要check之后再跑实验！！
      * patch sampling
  * calcification：
      * faster rcnn：复现论文在INBreast上的效果：在ddsm上训练;和作者通信
          * VGG16降低下采样到16倍、8倍
      * 调inbreast的窗宽窗位：“发明”了“自适应”调节方法
      * huber loss
      * hard example mining
  * mass:
      * local contrast normalization的pytorch实现
      * evaluation要统一计算标准
      * mask rcnn：
          * multi_view：R和L的对齐;check不同数据集的对齐效果
          * ignore四种类型
          * rcnn、rpn加focal loss
          * data augmentation：gamma correction
      * 迭代新模型:
          * 加入新数据训练
          * 做错误分析：统计漏检的大小分布，腺体分型；误检、漏检统计，分数最低
  * 产品化：做控制单一变量的对比实验！！！
  
## 数据格式

```json
{ # 一个pid对应的四张片子为一组，保存再一个json中
'patientID': pid
'studyUID': studyid
'seriesUID' : seriesid
'json_format_version':2.0
'quality' : 0
'pixel_spacing' : np.array([0.085, 0.085]).tolist() # 从dcm中提取得到
'task': task_name
'other_info': {}
'nodes': {
        "node_index": "1", # 病灶序号，钙化和肿块依次标号
        "score": 0.0, # 检出的打分
        "note": "",
        "from": 0b100,
        "type": "Breast_Calcification", # 病灶类型："Breast_Calcification"（钙化）或者 "Breast_Lump"（肿块）
        "attr": {},
        "bounds": [ # 存储检出的bbox：钙化和肿块都有
            {
                "slice_index": 0, # instance id
                "edge": [] # bbox的四个点的座标（x, y）
            }
        ],
        "rois": [ # 存储检出的分割segms：只有肿块有
            {
                "slice_index": 0, # instance id
                "edge": [] #分割contour上所有点的座标 (x, y)
            }
        ],
    }
}
```
保存 
```python
import copy
def __make_json_result(series_list, pid, studyid, seriesid, mass_list, task_name='BreastMGDetect'):
    node = {
        "node_index": "1", 
        "score": 0.0,
        "note": "",
        "from": 0b100,
        "type": "Breast_Calcification", 
        "attr": {},
        "bounds": [
            {
                "slice_index": 0,
                "edge": []
            }
        ],
        "rois": [
            {
                "slice_index": 0,
                "edge": []
            }
        ],
    }

    ret = {}
    ret['patientID'] = pid
    ret['studyUID'] = studyid
    ret['seriesUID'] = seriesid
    ret['json_format_version'] = 2.0
    ret['quality'] = 0
    ret['pixel_spacing'] = np.array([0.085, 0.085]).tolist()
    ret['task'] = task_name
    ret['other_info'] = {}
    roi_list = []
    for series, masses in zip(series_list, mass_list): 
        bboxes = masses['bboxes']
        segms = masses['segms']
        calc_num = 0
        for i, item in enumerate(bboxes):
            x1, y1, x2, y2 = item[:4]
            score = item[-1]
            points = np.int32([[x1, y1], [x2, y1], [x2, y2], [x1, y2]]).tolist()
            nodule = copy.deepcopy(node)
            nodule['type'] = "Breast_Lump"
            nodule['node_index'] = str(i + 1 + calc_num)
            nodule['score'] = str(score)
            edges = np.int32(segms[i]).tolist()
            nodule['rois'][0]['edge'] = [[y, x] for (x, y) in edges]
            nodule['rois'][0]['slice_index'] = series
            nodule['bounds'][0]['edge'] = points
            nodule['bounds'][0]['slice_index'] = series
            roi_list.append(nodule)

    ret['nodes'] = roi_list
    return json.dumps(ret), "OK"
```

# 参考资料

      * 《乳腺比较 影像诊断学》by刘万花、《医学影像诊断学》第三版、《乳腺疾病诊治》第三版