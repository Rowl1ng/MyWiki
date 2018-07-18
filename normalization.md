## Simplified Whitening
The mean and variance of the pixels across the batch is calculated and then centered and scaled respectively. It’s said that it speeds up convergence.

## Local Contrast Normalization

```python
def LocalContrastNorm(image,radius=9):
    """
    image: torch.Tensor , .shape => (1,channels,height,width) 
    
    radius: Gaussian filter size (int), odd
    """
    if radius%2 == 0:
        radius += 1
    def get_gaussian_filter(kernel_shape):
        x = np.zeros(kernel_shape, dtype='float64')
 
        def gauss(x, y, sigma=2.0):
            Z = 2 * np.pi * sigma ** 2
            return  1. / Z * np.exp(-(x ** 2 + y ** 2) / (2. * sigma ** 2))
 
        mid = np.floor(kernel_shape[-1] / 2.)
        for kernel_idx in range(0, kernel_shape[1]):
            for i in range(0, kernel_shape[2]):
                for j in range(0, kernel_shape[3]):
                    x[0, kernel_idx, i, j] = gauss(i - mid, j - mid)
 
        return x / np.sum(x)
    
    n,c,h,w = image.shape[0],image.shape[1],image.shape[2],image.shape[3]

    gaussian_filter = Variable(torch.Tensor(get_gaussian_filter((1,c,radius,radius))))
    filtered_out = F.conv2d(image,gaussian_filter,padding=radius-1)
    mid = int(np.floor(gaussian_filter.shape[2] / 2.))
    ### Subtractive Normalization
    centered_image = image - filtered_out[:,:,mid:-mid,mid:-mid]
    
    ## Variance Calc
    sum_sqr_image = F.conv2d(centered_image.pow(2),gaussian_filter,padding=radius-1)
    s_deviation = sum_sqr_image[:,:,mid:-mid,mid:-mid].sqrt()
    per_img_mean = s_deviation.mean()
    
    ## Divisive Normalization
    divisor = np.maximum(per_img_mean.data.numpy(),s_deviation.data.numpy())
    divisor = np.maximum(divisor, 1e-4)
    new_image = centered_image.data / torch.FloatTensor(divisor)
    return new_image
```
## Local Response Normalization


## Reference

- [Normalizations in Neural Networks](http://yeephycho.github.io/2016/08/03/Normalizations-in-neural-networks/)
- 上一篇文章的可视化版本：[Visualizing Different Normalization Techniques](https://medium.com/@dibyadas/visualizing-different-normalization-techniques-84ea5cc8c378)