---
author: Alessandro Straziota
date: 2025-03-26
desc: Khintchine's inequality
---
# Khintchine's inequality
Let $X_1, \dots, X_n$ be **independent** [[Sub-gaussian random variables|sub-gaussian random variables]], with [[Some definitions#Mean|mean]] $0$, and $a = (a_1, \dots, a_N) \in \mathbb{R}^N$.

Then, for every $p \in \left[ 2, \infty \right)$ we have that
$$
\left( \sum_{i=1}^{N} a_i^2 \right)^{1/2} \leq \left\Vert \sum_{i=1}^{N} a_iX_i \right\Vert_{L^p} \leq CK\sqrt{p}\left( \sum_{i=1}^{N} a_i^2 \right)^{1/2}
$$
where $K = \max_i \Vert X_i \Vert_{\psi_2}$ and $C$ an absolute constant.

#### Proof #exercise
Since $\sum_{i=1}^N a_i X_i$ is a [[Sums of independent sub-gaussians]] with [[Some definitions#Mean|mean]] 0, this is a sub-gaussian. 
Since $\Vert \cdot \Vert_{\psi_2}$ is a [[Some definitions#Norm|norm]], by [[Sub-gaussian random variables#^6dcdad|property 2]] we have that
$$
\begin{align*}
\left\Vert \sum_{i=1}^{N} a_iX_i \right\Vert_{L^p}
&\leq C\sqrt{p} \left\Vert \sum_{i=1}^{N} a_iX_i \right\Vert_{\psi_2}\\
&= C\sqrt{p}  \sum_{i=1}^{N}\vert a_i \vert \cdot \Vert X_i \Vert_{\psi_2}\\
&\leq C\sqrt{p}  \sum_{i=1}^{N}\vert a_i \vert \cdot K\\
&\leq CK\sqrt{p} \cdot \Vert a \Vert_{L^1}\\
&\leq CK\sqrt{p} \cdot \Vert a \Vert_{L^2} = CK\sqrt{p} \left( \sum_{i=1}^{N} a_i^2\right)^{1/2}
\end{align*}
$$
Where last inequality is a [[Some inequalities#Jensen's inequality|consequence of Jensen's inequality]].


