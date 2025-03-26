---
author: Alessandro Straziota
date: 2025-03-26
---

In many cases, as [[General Hoeffding's inequality]], we assumed [[Sub-gaussian random variables]] with [[Some definitions#Mean|mean]] $0$.

We can generalize all the results for sub-gaussians, even when the mean is not $0$.

> [!lemma] Centering
> If $X$ is a [[Sub-gaussian random variables]] then $X - \mathbb{E}X$ is sub-gaussian with mean $0$, and 
> $$
> \Vert X - \mathbb{E}X\Vert_{\psi_2} \leq C\Vert X \Vert_{\psi_2}
> $$
> where $C$ is an absolute constant.


#### Proof
We first [[Sub-gaussian random variables#^ceb40a|recall]] that $\Vert \cdot \Vert_{\psi_2}$ is [[Some definitions#Norm|norm]].
Then we can use triangular inequality
$$
\Vert X - \mathbb{E}X\Vert_{\psi_2} \leq \Vert X \Vert_{\psi_2} + \Vert \mathbb{E}X\Vert_{\psi_2}
$$

It is easy to see that, for every **constant** $a$, we have $\Vert a \Vert_{\psi_2} \leq c \vert a \vert$, where $c$ is an absolute constant.

By [[Some inequalities#Jensen's inequality]] we have
$$
\Vert \mathbb{E}X\Vert_{\psi_2} \leq C \vert \mathbb{E}X\vert \leq C \mathbb{E}\vert X\vert = C \Vert X \Vert_{L^1}
$$

Finally, using [[Sub-gaussian random variables#^6dcdad|property 2]], we have 
$$
C \Vert X \Vert_{L^1} \leq C' \Vert X \Vert_{\psi_2}
$$
$$
\implies \Vert X - \mathbb{E}X\Vert_{\psi_2} \leq \Vert X \Vert_{\psi_2} + \Vert \mathbb{E}X\Vert_{\psi_2} \leq (1+C')\Vert X \Vert_{\psi_2} \;\;\; \square
$$

