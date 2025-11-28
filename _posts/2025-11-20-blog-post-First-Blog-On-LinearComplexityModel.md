---
title: 'Efficient Methods for Generative Models 1: Linear Attention, State-Space Models, and Linear RNNs'
date: 2025-11-20
permalink: /posts/2025/11/blog-1-efficent/
tags:
  - Efficient Architecture
  - Machine Learning
  - Long Context
  - Linear Attention
---

Modern sequence modeling has evolved from recurrent architectures to attention-based models and, more recently, state-space approaches. Traditional RNNs introduced an efficient way to process sequential data but struggled with long-term dependencies. Transformers later revolutionized the field with attention mechanisms, though their quadratic cost limits scalability to long contexts. This has driven research into more efficient alternatives—such as linear attention, state-space models like S4 and Mamba, and newer architectures like DeltaNet, that aim to combine scalability, stability, and strong modeling capacity for long-range sequence tasks.



## Introduction to Recurrent Neural Networks (RNNs)

Recurrent Neural Networks (RNNs) are a class of neural networks specifically designed to handle **sequential and temporal data** by maintaining a hidden state that evolves over time. Unlike feedforward networks, RNNs have **recurrent connections** that allow information from previous time steps to influence the current computation, effectively providing the network with memory. At each time step \\(t\\), the hidden state \\(h_t\\) is computed as:

\\[
h_t = f(W_{xh} x_t + W_{hh} h_{t-1} + b_h)
\\]

where \\(x_t\\) is the input at time step \\(t\\), \\(h_{t-1}\\) is the hidden state from the previous step, \\(W_{xh}\\) and \\(W_{hh}\\) are learnable weight matrices, \\(b_h\\) is a bias term, and \\(f(\cdot)\\) is a nonlinear activation function such as \\(\tanh\\) or \\(\text{ReLU}\\).  

A key advantage of RNNs is their ***linear computational complexity with respect to the input sequence length***. At each step, the network performs a fixed set of operations, making the total cost scale linearly with the number of time steps. This efficiency allows RNNs to process sequences of arbitrary length without a combinatorial explosion in computation, which was one reason for their widespread adoption in language modeling, speech recognition, and time-series prediction during the 2010s.  

Despite this efficiency, standard RNNs face significant training challenges, primarily the **vanishing and exploding gradient problem**. During backpropagation through time (BPTT), gradients are propagated through the recurrent weights over many time steps. For long sequences, this can lead to gradients that **decay exponentially** (vanishing) or **grow uncontrollably** (exploding), making it difficult for the network to capture long-term dependencies. These limitations motivated the development of gated variants such as **Long Short-Term Memory (LSTM)**[^2] and **Gated Recurrent Unit (GRU)** networks, which incorporate mechanisms to preserve information over extended sequences and stabilize training.

---


## Introduction to Linear Attention
The attention mechanism in the **Transformer** {% cite vaswani2023attentionneed %} involves computing pairwise interactions between all tokens in the sequence. Given $$Q, K, $$ and $$V$$ which are query, key, value respectively, 
If \\(Q \in \mathbb{R}^{N \times d}\\) and \\(K \in \mathbb{R}^{N \times d}\\), then the attention score matrix is
\\(
QK^{\top} \in \mathbb{R}^{N \times N},
\\)
and its computational complexity is
\\(
\mathcal{O}(N^{2}),
\\)

Under long-context reasoning tasks, the quadratic growth of the attention operation becomes a major bottleneck, since increasing the sequence length \\(N\\) leads to a rapid increase in both computation and memory cost. As \\(N\\) grows into the hundreds of thousands or millions, storing and manipulating the full \\(N \times N\\) attention matrix becomes infeasible, motivating the development of more efficient attention mechanisms that reduce or avoid the \\(\mathcal{O}(N^{2})\\) complexity.


Hence, later works started looking for clever ways to get around this massive computation bottleneck. One of the first big ideas was Locality-Sensitive Hashing (LSH) Attention [^4], which cuts the complexity down to \\(\mathcal{O}(N \log N)\\). The trick here is simple but powerful: instead of comparing every token with every other token, LSH groups similar tokens into buckets so that attention only happens within those smaller groups. Fewer comparisons, faster models.

Then came Linear Attention [^5], which pushed things even further by reducing the cost to linear time, \\(\mathcal{O}(N)\\). By rethinking the softmax attention itself: Instead of explicitly building the full attention matrix, Linear Attention rewrites the operation using kernel functions so that attention can be computed through a series of efficient matrix multiplications. The result is that you get attention-like behavior without paying the quadratic price. It assumes that the exponential kernel can be approximated or represented by a feature map \\(\phi\\) such that \\( \exp(q^\top k)=\phi(q)^\top \phi(k) \\). Substituting this into the numerator yields \\[ \sum_{j=1}^N \exp(q_i^\top k_j)v_j = \phi(q_i)^\top \left( \sum_{j=1}^N \phi(k_j)v_j^\top \right), \\] allowing us to define \\( S=\phi(K)^\top V \\) so that all numerators can be written compactly as \\( \phi(Q)S = \phi(Q)(\phi(K)^\top V) \\). Similarly, the denominator becomes \\[ \sum_{j=1}^N \exp(q_i^\top k_j)=\phi(q_i)^\top \left( \sum_{j=1}^N \phi(k_j) \right), \\] and defining \\( z=\phi(K)^\top \mathbf{1}_N \\) gives the denominator vector as \\( \phi(Q)z \\). Combining numerator and denominator gives the linear-attention approximation: \\[ \mathrm{Att}(Q,K,V)=\frac{\phi(Q)(\phi(K)^\top V)}{\phi(Q)(\phi(K)^\top \mathbf{1}_N)}, \\] and in row-wise form \\( y_i=\frac{\phi(q_i)^\top S}{\phi(q_i)^\top z} \\), where \\( S=\phi(K)^\top V \\) and \\( z=\phi(K)^\top \mathbf{1}_N \\). Since \\(S\\) and \\(z\\) are computed once in \\(\mathcal{O}(N)\\) time and reused for all queries, the overall complexity becomes linear in sequence length, yielding the key efficiency benefit of linear attention.



