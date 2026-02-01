---
title: 'Note on Submodular Function Optimization, Minimization and Maximization, Lazy Greedy'
date: 2025-11-20
permalink: /posts/2025/11/blog-1-efficent/
tags:
  - Submodular
  - Algorithms
  - Greedy Algorithm
  - Linear Attention
layout: single
sidebar:
  enabled: false
author_profile: false
---

This blog is based on [week 10 of PKU Algorithms for Big Data Analysis](http://faculty.bicmr.pku.edu.cn/~wenzw/bigdata/lect-submodular.pdf).

## Introduction of Submodular Functions

**Definition 1.1 (Submodular Function).**  
Let \( V \) be a finite ground set. A set function  
\[
f : 2^{V} \rightarrow \mathbb{R}
\]
is called **submodular** if for all subsets \( A, B \subseteq V \),
\[
f(A) + f(B) \geq f(A \cup B) + f(A \cap B).
\]

**Definition 1.2 (Diminishing Returns Property).**  
A set function \( f : 2^{V} \rightarrow \mathbb{R} \) is said to satisfy the **diminishing returns property** if for all \( A \subseteq B \subseteq V \) and all \( x \in V \setminus B \),
\[
f(A \cup \{x\}) - f(A) \geq f(B \cup \{x\}) - f(B).
\]

**Useful properties:**
*Monotone:*
*Normalized:*


submodular maximization


