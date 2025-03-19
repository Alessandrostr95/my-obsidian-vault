---
tags:
  - HDP
date: 2025-03-19
desc: Hoeffding's inequality
---
Let $X_1, ... , X_N$ be independent **symmetric Bernoulli** random variables, i.e., random binary random variables that assumes value $1$ or $-1$ with probability $1/2$.
Let $a = (a_1, \dots, a_N) \in \mathbb{R}^N$.
Then, for every $t \geq 0$, we have
$$\mathbb{P}\left\lbrace \sum_{i=1}^{N}a_iX_i \geq t \right\rbrace \leq \exp\left( - \frac{t^2}{2 \Vert a \Vert_2^2} \right)$$


#### Proof
We start multiplying both sides of the inequality by a value $\lambda>0$ and and exponentiate them (as for the proof of [[Some inequalities#Chebyshev's inequality|Chebyshev's inequality]]), and then we apply [[Some inequalities#Markov's inequality|Markov's inequality]].
$$\begin{align*}
\mathbb{P}\left\lbrace \sum_{i=1}^{N}a_iX_i \geq t \right\rbrace
&= \mathbb{P}\left\lbrace \exp\left(\lambda\sum_{i=1}^{N}a_iX_i \right)\geq \exp(\lambda t) \right\rbrace\\
&\leq \frac{\mathbb{E}\exp\left(\lambda\sum_{i=1}^{N}a_iX_i \right)}{e^{\lambda t}}\\
&= e^{-\lambda t} \mathbb{E} \prod_{i=1}^{N}e^{\lambda a_i X_i}\\
&= e^{-\lambda t} \prod_{i=1}^{N} \mathbb{E} e^{\lambda a_i X_i}
\end{align*}$$ where the last inequality holds by the indipendence of the random variables.

For every $i$, since $X_i \in \{-1, 1\}$, we have that $$\mathbb{E}e^{\lambda a_i X_i} = \frac{e^{\lambda a_i} + e^{-\lambda a_i}}{2} = \cosh(\lambda a_i)$$ (see [hyperbolic functions](https://en.wikipedia.org/wiki/Hyperbolic_functions#Exponential_definitions)).

In can be prooved (see [this](https://en.wikipedia.org/wiki/Hyperbolic_functions#Inequalities)) that $\cosh(x) \leq e^{x^2/2}$, for every $x \in \mathbb{R}$.
Therefore, 
$$\prod_{i=1}^{N} \mathbb{E} e^{\lambda a_i X_i} \leq \prod_{i=1}^{N}e^{\lambda^2 a_i^2/2} = \exp\left( \frac{\lambda^2}{2} \sum_{i=1}^{N}a_i^2\right) = \exp\left( \frac{\lambda^2}{2} \Vert a \Vert_2^2\right)$$
By combining the previous inequality we have
$$\mathbb{P}\left\lbrace \sum_{i=1}^{N}a_iX_i \geq t \right\rbrace \leq \exp\left(-\lambda t + \frac{\lambda^2}{2} \Vert a \Vert_2^2\right)$$
for every $\lambda \geq 0$.

Since the $\exp$ function is *convex*, we can **minimize** it simply by finding the value of $\lambda$ such that the derivative is $0$ (thus providing a better upper bound).
The derivative is $-t + \lambda \Vert a \Vert_2^2$, and it's equal to $0$ when $\lambda = t/\Vert a \Vert_2^2$.
Therefore,
$$\mathbb{P}\left\lbrace \sum_{i=1}^{N}a_iX_i \geq t \right\rbrace \leq \exp\left(-\frac{t^2}{\Vert a \Vert_2^2} + \frac{1}{2}\frac{t^2}{\Vert a \Vert_2^2} \right) = \exp\left(-\frac{t^2}{2\Vert a \Vert_2^2}\right) \;\; \square$$


-----
# Hoeffding’s inequality, two-sided
Let $X_1, ... , X_N$ be independent **symmetric Bernoulli** random variables, i.e., random binary random variables that assumes value $1$ or $-1$ with probability $1/2$.
Let $a = (a_1, \dots, a_N) \in \mathbb{R}^N$.
Then, for every $t \geq 0$, we have
$$\mathbb{P}\left\lbrace \left\vert\sum_{i=1}^{N}a_iX_i \right\vert \geq t \right\rbrace \leq 2\exp\left( - \frac{t^2}{2 \Vert a \Vert_2^2} \right)$$

#### Proof
We can simply apply the Hoeffding’s inequality for the variables $-X_i$ instead of $X_i$, and obtain the same bound for $\mathbb{P}\{-S_N \geq t\}$. Then $$\mathbb{P}\{\vert S_N \vert \geq t\} = \mathbb{P}\{S_N \geq t\} + \mathbb{P}\{- S_N \geq t\} \;\; \square$$

