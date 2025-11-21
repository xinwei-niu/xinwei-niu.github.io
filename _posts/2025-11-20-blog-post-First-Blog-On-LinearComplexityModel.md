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

Under long-context reasoning tasks, the quadratic growth of the attention operation becomes a major bottleneck, since increasing the sequence length \\(N\\) leads to a rapid increase in both computation and memory cost. As \\(N\\) grows into the hundreds of thousands or millions, storing and manipulating the full \\(N \times N\\) attention matrix becomes infeasible, motivating the development of more efficient attention mechanisms that reduce or avoid the \\(\mathcal{O}(N^{2})\\) complexity.


Hence, later works started looking for clever ways to get around this massive computation bottleneck. One of the first big ideas was Locality-Sensitive Hashing (LSH) Attention [4], which cuts the complexity down to \\(\mathcal{O}(N \log N)\\). The trick here is simple but powerful: instead of comparing every token with every other token, LSH groups similar tokens into buckets so that attention only happens within those smaller groups. Fewer comparisons, faster models.

Then came Linear Attention [5], which pushed things even further by reducing the cost to linear time, \\(\mathcal{O}(N)\\). By rethinking the softmax attention itself: Instead of explicitly building the full attention matrix, Linear Attention rewrites the operation using kernel functions so that attention can be computed through a series of efficient matrix multiplications. The result is that you get attention-like behavior without paying the quadratic price. It assumes that the exponential kernel can be approximated or represented by a feature map \\(\phi\\) such that \\( \exp(q^\top k)=\phi(q)^\top \phi(k) \\). Substituting this into the numerator yields \\[ \sum_{j=1}^N \exp(q_i^\top k_j)v_j = \phi(q_i)^\top \left( \sum_{j=1}^N \phi(k_j)v_j^\top \right), \\] allowing us to define \\( S=\phi(K)^\top V \\) so that all numerators can be written compactly as \\( \phi(Q)S = \phi(Q)(\phi(K)^\top V) \\). Similarly, the denominator becomes \\[ \sum_{j=1}^N \exp(q_i^\top k_j)=\phi(q_i)^\top \left( \sum_{j=1}^N \phi(k_j) \right), \\] and defining \\( z=\phi(K)^\top \mathbf{1}_N \\) gives the denominator vector as \\( \phi(Q)z \\). Combining numerator and denominator gives the linear-attention approximation: \\[ \mathrm{Att}(Q,K,V)=\frac{\phi(Q)(\phi(K)^\top V)}{\phi(Q)(\phi(K)^\top \mathbf{1}_N)}, \\] and in row-wise form \\( y_i=\frac{\phi(q_i)^\top S}{\phi(q_i)^\top z} \\), where \\( S=\phi(K)^\top V \\) and \\( z=\phi(K)^\top \mathbf{1}_N \\). Since \\(S\\) and \\(z\\) are computed once in \\(\mathcal{O}(N)\\) time and reused for all queries, the overall complexity becomes linear in sequence length, yielding the key efficiency benefit of linear attention.



---
## State-Space Model: S4, Mamba and Mamba 2

State-Space Models are 

Structured State Space sequence model (S4) [7]

HIPPO


Recurrent Mode:

Parallel Mode:



Mamba 2 []



---
## DeltaNet and Future Work







---

### References

[1] Hochreiter, S., & Schmidhuber, J. (1997). Long short-term memory. Neural computation, 9(8), 1735-1780.

[2] Cho, K., Van Merriënboer, B., Gulcehre, C., Bahdanau, D., Bougares, F., Schwenk, H., & Bengio, Y. (2014). Learning phrase representations using RNN encoder-decoder for statistical machine translation. arXiv preprint arXiv:1406.1078.

[3] Vaswani, A., Shazeer, N., Parmar, N., Uszkoreit, J., Jones, L., Gomez, A. N., ... & Polosukhin, I. (2017). Attention is all you need. Advances in neural information processing systems, 30.

[4] Kitaev, N., Kaiser, Ł., & Levskaya, A. (2020). Reformer: The efficient transformer. arXiv preprint arXiv:2001.04451.

[5] Katharopoulos, A., Vyas, A., Pappas, N., & Fleuret, F. (2020, November). Transformers are rnns: Fast autoregressive transformers with linear attention. In International conference on machine learning (pp. 5156-5165). PMLR.

[6] Schlag, I., Irie, K., & Schmidhuber, J. (2021, July). Linear transformers are secretly fast weight programmers. In International conference on machine learning (pp. 9355-9366). PMLR.

[7] Gu, A., Goel, K., & Ré, C. (2021). Efficiently modeling long sequences with structured state spaces. arXiv preprint arXiv:2111.00396.

[8] Gu, A., & Dao, T. (2024, May). Mamba: Linear-time sequence modeling with selective state spaces. In First conference on language modeling.

[] Yang, S., Wang, B., Zhang, Y., Shen, Y., & Kim, Y. (2024). Parallelizing linear transformers with the delta rule over sequence length. Advances in neural information processing systems, 37, 115491-115522.

[] Yang, S., Kautz, J., & Hatamizadeh, A. (2024). Gated delta networks: Improving mamba2 with delta rule. arXiv preprint arXiv:2412.06464.

[] Team Kimi(2025). Kimi Linear: An Expressive, Efficient Attention Architecture. arXiv preprint arXiv:2510.26692.
