
# Generation

## Generative Adversarial Network (GAN)
GAN models are known for potentially unstable training and less diversity in generation due to their adversarial training nature.




## VAE
VAE relies on a surrogate loss.


- [如何简单易懂地理解变分推断(variational inference)？](https://www.zhihu.com/question/41765860)
- [Inference Suboptimality in Variational Autoencoders](https://arxiv.org/pdf/1801.03558.pdf)
- [The Reparameterization Trick](http://gregorygundersen.com/blog/2018/04/29/reparameterization/)
- [Paper List](https://github.com/matthewvowels1/Awesome-VAEs)


## Flow-based Model

Flow models have to use specialized architectures to construct reversible transform.

Normalizing flows is a class of generative models focusing on mapping a complex probability distribution to a simple distribution such as a Gaussian. 
 * normalizing flow的正向和逆向是完全一一对应的可逆过程，不需要在base和target distribution重复可视化
  - 李宏毅：Flow-based Generative Model
  
## Diffusion Model

  * Diffusion model：独立高斯分布可加性
  
![overviews of different generative models](https://lilianweng.github.io/posts/2021-07-11-diffusion-models/generative-overview.png)


## Reference

- [What are Diffusion Models?](https://lilianweng.github.io/posts/2021-07-11-diffusion-models/)

# Conditional Generation

consider learning a conditional mapping function $$G: \mathcal X \rightarrow \mathcal Y$$ which generates an output $$\mathbf y \in \mathcal Y$$. Our goal is to learn a multi-modal mapping $$G: \mathcal X \times \mathcal Z \rightarrow \mathcal Y$$ such that an input $x$ can be mapped to multiple and diverse ouputs in $\mathcal Y$ depending on the latent factors encoded in $\mathbf z \in \mathcal Z$. 

offen suffers from mode-collapse problem.