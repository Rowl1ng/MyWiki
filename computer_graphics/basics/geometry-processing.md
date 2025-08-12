


# Representations

## voxel
discretization

## point cloud
discretization of surface into 3D points
does not model connectivity

[Generating Point Samples on a Mesh](https://www.fwilliams.info/point-cloud-utils/sections/mesh_sampling/)


## Meshes
discretization into vertices and faces



# File Format

## off


## obj

## fbx

# Datasets

Target Dataset:

- SHREC13STB: 1258 models of 90 classes
- [Princeton Shape Benchmark (2003)][5]:  1,814 models collected from the web in .OFF format. Used to evaluating shape-based retrieval and analysis algorithms.
    - hierachical label supports clssification of multiple granularities
    - 161 classes each contain at least 4 models at most 100 models

![此处输入图片的描述][6]

- [Dataset for IKEA 3D models and aligned images (2013)][7]: 759 images and 219 models including Sketchup (skp) and Wavefront (obj) files, good for pose estimation.
- [ModelNet (2015)][8]: 127915 3D CAD models from 662 categories
    - ModelNet10: 4899 models from 10 categories
    - ModelNet40: 12311 models from 40 categories, all are uniformly orientated

![此处输入图片的描述][9]

- [Shapenet 2015][10]: 3Million+ models and 4K+ categories. A dataset that is large in scale, well organized and richly annotated.
- [ObjectNet3D: A Large Scale Database for 3D Object Recognition (2016) ][11]:100 categories, 90,127 images, 201,888 objects in these images and 44,147 3D shapes.
Tasks: region proposal generation, 2D object detection, joint 2D detection and 3D object pose estimation, and image-based 3D shape retrieval
Benchmark:

- [Kinect300][12]: Abstract and noisy
    - a 3D Sketch Dataset, with 300 3D Sketches. 30 classes, each with 10 sketches by utilizing a Kinect-based virtual 3D drawing system
    - format: .off

 
  [5]: http://segeval.cs.princeton.edu/
  [6]: https://github.com/timzhang642/3D-Machine-Learning/raw/master/imgs/Princeton%20Shape%20Benchmark%20(2003).jpeg
  [7]: http://ikea.csail.mit.edu/
  [8]: http://modelnet.cs.princeton.edu/#
  [9]: https://camo.githubusercontent.com/f1dbf1e2ff7a45cc574c2094253cdcc1b0fd8d59/687474703a2f2f3364766973696f6e2e7072696e6365746f6e2e6564752f70726f6a656374732f323031342f4d6f64656c4e65742f7468756d626e61696c2e6a7067
  [10]: https://www.shapenet.org/
  [11]: http://cvgl.stanford.edu/projects/objectnet3d/
  [12]: https://userweb.cs.txstate.edu/~yl12/SBR2016/project.html