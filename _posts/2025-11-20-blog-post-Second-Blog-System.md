---
title: 'Efficient Methods for Generative Models 2: KV Cache, FlashAttention, vLLM'
date: 2025-11-20
permalink: /posts/2025/11/blog-2-efficent/
tags:
  - Efficient Architecture
  - Machine Learning
  - Long Context
  - Linear Attention
---


## Introduction to Recurrent Neural Networks (RNNs)

Recurrent Neural Networks (RNNs) are a class of neural networks specifically designed to handle **sequential and temporal data** by maintaining a hidden state that evolves over time. Unlike feedforward networks, RNNs have **recurrent connections** that allow information from previous time steps to influence the current computation, effectively providing the network with memory. At each time step \\(t\\), the hidden state \\(h_t\\) is computed as:

\\[
h_t = f(W_{xh} x_t + W_{hh} h_{t-1} + b_h)
\\]

where \\(x_t\\) is the input at time step \\(t\\), \\(h_{t-1}\\) is the hidden state from the previous step, \\(W_{xh}\\) and \\(W_{hh}\\) are learnable weight matrices, \\(b_h\\) is a bias term, and \\(f(\cdot)\\) is a nonlinear activation function such as \\(\tanh\\) or \\(\text{ReLU}\\).  

A key advantage of RNNs is their ***linear computational complexity with respect to the input sequence length***. At each step, the network performs a fixed set of operations, making the total cost scale linearly with the number of time steps. This efficiency allows RNNs to process sequences of arbitrary length without a combinatorial explosion in computation, which was one reason for their widespread adoption in language modeling, speech recognition, and time-series prediction during the 2010s.  

Despite this efficiency, standard RNNs face significant training challenges, primarily the **vanishing and exploding gradient problem**. During backpropagation through time (BPTT), gradients are propagated through the recurrent weights over many time steps. For long sequences, this can lead to gradients that **decay exponentially** (vanishing) or **grow uncontrollably** (exploding), making it difficult for the network to capture long-term dependencies. These limitations motivated the development of gated variants such as **Long Short-Term Memory (LSTM)**[2] and **Gated Recurrent Unit (GRU)** networks, which incorporate mechanisms to preserve information over extended sequences and stabilize training.

---


## Introduction to Linear Attention
The attention mechanism in the **Transformer**[1] involves computing pairwise interactions between all tokens in the sequence. Given $$Q, K, $$ and $$V$$ which are query, key, value respectively, 
If \\(Q \in \mathbb{R}^{N \times d}\\) and \\(K \in \mathbb{R}^{N \times d}\\), then the attention score matrix is
\\(
QK^{\top} \in \mathbb{R}^{N \times N},
\\)
and its computational complexity is
\\(
\mathcal{O}(N^{2}),
\\)

Under long-context reasoning tasks, the quadratic growth of the attention operation becomes a major bottleneck, since increasing the sequence length \\(N\\) leads to a rapid increase in both computation and memory cost. As \\(N\\) grows into the hundreds of thousands or millions, storing and manipulating the full \(N \times N\) attention matrix becomes infeasible, motivating the development of more efficient attention mechanisms that reduce or avoid the \\(\mathcal{O}(N^{2})\\) complexity.


Hence, later works started to trying mitigate this computation bottleneck Locality-Sensitive Hashing Attention [4] . 
Linear Attention[5] 






---

### References

[1] Hochreiter, S., & Schmidhuber, J. (1997). Long short-term memory. Neural computation, 9(8), 1735-1780.

[2] Cho, K., Van Merriënboer, B., Gulcehre, C., Bahdanau, D., Bougares, F., Schwenk, H., & Bengio, Y. (2014). Learning phrase representations using RNN encoder-decoder for statistical machine translation. arXiv preprint arXiv:1406.1078.

[3] Vaswani, A., Shazeer, N., Parmar, N., Uszkoreit, J., Jones, L., Gomez, A. N., ... & Polosukhin, I. (2017). Attention is all you need. Advances in neural information processing systems, 30.

[4] Kitaev, N., Kaiser, Ł., & Levskaya, A. (2020). Reformer: The efficient transformer. arXiv preprint arXiv:2001.04451.

[5] Katharopoulos, A., Vyas, A., Pappas, N., & Fleuret, F. (2020, November). Transformers are rnns: Fast autoregressive transformers with linear attention. In International conference on machine learning (pp. 5156-5165). PMLR.

[6] 


