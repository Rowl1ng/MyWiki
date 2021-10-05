
# Tools

- [Pytorch3D](https://pytorch3d.readthedocs.io/en/latest/overview.html#installation)
- [geometryprocessing](https://geometryprocessing.github.io/)
- [mehsplot](https://skoch9.github.io/meshplot/tutorial/#scalar-field-visualization)

```python
import meshplot as mp

d = mp.subplot(v, c=v[:, 1], s=[1, 2, 0], shading={"point_size": 0.03})
mp.subplot(v, c=np.random.rand(*v.shape), s=[1, 2, 1], data=d, shading={"point_size": 0.03})
```
