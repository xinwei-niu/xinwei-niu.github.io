---
title: 'Non-convex optimization for Over-parameterized Neural Nets: Reproducing Kernel Hilbert Space and Neural Tangent Kernel'
date: 2025-02-01
permalink: /posts/2025/11/blog-RKHS/
tags:
  - Reproducing Hilbert Space
  - Neural Tangent Kernel
  - Kernel Methods
  - Kernel Trick
layout: single
sidebar:
  enabled: false
author_profile: false
---


## 1. Reproducing Kernel Hilbert Space
 
Here are the prerequisites for understanding RKHS:
### 1.1 Kernel Functions

(Definition 1.1) Hilbert Space: A Hilbert Space \(\mathcal{H}\) is a vector space with an inner product \(\langle f,g\rangle_\mathcal{H}\) where the norm is defined by \(||f||_\mathcal{H} = \sqrt{\langle f,f\rangle_\mathcal{H}} \). (\(L^2\) space is a type of Hilbert Space.)


(Theorem 1.1) Riesz Representation Theorem: Every continuous linear functional \(\Phi\) can be written in the form:

\[\Phi(f) = \langle f, g \rangle\] for some appropriate element \(g \in \mathcal{H}\).

(Definition 1.2) Reproducing Property of RKHS: A kernel of Hilbert Space has the reproducing property if, given an kernel function \(K(\cdot, x)\), define a reproducing kernel feature map \(\Phi: \mathcal{X} \rightarrow \mathbb{R}^\mathcal{X}\) which satisfies:

\[\Phi (x) = \langle\Phi(\cdot), K(\cdot, x)\rangle\]

(Theorem 1.2) Moore-Aronszajn Theorem: For every positive definite kernel function \(K: \mathbb{R}^N \times \mathbb{R}^N \rightarrow \mathbb{R}\), there exists a Hilbert Space \(\mathcal{H}\) with \(K\) as its reproducing kernel.





## 2. Neural Tangent Kernel

### 2.1 Intuition of NTK

### 2.1 Connection between NTK and RKHS


## 3. Application of Neural Tangent Kernel in Deep Learning

In modern research of Deep Learning topics, researchers begin to taken NTK concepts for NLP tasks, with mainly over context window extention methods (known as NTK-aware) to preserve the high-frequency components that the network needs to learn fine-grained distinctions between nearby positions.
Rotary Positional Embedding(RoPE)

Also, NTK is also being a tool which shows the 




