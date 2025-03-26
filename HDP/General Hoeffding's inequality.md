---
author: Alessandro Straziota
date: 2025-03-26
---

Let $X_1, \dots, X_n$ be **independent** [[Sub-gaussian random variables|sub-gaussian random variables]], with [[Some definitions#Mean|mean]] $0$.

Then, for every $t \geq 0$ we have
$$
\mathbb{P}\left\lbrace \left\vert \sum_{i=1}^{N}X_i \right\vert \geq t \right\rbrace \leq 2 \exp\left( - \frac{ct^2}{\sum_{i=1}^{N} \Vert X_i \Vert_{\psi_2}^2}\right)
$$

Moreover, for every $a = (a_1, \dots, a_N) \in \mathbb{R}^N$ we have
$$
\mathbb{P}\left\lbrace \left\vert \sum_{i=1}^{N}a_i X_i \right\vert \geq t \right\rbrace \leq 2 \exp\left( - \frac{ct^2}{K^2 \Vert a \Vert_2^2}\right)
$$
where $K = \max_i \Vert X_i \Vert_{\psi_2}$.

#### Proof

Since a [[Sums of independent sub-gaussians]] is a [[Sub-gaussian random variables|sub-gaussian]], by applying [[Sub-gaussian random variables#^fedbd4|property 1]] we have that
$$
\mathbb{P}\left\lbrace \left\vert \sum_{i=1}^{N}X_i \right\vert \geq t \right\rbrace \leq 2 \exp\left( - \frac{ct^2}{\Vert\sum_{i=1}^{N} X_i \Vert_{\psi_2}^2}\right)
$$
Moreover we [[Sums of independent sub-gaussians#^690103|know that]]
$$
\left\Vert \sum_{i=1}^{N}X_i \right\Vert_{\psi^2}^2 \leq C \sum_{i=1}^{N} \Vert X \Vert_{\psi^2}^2
$$
for some absolute constant $C$.
Therefore
$$
\mathbb{P}\left\lbrace \left\vert \sum_{i=1}^{N}X_i \right\vert \geq t \right\rbrace \leq 2 \exp\left( - \frac{ct^2}{\Vert\sum_{i=1}^{N} X_i \Vert_{\psi_2}^2}\right)
\leq 2 \exp\left( - \frac{c' t^2}{\sum_{i=1}^{N} \Vert X_i \Vert_{\psi_2}^2}\right)
$$

This proves the first part of the theorem.

For the second part, we remark that $\Vert \cdot \Vert_{\psi_2}$ is a [[Some definitions#Norm|norm]], therefore
$$
\Vert a_i X_i \Vert_{\psi_2} \leq \vert a_i \vert \cdot \Vert X_i \Vert_{\psi_2}
$$
and thus
$$
\sum_{i=1}^{N}\Vert a_i X_i \Vert_{\psi_2}^{2} \leq \sum_{i=1}^{N}a_i^2 \cdot \Vert X_i \Vert_{\psi_2}^{2}
\leq \Vert a \Vert_2^2 \cdot \max_{i} \Vert X_i \Vert_{\psi_2}^2 \;\;\; \square
$$


