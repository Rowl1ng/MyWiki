# Data collection

## Devices

|Name|Function|
|--|--|
|[HTC Vive][1]|GAMER VIVE VR System £499
|[Oculus][2]| Oculus Rift £399  |
||Oculus Quest(All in One) 64G £399 ;128G £499
||early 2020 will release Bare-Handed version
|[HoloLens](https://www.microsoft.com/en-us/hololens/buy)|

## Tutorials

[Learn from Oculus + Unity Developers w/ This Free 11-Unit VR Development Course](https://developer.oculus.com/blog/free-oculus-unity-vr-development-course/)

# 3D reconstruction

Typical input for 3D reconstruction: images and point clouds

Task: converting rough, imprecise, (distorted compared to gt) **sketch** to clean, complete 3D shape

- rough -> requiring interpretation of noisy signals

High quality 3D modeling:

- high resolution
- sharp features

## 2D sketch

- imcomplete -> requiring hallucination of missing parts

problem:

- ambiguities -> 
    - hand-desinged priors
    - limiting applications
    - learn the shapes from data
    - implicitly inferring relevant priors

## 3D sketch

# Loss Functions

- 2D: use differentiable renderer and measure inconsitencies in 2D
- 3D: Chamfer / regularized Wasserterin

# Terminology

-【artifact】就是合成图片中，不自然的、反常的、能让人看出是人为处理过的痕迹、区域、瑕疵等。


# Reference

      * Seminar：Sketching Fast and Slow: Cognition, Heuristics, and the Challenge of Intuitive Workflows from Kevin    
      *  * Prof. Daniel Sykora from CTU Prague

[1]: https://www.vive.com/uk/product/cosmos/?gclid=CjwKCAjw5_DsBRBPEiwAIEDRWyXCbSl6FfrrDH82GmMol5dQdGgQVCWEx-ESFb3dQ8szSFE0dYja_xoC5-8QAvD_BwE
[2]: https://www.oculus.com/setup/#rift-s-setup