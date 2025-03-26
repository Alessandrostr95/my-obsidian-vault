---
author: Alessandro Straziota
tags:
  - HDP
date: 2025-03-25
---

Let $X_1, \dots, X_n$ be **independent** [[Sub-gaussian random variables|sub-gaussian random variables]], with [[Some definitions#Mean|mean]] $0$.

Then $\sum_{i=1}^{N}X_i$ is also a sub-gaussian random variable, and
$$
\left\Vert \sum_{i=1}^{N}X_i \right\Vert_{\psi^2}^2 \leq C \sum_{i=1}^{N} \Vert X \Vert_{\psi^2}^2
$$

^690103

where $C$ is an **absolute constant**.

### Proof
Let us analize the [[Some definitions#MGF - Moment Generating Function]].
For every $\lambda \in \mathbb{R}$ we have that
$$
\mathbb{E}\exp\left(\lambda \sum_{i=1}^{N}X_i\right) = \prod_{i=1}^{N} \mathbb{E}\exp(\lambda X_i)
$$
Since $X_i$ are sub-gaussians with **zero-mean**, we have by [[Sub-gaussian random variables#^7ff2f7|property 5]] that
$$
\prod_{i=1}^{N} \mathbb{E}\exp(\lambda X_i) \leq \prod_{i=1}^{N} \exp(\lambda^2 C \Vert X_i \Vert_{\psi^2}^2) = \exp\left(\lambda^2 C \sum_{i=1}^{N}\Vert X_i \Vert_{\psi^2}^2\right)
$$
for some absolute constant $C$.

Let us call $K^2 = C \sum_{i=1}^{N}\Vert X_i \Vert_{\psi^2}^2$.
This satisfies the [[Sub-gaussian properties#^70f6af|property 5]], and it is sufficient to say that the sum of $X_1, \dots, X_N$ is sub-gaussian itself.

Finally, since $\Vert \sum_{i=1}^{N} X_i \Vert_{\psi^2}$ is the **smallest** value that makes all the properties true (see [[Sub-gaussian random variables#^e7688e|this]]), then we have that
$$
\exp\left(\lambda^2 C \left\Vert \sum_{i=1}^{N} X_i \right\Vert_{\psi^2}^2\right) \leq \exp\left(\lambda^2 C' K^2 \right) =\exp\left(\lambda^2 C' \sum_{i=1}^{N}\Vert X_i \Vert_{\psi^2}^2\right)
$$
therefore
$$
\left\Vert \sum_{i=1}^{N} X_i \right\Vert_{\psi^2}^2 \leq  c  \sum_{i=1}^{N} \Vert X_i \Vert_{\psi^2}^2
$$
for some absolute constant $c$ $\square$.


--------
==da togliere==
$$
\left\Vert \sum_{i=1}^{N} X_i \right\Vert_{\psi^2} \leq\sqrt{ c \sum_{i=1}^{N} \Vert X_i \Vert_{\psi^2}^2} \leq  c'  \sum_{i=1}^{N} \Vert X_i \Vert_{\psi^2}
$$
for some absolute constants $c,c'$ $\square$.

> [!help]
> Observe that
> $$
> x^p + y^p \leq (\vert x \vert + \vert y\vert)^p
> $$
> for every $x,y \in \mathbb{R}$ and $p \geq 0$.
> Therefore
> $$
> \sqrt[p]{x^p+y^p} \leq \vert x \vert + \vert y \vert
> $$

