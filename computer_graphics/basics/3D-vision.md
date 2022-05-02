REcovering thee underlying structure from iamges

Pose Estimation

RGB(D) Image -> 3D structure -> 3D understanding

# 3D Representations

Geometric representations in deep learning broadly can be divided into 3 classes:

- voxel-based: limited in resolution, offer no topological guarantees, cannot represent sharp features
- point-based: memory issues, do not capture manifold connectivity
- mesh-based

State-of-the-art deep learning systems output 3D geometry as **point clouds**, **triangle meshes**, **voxel grids**, and **implicit surfaces**

## Volumetric
3D Voxel Grids

- 3D ShapeNets, Deep Belief Network, 2015
- 3D GAN, 2016
- Sparse 3D Convolutional Networks, 2016

## Light Field Representation (LFD)

## Multi-view

## Triangular mesh

## Point cloud

- PointNet, PointNet++

## Implicit representation
arbitrary topology

decision boundary

- Occupancy Network, CVPR 2019
    - octree
- DeepSDF, signed distance field, CVPR 2019
    - Shape completion: noisy pointcloud -> mesh
    - why model sharp edge? ReLU: piece-wise smooth
- pixel2mesh, graph convolutional network, 2018
    - graph unpooling layer: subdivision=deconvolution
    - motivated by geometric mesh flow
    - perceptual feature pooling
    - Regularization: Laplacian-smoothness