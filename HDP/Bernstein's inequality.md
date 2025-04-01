---
tags:
  - HDP
author: Alessandro Straziota
date: 2025-02-27
---
Let $X_1, \dots, X_n$ be **independent** [[Sub-exponential Random Variables|sub-exponential random variables]], with [[Some definitions#Mean|mean]] $0$.

For every $t \geq 0$
$$
\mathbb{P}\left\lbrace \left\vert \sum_{i=1}^{N} X_i \right\vert \geq t \right\rbrace \leq 2 \cdot \exp \left[ -c \cdot \min\left( \frac{t^2}{\sum_{i=1}^{N} \Vert X_i \Vert_{\psi_1}^2}, \frac{t}{\max_{i}\Vert X_i \Vert_{\psi_1}} \right) \right]
$$

where $c > 0$ is an absolute constant.

Moreover, for every $a = (a_1, \dots, a_N) \in \mathbb{R}^N$
$$
\mathbb{P}\left\lbrace \left\vert \sum_{i=1}^{N} a_i X_i \right\vert \geq t \right\rbrace \leq 2 \cdot \exp \left[ -c \cdot \min\left( \frac{t^2}{ K^2 \Vert a \Vert_{2}^2}, \frac{t}{K \Vert a \Vert_{\infty}} \right) \right]
$$
where $K = \max_{i} \Vert X_i \Vert_{\psi_1}$.

A special case is when $a_i = 1/N$, i.e.
$$
\mathbb{P}\left\lbrace \left\vert \frac{1}{N}\sum_{i=1}^{N} X_i \right\vert \geq t \right\rbrace \leq 2 \cdot \exp \left[ -c \cdot \min\left( \frac{t^2}{ K^2 }, \frac{t}{K } \right) N \right]
$$


## Bernstein's inequality for bounded distributions
Let $X_1, \dots, X_n$ be **independent** [[Sub-exponential Random Variables|sub-exponential random variables]], with [[Some definitions#Mean|mean]] $0$, and such that $\vert X_i \vert \leq K$.
Then, for every $t \geq 0$ we have
$$
\mathbb{P}\left\lbrace \left\vert \sum_{i=1}^{N} X_i \right\vert \geq t \right\rbrace \leq 2 \exp\left( - \frac{t^2/2}{\sigma^2+ Kt/3} \right)
$$
where $\sigma^2 = \sum_{i=1}^{N} \mathbb{E} X_i^2$ is the [[Some definitions#Variance|variance]] of the sum $X_1 + \dots + X_N$.

> [!note] Exercise 2.8.5 (A bound on MGF). #exercise
> Let $X$ be a mean-zero random variable such that $\vert X \vert \leq K$.
> Prove the following bound on the [[Some definitions#MGF - Moment Generating Function|MGF]] of $X$:
> $$
> M_X(\lambda) =\mathbb{E}\exp(\lambda X) \leq \exp(g(\lambda)\mathbb{E}X^2)
> $$
> where
> $$
> g(\lambda) := \frac{\lambda^2/2}{1-\vert \lambda \vert K/3}
> $$
> provided that $\vert \lambda \vert < 3/K$.


