# Visualization 

## Terms

- camera：
  - camera distance
  - __elevation angle__ 抬高角度
  - __azimuth angle__ 旋转角度
  - camera 类型
    - orthogonal 正交，没有近大远小的透视效果
    - perspective 立体透视

## Webpage

```yaml
{
        "title": "source",
        "visuals": {
            "pts": "source_pointcloud.pts",
            "kpts": "source_keypoints.txt"
        }
    },
    {
        "title": "target",
        "visuals": {
            "pts": "target_pointcloud.pts",
            "kpts": "target_keypoints.txt"
        },
    {
        "title": "deformed_cage",
        "visuals": {
            "ply": "deformed_cage.ply"
        }
    }
}
```

```shell
python browse3d/browse3d.py --log_dir $DATA_PATH --port 5050
```

## Packages

### meshplot

```python
import meshplot as mp
# visualize pointcloud
mp.plot(point_set)
```
### PyGEL3D

```python
from PyGEL3D import gel, js
m = gel.obj.load(model_path)
js.display(m, smooth=False)
```

## Rendering

- [Point Cloud Renderer](https://github.com/zekunhao1995/PointFlowRenderer)
  - ![example](https://github.com/zekunhao1995/PointFlowRenderer/raw/master/mitsuba_scene.png)
