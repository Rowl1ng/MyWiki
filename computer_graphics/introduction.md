# Introduction

Geometric representations in deep learning broadly can be divided into 3 classes:

- voxel-based: limited in resolution, offer no topological guarantees, cannot represent sharp features
- point-based: memory issues, do not capture manifold connectivity
- mesh-based

State-of-the-art deep learning systems output 3D geometry as **point clouds**, **triangle meshes**, **voxel grids**, and **implicit surfaces**