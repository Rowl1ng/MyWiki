Recovering thee underlying structure from images

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
pros:

- arbitrary topology
- decision boundary

implicit:

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

primitive based:

- generate parts as sequence: 3D-PRNN (ICCV), PQ-NET
- BSP-Net, constructs a 3D shape by convexes organized by a BSP-tree. The convexes can be seen as parametric primitives which can represent geometry details of 3D shapes rather than general structures.

hierarchically organized:

- GRASS, describe the shape structure by a hierarchical binary tree. Leaves represent the oriented bounding box (OBBs) and geometry features for each part.
- StructureNet, 2019. organized the hierarchical structure in the form of graphs.
- DSG-Net, 2021

# Structure and Geometry

# 3D Reconstruction or Generation

# Mesh-based deformation

generate shapes by deforming a mesh template.

# Shape Completion


