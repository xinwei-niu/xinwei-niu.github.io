---
permalink: /
title: "Academic Profile"
description: "Xinwei Niu — Research Assistant at Encode Lab, Westlake University, researching efficient generative models, sparse attention, and multimodal systems."
author_profile: true
redirect_from:
  - /about/
  - /about.html


---

I am a Research Assistant in **Encode Lab at [Westlake University](https://westlake.edu.cn/)**, Hangzhou, China, working under the supervision of **[Huan Wang](https://huanwang.tech/)**. My research focuses on efficient generative models, particularly methods that reduce the computational and memory costs of large language model inference.

[Email](mailto:nxw2002@outlook.com) · [GitHub](https://github.com/xinwei-niu)

## Research Interests

I am particularly interested in hardware-aware sparse computation, including sparse attention, model pruning, and KV-cache eviction and compression. My recent work on [SparseDecoding]({{ '/publication/sparsedecoding/' | relative_url }}) investigates decoding-aware pruning and efficient sparse execution for large language models.

My broader interests include autoregressive and semi-autoregressive models, diffusion language models, state-space models, and linear recurrent architectures. I also study efficient generative modelling for image and video synthesis; my literature review on [efficient diffusion models for image synthesis]({{ '/files/IRR_Xinwei.pdf' | relative_url }}) is available online.

## Publications

{% assign post = site.publications | where: "permalink", "/publication/sparsedecoding/" | first %}
<div class="publication-row">
  <a class="publication-thumbnail" href="{{ post.url | relative_url }}">
    <img src="{{ post.thumbnail | relative_url }}" alt="Overview of the SparseDecoding calibration and inference pipeline" loading="lazy">
  </a>
  <div class="publication-row-content" markdown="1">

**[SparseDecoding: Decoding-Aware Pruning for Accurate and Efficient LLM Inference]({{ post.url | relative_url }})**

Qitong Wang, **Xinwei Niu**, Mingluo Su, Shanwei Zhao, Shiai Zhu, and Huan Wang.

*arXiv preprint*, 2026. [arXiv](https://arxiv.org/abs/2610.12327) · [PDF](https://arxiv.org/pdf/2610.12327)

{% include publication-abstract.html %}

  </div>
</div>

## Education and Experience

<div class="experience-list">
  <div class="experience-row">
    <a class="institution-logo" href="https://westlake.edu.cn/"><img src="{{ '/images/institutions/westlake.png' | relative_url }}" alt="Westlake University logo" loading="lazy"></a>
    <div class="experience-content"><h3><a href="https://westlake.edu.cn/">Westlake University</a></h3><p class="experience-role">Research Assistant · Encode Lab</p><p>Hangzhou, China. Working under the supervision of <a href="https://huanwang.tech/">Huan Wang</a> on efficient generative modelling and large language model inference.</p></div>
  </div>
  <div class="experience-row">
    <a class="institution-logo" href="https://www.ed.ac.uk/"><img src="{{ '/images/institutions/edinburgh.png' | relative_url }}" alt="University of Edinburgh logo" loading="lazy"></a>
    <div class="experience-content"><h3><a href="https://www.ed.ac.uk/">University of Edinburgh</a></h3><p class="experience-role">MSc in Artificial Intelligence</p><p>Postgraduate studies in the <a href="https://informatics.ed.ac.uk/">School of Informatics</a>.</p></div>
  </div>
  <div class="experience-row">
    <a class="institution-logo" href="https://www.manchester.ac.uk/"><img src="{{ '/images/institutions/manchester.png' | relative_url }}" alt="University of Manchester logo" loading="lazy"></a>
    <div class="experience-content"><h3><a href="https://www.manchester.ac.uk/">University of Manchester</a></h3><p class="experience-role">BSc with Honours in Artificial Intelligence</p><p>Undergraduate studies in the <a href="https://www.cs.manchester.ac.uk/">Department of Computer Science</a>.</p></div>
  </div>
  <div class="experience-row">
    <a class="institution-logo" href="https://research.samsung.com/src-b"><img src="{{ '/images/institutions/samsung.png' | relative_url }}" alt="Samsung Research logo" loading="lazy"></a>
    <div class="experience-content"><h3><a href="https://research.samsung.com/src-b">Samsung Research China, Beijing</a></h3><p class="experience-role">Research Internship · Language Understanding Lab</p><p>During my undergraduate studies, I gained experience with multimodal large language models and vision–language representations.</p></div>
  </div>
</div>

<details class="personal-details" markdown="1">
<summary>Personal Interests</summary>

In my spare time, I enjoy travelling, especially around Europe.

<div class="travel-grid">
  <img src="{{ '/files/travel1.jpg' | relative_url }}" alt="A photograph from my travels in Europe" loading="lazy">
  <img src="{{ '/files/travel4.jpg' | relative_url }}" alt="Another moment from my travels" loading="lazy">
  <img src="{{ '/files/travel2.jpg' | relative_url }}" alt="A view captured while travelling" loading="lazy">
  <img src="{{ '/files/travel3.jpg' | relative_url }}" alt="A memory from exploring Europe" loading="lazy">
</div>

<figure class="personal-memory">
  <img src="{{ '/files/hammy.jpg' | relative_url }}" alt="Yashi, my hamster" loading="lazy">
  <figcaption>In memory of my little hammy, Yashi (ヤシ, 🥥).</figcaption>
</figure>

</details>
