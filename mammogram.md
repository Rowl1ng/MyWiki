# 钼钯

```python
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