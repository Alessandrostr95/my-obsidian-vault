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
So the probability converges to zero at least linearly in $N$. ^e10133

We can obtain a better bound using [[Some inequalities#de Moivre-Laplace central limit theorem|CTL]], i.e., we can *approximate* the probability using normal random variables.
$$\begin{align*}
\mathbb{P}\left\lbrace S_N \geq \frac{3}{4}N \right\rbrace
&= \mathbb{P}\left\lbrace S_N - \frac{N}{2}\geq \frac{1}{4}N \right\rbrace\\
&= \mathbb{P}\left\lbrace \frac{S_N - \frac{N}{2}}{\sqrt{N/4}}\geq \sqrt{N/4} \right\rbrace \approx \mathbb{P}\left\lbrace g \geq \sqrt{N/4} \right\rbrace
\end{align*}$$
where $g \sim N(0,1)$.
It can be [[#^e0aac6|proved]] that this probability decrease **exponentially** in $N$, in particular $$\mathbb{P}\left\lbrace g \geq \sqrt{N/4} \right\rbrace \leq \frac{1}{\sqrt{2\pi}}e^{-N/8}$$
better then the linear decay [[#^e10133|obtained before]] using [[Some inequalities#Chebyshev's inequality|Chebyshev's inequality]]. ^5bba0e

> **Tails of the normal distribution**
> Let $g \sim N(0,1)$. Then, for every $t > 0$ we have $$\left(\frac{1}{t} - \frac{1}{t^3} \right)\phi(t)\leq \mathbb{P}\left\lbrace g \geq t \right\rbrace \leq \frac{1}{t}\phi(t)$$ where $$\phi(t) = \frac{1}{\sqrt{2\pi}}e^{-t^2/2}$$
> In particular, when $t \geq 1$, we have that $$\mathbb{P}\left\lbrace g \geq t \right\rbrace \leq \frac{1}{\sqrt{2\pi}}e^{-t^2/2}$$

^e0aac6

Since the [[#^5bba0e|previous bound]] is obtained using [[Some inequalities#de Moivre-Laplace central limit theorem|CTL]], it holds asymptotically, thus we must consider the **error of approximation**.
By  [[Some inequalities#Berry-Esseen Central Limit Theorem]], we can see that the error decays too slow, even **slower than linearly** in $N$, more precisely the error is of the order of $1/\sqrt{N}$.

> **Example**
> For example, if $N$ is even, the probability that $S_N$ is $N/2$ is $\Theta(1/\sqrt{N})$ (using [Stirling's Approximation](https://en.wikipedia.org/wiki/Stirling%27s_approximation)).
> Instead, the probability that a normal $g \sim N(0,1)$ its equal to its mean is $0$.
> Thus approximating $S_N$ with a normal random variable has error of $\Theta(1/\sqrt{N})$.


```ad-summary
-  [[Some inequalities#Chebyshev's inequality|Chebyshev's inequality]] does not offer a tight bound for $S_N$, since the probability decay lineary in $N$.
- [[Some inequalities#de Moivre-Laplace central limit theorem|CTL]] offers a good asympototic approximation of $S_N$, since it decay *exponentially* in $N$.
- At the same time, the error of approximation in central limit theorem decays too slow, even slower than linear.
```


> **Exercise 2.1.4 (Truncated normal distribution)**. #exercise
> Let $g \sim N(0,1)$. Show that, for each $t \geq 1$, we have
> $$\mathbb{E}g^2\mathbb{1}\{g > t\} = t \frac{1}{\sqrt{2\pi}}e^{-t^2/2} + \mathbb{P}\{g > t\} \leq \left(t + \frac{1}{t} \right)\frac{1}{\sqrt{2\pi}}e^{-t^2/2}$$
> **Solution**
> $$\begin{align*}
> \mathbb{E}g^2\mathbb{1}\{g > t\}
> &= \int_{t}^{\infty}x^2\phi(x)\,dx = \int_{t}^{\infty}x^2\frac{1}{\sqrt{2\pi}}e^{-x^2/2}\,dx\\
> &= t\frac{1}{\sqrt{2\pi}}e^{-t^2/2} + \int_{t}^{\infty}\frac{1}{\sqrt{2\pi}}e^{-x^2/2}\,dx\\
> &= t\frac{1}{\sqrt{2\pi}}e^{-t^2/2} + \mathbb{P}\{g > t\}
> \leq \left(t + \frac{1}{t} \right)\frac{1}{\sqrt{2\pi}}e^{-t^2/2} \;\; \square
> \end{align*}$$


