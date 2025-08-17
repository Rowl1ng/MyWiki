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

- [mehsplot](https://skoch9.github.io/meshplot/tutorial/#scalar-field-visualization)

```python
import meshplot as mp

d = mp.subplot(v, c=v[:, 1], s=[1, 2, 0], shading={"point_size": 0.03})
mp.subplot(v, c=np.random.rand(*v.shape), s=[1, 2, 1], data=d, shading={"point_size": 0.03})
```

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

  * visualization using Pytorch3d’s renderer
- [Point Cloud Renderer](https://github.com/zekunhao1995/PointFlowRenderer)
  - ![example](https://github.com/zekunhao1995/PointFlowRenderer/raw/master/mitsuba_scene.png)
