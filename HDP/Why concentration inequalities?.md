---
tags:
  - HDP
date: 2025-03-17
---

Toss a fair coin $N$ times, and assume you want to compute the probability that we get *at least* $\frac{3}{4}N$ haeds.

Let $S_N = X_1 + ... + X_N$ the number of heads, where $X_i$ is the result of the $i$-th toss.
Then $$\mathbb{E}S_N = \frac{N}{2}, \;\;\; \text{Var}(S_N) = \frac{N}{4}$$

Using [[Some inequalities#Chebyshev's inequality|Chebyshev's inequality]] we have
$$\mathbb{P}\left\lbrace S_N \geq \frac{3}{4}N \right\rbrace = \mathbb{P}\left\lbrace S_N - \frac{N}{2} \geq \frac{1}{4}N \right\rbrace \leq \mathbb{P}\left\lbrace \left\vert S_N - \frac{N}{2} \right\vert \geq \frac{1}{4}N \right\rbrace \leq \frac{4}{N}$$

So the probability converges to zero at least linearly in $N$.

We can obtain a better bound using [[Some inequalities#de Moivre-Laplace central limit theorem|CTL]], i.e., we can *approximate* the probability using normal random variables.
$$\begin{align*}
\mathbb{P}\left\lbrace S_N \geq \frac{3}{4}N \right\rbrace
&= \mathbb{P}\left\lbrace S_N - \frac{N}{2}\geq \frac{1}{4}N \right\rbrace\\
&= \mathbb{P}\left\lbrace \frac{S_N - \frac{N}{2}}{\sqrt{N/4}}\geq \sqrt{N/4} \right\rbrace \approx \mathbb{P}\left\lbrace g \geq \sqrt{N/4} \right\rbrace
\end{align*}$$
where $g \sim N(0,1)$.
It can be proved that this probability decrease **exponentially** in $N$, in particular $$\mathbb{P}\left\lbrace g \geq \sqrt{N/4} \right\rbrace \leq \frac{1}{\sqrt{2\pi}}e^{-N/8}$$

> **Tails of the normal distribution**
> Let $g \sim N(0,1)$. Then, for every $t > 0$ we have $$\left(\frac{1}{t} - \frac{1}{t^3} \right)\phi(t)\leq \mathbb{P}\left\lbrace g \geq t \right\rbrace \leq \frac{1}{t}\phi(t)$$ where $$\phi(t) = \frac{1}{\sqrt{2\pi}}e^{-t^2/2}$$
> In particular, when $t \geq 1$, we have that $$\mathbb{P}\left\lbrace g \geq t \right\rbrace \leq \frac{1}{\sqrt{2\pi}}e^{-t^2/2}$$