---
## State-Space Model: S4, Mamba and Mamba 2

State-space models (SSMs) provide a mathematically grounded framework for sequence modeling by representing long-range temporal dependencies through latent linear dynamical systems. A generic continuous-time SSM is expressed as
\[
\dot{x}(t) = A x(t) + B u(t), \qquad y(t) = C x(t) + D u(t),
\]
which, after discretization, yields a structured recurrence that can be evaluated either sequentially or in parallel. The challenge in modern deep learning applications is to parameterize these systems such that they remain stable, expressive, and computationally efficient on long sequences. Recent advances have focused on imposing algebraic structure on the transition matrix \(A\) to enable fast convolution, efficient parallelization, and numerically stable training. 

The Structured State Space sequence model (S4) is a foundational example of this line of work. S4 draws heavily on the HiPPO (High-Order Polynomial Projection Operators) method to initialize state dynamics in a basis capable of retaining long-range history. S4 represents the continuous-time transition matrix \(A\) in a **Normal Plus Low-Rank (NPLR)** form, ensuring controlled spectral properties while maintaining flexibility. Through a learned similarity transformation, this representation is converted into a **Diagonal Plus Low-Rank (DPLR)** form. The diagonal component allows exact and efficient discretization and supports parallel prefix multiplication, while the low-rank correction is applied using Woodbury-style updates. This architecture enables fast kernel generation and efficient convolution-based evaluation, allowing S4 to model long sequences with high stability and strong gradient flow.

Mamba extends this framework by introducing **selective** state-space updates, where the discretized recurrence is modulated by input-dependent gates. Instead of a fixed state update
\[
x_{t+1} = \overline{A} x_t + \overline{B} u_t,
\]
Mamba applies elementwise modulation,
\[
x_{t+1} = f_t \odot (\overline{A} x_t) + g_t \odot (\overline{B} u_t),
\]
where the gates \(f_t\) and \(g_t\) are dynamically computed from the input sequence. This allows the model to selectively retain, suppress, or transform information at each timestep while preserving the computational structure of the underlying SSM. The key advantage of Mamba lies in the fact that the gating functions augment expressivity without sacrificing linear-time recurrence or the ability to execute fast parallel prefix computations.

**Recurrent mode** evaluation in Mamba processes tokens sequentially and operates in \(O(N)\) per timestep, where \(N\) is the state dimension. GPU implementations typically fuse the diagonal evolution, low-rank correction, and gating operations into a single kernel to minimize memory movement. This yields high streaming throughput suitable for autoregressive inference and online applications. In contrast, **parallel mode** evaluation constructs a time-varying convolution kernel induced by the gated recurrence. Since the coefficients depend on the input, Mamba performs a parallel prefix-product over the diagonal dynamics and integrates low-rank corrections in a block-wise manner. This allows efficient training on long sequences and batch inference, although the algorithmic structure is more complex than in fixed-parameter SSMs such as S4.

Mamba 2 further refines this design by improving numerical conditioning, parameter efficiency, and hardware alignment. From a modeling perspective, Mamba 2 adopts more stable parameterizations for the diagonal and low-rank components, ensuring contractive behavior under long prefix-product scans and reducing sensitivity to mixed-precision training. Algorithmically, Mamba 2 introduces a more efficient parallel scan procedure that avoids explicit storage of per-timestep kernel factors, significantly reducing memory overhead. Systems-level optimizations include a redesigned layout for state vectors and parameters to maximize GPU coalescing, as well as enhanced fused kernels that reduce launch overhead and exploit register-level tiling. These improvements yield faster training, lower memory consumption, and more robust behavior on very long sequences. Overall, Mamba 2 represents an evolution of the selective SSM framework that is both theoretically more stable and practically more hardware-efficient.

---

## RetNet, DeltaNet and More Linear Models

Fast Weight Programmers (FWPs) []


DeltaNet

DeltaNet explores the intermediate space between purely recurrent SSMs and fully parallelizable convolutional or attention-based architectures. Its design decomposes the sequence update into components that can be executed either in parallel or recurrently, allowing the model to capture long-range dependencies while maintaining predictable latency. DeltaNet variants often incorporate structured transitions combined with delta-function or short-range convolutional updates, enabling them to achieve strong performance on long-context tasks while remaining simpler to implement and optimize compared to fully diagonalized SSMs.

Future research directions include advancing the theoretical understanding of structured transition operators, particularly the stability properties of diagonal-plus-low-rank parameterizations under input-dependent modulation. Hardware-aware model design remains another important goal, as efficient execution of SSMs increasingly depends on custom fused kernels, memory layouts, and prefix-scan algorithms. Hybrid architectures that combine SSMs with sparse or content-based attention are also promising, offering ways to capture both long-range and local interactions efficiently. Finally, emerging directions such as adaptive state dimension, learnable discretization schemes, quantized SSM inference, and multimodal selective SSMs present exciting opportunities for further improving scalability and applicability.







---

### References

{% bibliography --cited %}

