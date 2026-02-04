---
title: 'Non-convex optimization for Over-parameterized Neural Nets: Reproducing Kernel Hilbert Space and Neural Tangent Kernel'
date: 2026-02-01
permalink: /posts/2026/02/blog-RKHS/
tags:
  - Reproducing Hilbert Space
  - Neural Tangent Kernel
  - Kernel Methods
  - Empirical Risk Minimization
layout: single
sidebar:
  enabled: false
author_profile: false
---

This blog is based on *Real Analysis* by Elias M. Stein and Rami Shakarchi, and *Learning Theory on First Principles* by Francis Bach.


## 1. Reproducing Kernel Hilbert Space(RKHS)
 
Here are the prerequisites for understanding RKHS:
### 1.1 Kernel Functions

(Definition 1.1) **Hilbert Space**: A Hilbert Space \\(\mathcal{H}\\) is a vector space with an inner product \\(\langle f,g\rangle_\mathcal{H}\\) where the norm is defined by \\(\|f\|_\mathcal{H} = \sqrt{\langle f,f\rangle_\mathcal{H}} \\). (\\(L^2\\) space is a type of Hilbert Space.)


(Theorem 1.1) **Riesz Representation Theorem**: Every continuous linear functional \\(\Phi\\) can be written in the form:

\\[\Phi(f) = \langle f, g \rangle\\] for some appropriate element \\(g \in \mathcal{H}\\).

(Definition 1.2) **Reproducing Property of RKHS**: A kernel of Hilbert Space has the reproducing property if, given an kernel function \\(K(\cdot, x)\\), define a reproducing kernel feature map \\(\Phi: \mathcal{X} \rightarrow \mathbb{R}^\mathcal{X}\\) which satisfies:

\\[\Phi (x) = \langle\Phi(\cdot), K(\cdot, x)\rangle\\]

(Theorem 1.2) **Moore-Aronszajn Theorem**: For every positive definite kernel function \\(K: \mathbb{R}^N \times \mathbb{R}^N \rightarrow \mathbb{R}\\), there exists a Hilbert Space \\(\mathcal{H}\\) with \\(K\\) as its reproducing kernel.





## 2. Neural Tangent Kernel

### 2.1 Intuition of NTK

The Neural Tangent Kernel (NTK) is a tool that allows us to treat a deep neural network as a linear model in a high-dimensional feature space. The central intuition is that as the width of a neural network layer $m$ approaches infinity, the training dynamics simplify. Instead of the weights moving in a complex, non-linear way, the network's output evolves as if it were performing a simple gradient descent on a fixed kernel.

### 2.2 Lazy Training

Under over-parameterized settings, the network enters a regime known as Lazy Training. In this state, because the model has an abundance of parameters, the "effort" required to minimize the loss is spread across so many neurons that each individual weight only needs to move an infinitesimal distance from its random initialization \\(\theta_0\\).Mathematically, the parameters \\(\theta\\) stay so close to \\(\theta_0\\) that the model can be linearized via a Taylor expansion:\\[f(x, \theta) \approx f(x, \theta_0) + \nabla_\theta f(x, \theta_0)^\top (\theta - \theta_0)\\] 
Because the weights barely move, the features learned by the network are essentially the random features present at initialization.


### 2.3 Definition of Neural Tangent Kernels

For a neural network \\(f(x, \theta)\\), the NTK \\(\Theta\\) is defined by the inner product of the gradients of the model output with respect to its parameters:\\[\Theta(x, x') = \sum_{p=1}^P \frac{\partial f(x, \theta)}{\partial \theta_p} \frac{\partial f(x', \theta)}{\partial \theta_p} = \langle \nabla_\theta f(x, \theta), \nabla_\theta f(x', \theta) \rangle\\]
In the infinite-width limit, this kernel becomes deterministic and remains constant throughout the entire training process.

### 2.4 Connection between NTK and RKHS

The connection is established by viewing the gradient \\(\nabla_\theta f(x, \theta_0)\\) as a feature map \\(\Phi(x)\\). According to the Moore-Aronszajn Theorem, the NTK defines a unique RKHS \\(\mathcal{H}_{ntk}\\). Training an infinite-width neural network with gradient descent is mathematically equivalent to finding the minimum-norm solution in this RKHS, essentially performing Kernel Ridge Regression with the NTK.


## 3. Application of Neural Tangent Kernel in Deep Learning

In modern research of Deep Learning topics, researchers begin to take NTK concepts for NLP tasks, with mainly over context window extension methods (known as NTK-aware interpolation) to preserve the high-frequency components that the network needs to learn fine-grained distinctions between nearby positions. This is particularly relevant when scaling Rotary Positional Embeddings (RoPE), where simple linear scaling often causes the model to "forget" high-resolution positional information.

Also, NTK is a tool which shows the convergence of neural networks. By proving that the kernel remains positive definite and constant during training, researchers can demonstrate that over-parameterized networks will always converge to a global minimum when trained with gradient descent, providing a theoretical bedrock for why "bigger is often better" in model architecture.




