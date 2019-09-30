---
title: "Filtering LRPN"
header: 
    overlay_image: /assets/images/posts/shadertoy-lrpn/Filtered.jpg
    show_overlay_excerpt: false
    actions: 
      - label: "SHADERTOY LINK"
        url: https://www.shadertoy.com/view/wlf3RH
classes: wide
# link: https://www.shadertoy.com/view/wlf3RH
# tags: [PRNG,LRPN,Noise,Shadertoy]
# categories: [Links]
---

Today, I posted a [shadertoy](https://www.shadertoy.com/view/wlf3RH) about the [Local Random Phase Noise](https://www.unilim.fr/pages_perso/guillaume.gilet/publications/pdf/ProcTextures.pdf) again. This implementation shows how to filter it and requires some maths equations that you cand find here.

**PS**
I will update this post during the week, it's under construction.
I'm open to suggestions, so email me or put a comment in the shadertoy implementation. 


<div align="center"><iframe width="640" height="360" frameborder="0" src="https://www.shadertoy.com/embed/wlf3RH?gui=true&t=10&paused=true&muted=true" allowfullscreen></iframe></div>

The local random phase noise, introduced in 2014 by Gilet et al.[^GSVDG14], is a follow-up to the Gabor noise model, presented by Lagae et al. in 2009[^LLDD09] and followed by a series of publications that improved the model. These noise functions are part of a family called Sparse Convolution Noise. It consists in the convolution of an arbitrary kernel function with a sparse points distribution (poisson, stratified, ...).
In practice, you evaluate a weighted sum of kernels shot at impulses $x_i$.

The [Gabor noise](http://graphics.cs.kuleuven.be/publications/LLDD09PNSGC/), as its name suggests, is the convolution of a points distribution and a gabor kernel. The Gabor kernel is the product of a Gaussian window function and a cosine function.

$$
\begin{align*}
    \text{GaborKernel}(\mathbf{x}, \lambda, \mathbf{f}, \Sigma ) &= g(\mathbf{x}; \lambda, \vec{0}, \Sigma) cos(\mathbf{F} \cdot \mathbf{x}) \\
    with \; g(\mathbf{x}; \lambda, \mu, \Sigma) &= \lambda \, exp( -\frac{1}{2} (\mathbf{x}-\mu)^T \Sigma^{-1} (\mathbf{x}-\mu) ) \\
    and \; \mathbf{f} &= 2\pi\begin{pmatrix} F_0 cos(w_0) \\ F_0 sin(w_0) \end{pmatrix}
\end{align*}
$$

The Gabor noise consists in the weighted sum of Gabor kernels at impulses $x_i$ :

$$ 
\text{GaborNoise}(\mathbf{x}) = \sum_{i}^{N} w_i \text{GaborKernel}(x-x_i;\lambda, \mathbf{f_i}, \Sigma) 
$$

In the original paper, the gaussian window function used is described as the following :

$$ 
\text{Gaussian}(\mathbf{x}) = K \, exp( -\pi\,a^2 \mathbf{x}^T\mathbf{x} )
$$

Its an isotropic gaussian function with $K$ as the magitude of the gaussian and $a$ the width of the function. I prefer to use a common gaussian function $g$ with mean $\mu$ and covariance matrix $\Sigma$ because it allows to have anisotropic shape by defining the covariance matrix. To retrieve the original formulation of the paper you have to write $\Sigma$ as $(2\pi a^2)^{-1} I$ where $I$ denotes the identity matrix.

The local random phase noise is slightly different. 
Instead of a pulse process, it uses a regular distribution of points. 
Then, we need to evaluate the product of another window function $W$ and a sum of cosines. In the paper, the authors used a Kaiser-Bessel window but stated that a gaussian function can be used as a substitute, and that's what I'm doing.

$$ \text{LRPN}(\mathbf{x}) = \sum_{i}^{N} W(\mathbf{x}-\mathbf{x}_i) \sum_{j}^{M} A_{ij} cos(2\pi \mathbf{f_{ij}} \cdot \mathbf{x} + \phi_{ij}) $$

This can be interpreted as the sum of Gabor kernels that shares the same Gaussian enveloppe. Why ? because now, we can use the same methodology described in the paper from 2009 to analytically filter it.

But we need some maths tools in order to do it. 
Our goal is to compute the convolution of the projected pixel footprint in texture space with the weight sums of kernels. Using the convolution theorem, we kown that a convolution in the spatial domain becomes a product in the spectral domain. So, we need to know what is the fourier transform of the gabor kernel, how to model the projected pixel footprint in the spectral domain and how to compute the product.

**First, the pixel footprint**. 
We want to model our pixel footprint like Lagae et al. : using a gaussian pixel footprint. Why ? because gaussians function are great for calculations as it often enables closed-form solutions. Lagae et al. used the same formulation as Heckbert in his master thesis[^Hec89].

The pixel footprint results in :

$$
    k_P(\mathbf{x}) = \frac{1}{2\pi\sqrt{|JJ^T|}} e^{ -\frac{1}{2} \big[ \mathbf{x}^T (JJ^{T})^{-1} \mathbf{x} \big] }          
$$

with 

$$
J = \sigma \begin{pmatrix} \frac{du}{dx} & \frac{du}{dy} \\ \frac{dv}{dx} & \frac{dv}{dy} \end{pmatrix}
$$

**Then, the fourier transform :**
We will use the non-unitary fourier transform in 2D with the angular frequency notation $\omega = 2\pi\xi$  

$$ 
    \hat{f}(\omega) = \int_{\mathbb{R}^D} f(\mathbf{x}) e^{-i\omega^T \mathbf{x}} d\mathbf{x}
$$

The Gabor kernel have a particular fourier transform as it becomes a sum of two symmetric gaussian :

$$
\hat{\text{Gabor}}(\omega) = \frac{k \; (2\pi) \; |\Sigma|^{1/2} }{2}  \big[ exp(-\frac{1}{2} (\omega+\mathbf{f})^T \Sigma (\omega+\mathbf{f})) +  exp(-\frac{1}{2} (\omega-\mathbf{f})^T \Sigma (\omega-\mathbf{f})) \big] 
$$

$$
\hat{\text{Gabor}}(\omega) = \frac{1}{2}  \big[ g(\omega;k \; (2\pi) \; |\Sigma|^{1/2},-\mathbf{f},\Sigma^{-1}) +  g(\omega;k \; (2\pi) \; |\Sigma|^{1/2},+\mathbf{f},\Sigma^{-1}) \big] 
$$

The proof uses Euler's Formulae and how to rearrange two square forms, you find it in the Matrix Cookbook[^PP12] in the "Gaussians" section :
<details>
<summary>Proof (*click*)</summary>
<p>
$$

\begin{align*}
    \hat{\text{Gabor}}(\omega) &= \int_{\mathbb{R}^2} g(\mathbf{x};k,\Sigma) cos(\vec{x} \cdot \vec{f}) exp(-i\omega^T\mathbf{x}) d\mathbf{x} \\
    &= \frac{k}{2} \int_{\mathbb{R}^2} exp( -\frac{1}{2} \mathbf{x}^T \Sigma^{-1} \mathbf{x}) exp(-i\omega^T\mathbf{x}) \big[ exp(i\mathbf{f}^T\mathbf{x}) + exp(-i\mathbf{f}^T\mathbf{x}) \big]  d\mathbf{x} \\
    \text{We distribute} & \text{ and split the integral in two} \\
    (1) &= \int_{\mathbb{R}^2} exp( -\frac{1}{2} \mathbf{x}^T \Sigma^{-1} \mathbf{x}) exp(-i\omega^T\mathbf{x}) exp(i\mathbf{f}^T\mathbf{x} ) d\mathbf{x} \\
    &= \int_{\mathbb{R}^2} exp( -\frac{1}{2} \mathbf{x}^T \Sigma^{-1} \mathbf{x}) exp(-i ( \omega^T\mathbf{x} - \mathbf{f}^T\mathbf{x})) d\mathbf{x} \\
    &= \int_{\mathbb{R}^2} exp( -\frac{1}{2} \mathbf{x}^T \Sigma^{-1} \mathbf{x} + (-i)(\omega-\mathbf{f})^T\mathbf{x}) d\mathbf{x} \\
    &= (2\pi) \; |\Sigma|^{1/2} \; exp(\frac{(-i)^2}{2} (\omega-\mathbf{f})^T \Sigma (\omega-\mathbf{f})) \\
    &= (2\pi) \; |\Sigma|^{1/2} \; exp(-\frac{1}{2} (\omega-\mathbf{f})^T \Sigma (\omega-\mathbf{f})) \\
    (2) &= (2\pi) \; |\Sigma|^{1/2} \; exp(-\frac{1}{2} (\omega+\mathbf{f})^T \Sigma (\omega+\mathbf{f})) \\
    \text{Yielding :} \\
    \hat{\text{Gabor}}(\omega) &= \frac{k \; (2\pi) \; |\Sigma|^{1/2} }{2}  \big[ exp(-\frac{1}{2} (\omega+\mathbf{f})^T \Sigma (\omega+\mathbf{f})) +  exp(-\frac{1}{2} (\omega-\mathbf{f})^T \Sigma (\omega-\mathbf{f})) \big] \\
\end{align*}
$$

</p>
</details>



The fourier transform of a Gaussian is still a Gaussian :

$$
\begin{align*}
\hat{\text{Gauss}}(\omega) &= k \; (2\pi) \; |\Sigma|^{1/2} exp(-\frac{1}{2} \, \omega^T \Sigma \, \omega) \\
&= g(\omega; k \; (2\pi) \; |\Sigma|^{1/2}, \vec{0}, \Sigma^{-1})
\end{align*}
$$

and because our pixel footprint is a normalized Gaussian function, we can remove the magnitude parameter.

<details>
<summary>Proof (*click*)</summary>
<p>
$$

\begin{align*}
    \hat{\text{Gauss}}(\omega) &= \int_{\mathbb{R}^2} g(\mathbf{x};k,\Sigma) exp(-i\omega^T\mathbf{x}) d\mathbf{x} \\
    &= k \; \int_{\mathbb{R}^2} exp(-\frac{1}{2} \mathbf{x}^T \Sigma^{-1} \mathbf{x}) exp(-i\omega^T\mathbf{x}) d\mathbf{x} \\
    &= k \; \int_{\mathbb{R}^2} exp(-\frac{1}{2} \mathbf{x}^T \Sigma^{-1} \mathbf{x} -i\omega^T\mathbf{x}) d\mathbf{x} \\
    &= k \; (2\pi) \; |\Sigma|^{1/2} exp(-\frac{1}{2} \, \omega^T \Sigma \, \omega) \\

\end{align*}
$$

</p>
</details>


**Now, the product**
Luckily Gaussians are closed under multiplication : the product of two multivariate Gaussian functions $g_1(\mathbf{x})$ and $g_2(\mathbf{x})$ gives another multivariate Gaussian $g_3(\mathbf{x})$:  

$$
\begin{align*}
    g(\mathbf{x};\lambda_1,\mu_1,\Sigma_1) &\cdot g(\mathbf{x};\lambda_2,\mu_2, \Sigma_2) = g(\mathbf{x};\lambda_3,\mu_3,\Sigma_3) \\
    where \; \Sigma_3 &= (\Sigma_1^{-1}+\Sigma_2^{-1})^{-1}\\
    \mu_3 &= \Sigma_3(\Sigma_1^{-1}\mu_1+\Sigma_2^{-1}\mu_2)\\
    \lambda_3 &= g(\mu_1;\lambda_1 \lambda_2,\mu_2,\Sigma_1+\Sigma_2)\\
\end{align*}
$$

Yielding another sum of Gaussians in the spectrum.


**Finally,** we can think that the resulting sum of Gaussians is the fourier transform of another anisotropic Gabor kernel and retrieve the new parameters.
By doing so, we 've got the filtered Gabor kernel for our procedural texture.




[^Hec89]: Paul S. Heckbert. Fundamentals of texture mapping and image warping. Technical Report UCB/CSD-89-516, EECSDepartment, University of California, Berkeley, Jun 1989. 2.4.1, 3.3

[^LLDD09]: Ares Lagae, Sylvain Lefebvre, George Drettakis and Philip Dutré. Procedural noise using sparse gabor convolution. ACM Transactions on Graphics (TOG), 28(3) :54, 2009. (document), 2.4.1, 2.3, 2.4.1, 2.4.1, 3.3 

[^GSVDG14]: Guillaume Gilet, Basile Sauvage, Kenneth Vanhoey,Jean-Michel Dischler and Djamchid Ghazanfarpour. Local random-phase noise for procedural texturing. ACM Transactions on Graphics (TOG), 33(6) :195, 2014. 2.4.2

[^PP12]: K. B. Petersen and M. S. Pedersen. The matrix cookbook, nov 2012. Version 20121115. 3.3.1, 4.3 