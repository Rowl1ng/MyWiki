# Computer Vision


- 问题：图像是三维实体的二维投影。因此，直接从图像中获得的简单的特征测量结果，会受到物体的**空间姿态、光照以及距离**的影响。只有当模式分类方法是：基于那些不随成像条件的变化而变化的特征时，才会取得好的结果。即我们能通过图像得到对物体真实属性的估计，例如反射率和形状。
- 二值图：从二值图中，我们只能得到物体的轮廓。很难得到：1.物体表面的凹凸形状；2）物体在空间中的姿态。

- Raster vs Vector：raster image（位图）和vector image（矢量图形）。JPEG和PNG都是属于raster image。最普遍的vector image的格式是SVG。Raster image和vector image的最大差别就是，当raster image的品质越好的时候，file size就会越大；而vector image没有这个问题。

# Reference

- 清华大学出版社的《图像处理与计算机视觉算法及应用(第0版) 》TP391.41/P121.
