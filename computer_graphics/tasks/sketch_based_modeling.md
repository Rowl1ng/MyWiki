Typical input for 3D reconstruction: images and point clouds

Task: converting rough, imprecise, (distorted compared to gt) **sketch** to clean, complete 3D shape

- rough -> requiring interpretation of noisy signals

High quality 3D modeling:

- high resolution
- sharp features

# 2D sketch

- imcomplete -> requiring hallucination of missing parts

problem:

- ambiguities -> 
    - hand-desinged priors
    - limiting applications
    - learn the shapes from data
    - implicitly inferring relevant priors

# 3D sketch

# Loss Functions

- 2D: use differentiable renderer and measure inconsitencies in 2D
- 3D: Chamfer / regularized Wasserterin

# Terminology

-【artifact】就是合成图片中，不自然的、反常的、能让人看出是人为处理过的痕迹、区域、瑕疵等。