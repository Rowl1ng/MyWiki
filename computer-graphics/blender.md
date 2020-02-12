deform the vertices

```python
# get a bmesh
bm = bmesh.from_edit_mesh(mesh)
for v in bm.verts:
    if not v.select:
        continue
    v.co.xyz += Vector([uniform(-delta, delta) for axis in "xyz"])
#update.
bmesh.update_edit_mesh(mesh)
```