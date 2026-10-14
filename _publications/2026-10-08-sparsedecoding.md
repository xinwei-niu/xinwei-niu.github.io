---
title: 'SparseDecoding: Decoding-Aware Pruning for Accurate and Efficient LLM Inference'
thumbnail: /images/publications/sparsedecoding/overview-SparseDecoding.svg
collection: publications
category: preprints
permalink: /publication/sparsedecoding/
date: '2026-10-08'
venue: arXiv preprint, arXiv:2610.12327
excerpt: Decoding-aware calibration and an optimized N:M sparse matrix-vector kernel
  improve pruned LLM generation quality and decoding efficiency.
paperurl: https://arxiv.org/pdf/2610.12327
citation: 'Qitong Wang<sup>*</sup>, <strong>Xinwei Niu</strong><sup>*</sup>, Mingluo Su, Shanwei Zhao, Shiai
  Zhu, and Huan Wang. (2026). SparseDecoding: Decoding-Aware Pruning for Accurate
  and Efficient LLM Inference. <i>arXiv preprint arXiv:2610.12327</i>.'
abstract: 'The memory-bound nature of the decoding stage of large language model (LLM)
  inference incurs significant latency. Layer-wise training-free network pruning approaches
  guided by the Hessian have been a prominent solution to this problem, as pruning
  reduces the number of nonzero parameters read from memory during decoding.


  Nevertheless, typical methods in this line compute the Hessian using pre-collected
  natural sequences, whereas the model is fed self-generated tokens during decoding,
  creating a distribution shift between the two sequences. The Hessian calculated
  on the natural sequence is different from that calculated on the generated sequence.
  We observe that this discrepancy causes the activation distribution during generation
  to deviate from that used for pruning, further hurting the pruned model performance.


  Moreover, most existing LLM pruning methods that bring actual speedup primarily
  target the sparse matrix-matrix (SpMM) multiplication, providing limited support
  for the sparse matrix-vector (SpMV) operations, which dominate decoding. To solve
  these problems, we introduce SparseDecoding, a principled decoding-aware pruning
  framework tailored for accurate and efficient LLM decoding.


  Specifically, at the algorithmic axis, SparseDecoding constructs calibration matrices
  from layer-wise activations collected during the dense-model autoregressive generation,
  excluding prefill, thereby aligning the pruning objective with the decoding activations.
  At the system axis, we develop an optimized N:M sparse matrix-vector kernel with
  bitmask indexing and fixed-step traversal.


  Substantial empirical results on representative LLMs (Llama-3.1-8B, Llama-3.3-70B,
  Qwen3-14B / 32B) demonstrate that our method consistently outperforms standard fixed-text
  calibration on the long-form generation benchmarks while achieving up to 1.48x end-to-end
  wall-clock decoding speedup on A100 GPUs.'
---

Qitong Wang<sup>*</sup>, **Xinwei Niu**<sup>*</sup>, Mingluo Su, Shanwei Zhao, Shiai Zhu, and Huan Wang.

**arXiv preprint · October 8, 2026**

## Abstract

{{ page.abstract | markdownify }}

[arXiv](https://arxiv.org/abs/2610.12327) · [PDF](https://arxiv.org/pdf/2610.12327)

## Figures

<figure class="paper-figure">
  <div class="paper-figure-panels">
    <a href="{{ '/images/publications/sparsedecoding/activation_drift_col.svg' | relative_url }}" aria-label="Open activation discrepancy plot at full size">
      <img src="{{ '/images/publications/sparsedecoding/activation_drift_col.svg' | relative_url }}" alt="Relative activation discrepancy across decoding steps, with an inset showing the first 40 steps." loading="lazy">
      <span>(a) Activation discrepancy during decoding</span>
    </a>
    <a href="{{ '/images/publications/sparsedecoding/llama3-1-8b_prefill_decode_speedup.svg' | relative_url }}" aria-label="Open prefill and decoding speedup plot at full size">
      <img src="{{ '/images/publications/sparsedecoding/llama3-1-8b_prefill_decode_speedup.svg' | relative_url }}" alt="Sparse prefill speedup is above the dense baseline, while sparse decoding speedup is below it across sequence lengths." loading="lazy">
      <span>(b) Prefill and decoding speedup</span>
    </a>
  </div>
  <figcaption><strong>Figure 1.</strong> Motivation for decoding-aware sparse inference: activation discrepancies emerge early in generation, and sparse kernels designed for prefill do not necessarily accelerate decoding.</figcaption>
</figure>

<figure class="paper-figure">
  <a href="{{ '/images/publications/sparsedecoding/overview-SparseDecoding.svg' | relative_url }}" aria-label="Open SparseDecoding overview at full size">
    <img src="{{ '/images/publications/sparsedecoding/overview-SparseDecoding.svg' | relative_url }}" alt="Comparison of standard N:M pruning and SparseDecoding, from calibration data and mask selection to decode-time execution." loading="lazy">
  </a>
  <figcaption><strong>Figure 2.</strong> SparseDecoding uses generated-token activations for calibration, excludes prefill statistics, and executes pruned weights with an optimized N:M sparse matrix-vector kernel.</figcaption>
</figure>

Figures from [SparseDecoding on arXiv](https://arxiv.org/html/2610.12327v1). Select a figure to view it at full size.
