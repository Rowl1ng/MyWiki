# Blender

deform the vertices

```python
mesh = imported.data

if mesh.is_editmode:
    bm = bmesh.from_edit_mesh(mesh)
else:
    bm = bmesh.new()
    bm.from_mesh(mesh)

# get a bmesh
bm = bmesh.from_edit_mesh(mesh)
for v in bm.verts:
    if not v.select:
        continue
    v.co.xyz += Vector([uniform(-delta, delta) for axis in "xyz"])

#update.
if bm.is_wrapped:
    bmesh.update_edit_mesh(mesh, False, False)
else:
    bm.to_mesh(mesh)
    mesh.update()
```

* Rendering: use blender to render obj. Render 360 degree gif.