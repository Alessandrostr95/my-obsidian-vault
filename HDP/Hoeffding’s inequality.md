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
# Hoeffding's inequality, two-sided
Let $X_1, ... , X_N$ be independent **symmetric Bernoulli** random variables, i.e., random binary random variables that assumes value $1$ or $-1$ with probability $1/2$.
Let $a = (a_1, \dots, a_N) \in \mathbb{R}^N$.
Then, for every $t \geq 0$, we have
$$\mathbb{P}\left\lbrace \left\vert\sum_{i=1}^{N}a_iX_i \right\vert \geq t \right\rbrace \leq 2\exp\left( - \frac{t^2}{2 \Vert a \Vert_2^2} \right)$$

#### Proof
Let $S_N = \sum_{i=1}^{N}a_iX_i$.
We can simply apply the Hoeffding’s inequality for the variables $-X_i$ instead of $X_i$, and obtain the same bound for $\mathbb{P}\{-S_N \geq t\}$. Then $$\mathbb{P}\{\vert S_N \vert \geq t\} = \mathbb{P}\{S_N \geq t\} + \mathbb{P}\{- S_N \geq t\} \;\; \square$$

-------

# Hoeffding's inequality for general bounded random variables

Let $X_1, ... , X_N$ be **independent** random variables, with $X_i \in \left[ m_i, M_i \right]$ for every $i$.
Then, for every $t > 0$, we have
$$\mathbb{P}\left\lbrace \sum_{i=1}^{N}(X_i- \mathbb{E}X_i) \geq t\right\rbrace \leq \exp\left( - \frac{2t^2}{\sum_{i=1}^N{}(M_i-m_i)^2} \right)$$

#### Proof #exercise

```ad-warning
WORK IN PROGRESS
```

As before, we multiply by a constant $\lambda > 0$, then exponentiate, then apply Markov's bound, and finally optimize with $\lambda$.

First 
$$\mathbb{P}\left\lbrace \sum_{i=1}^{N}(X_i- \mathbb{E}X_i) \geq t\right\rbrace \leq e^{-\lambda t} \prod_{i=1}^{N}\mathbb{E}e^{\lambda(X_i - \mathbb{E}X_i)}$$

Now, let $f(x) = e^{\lambda x}$, with $x \in \left[ a, b \right]$.
Since $f$ is increasing monotone, we have that $f(a) \leq f(b)$.
Moreovere, $f$ is **convex** then,  for every $0 < \alpha < 1$, we have
$$f(x) = f(\alpha x + (1-\alpha)x) \leq \alpha f(x) + (1-\alpha)f(x) \leq \alpha f(b) + (1-\alpha)f(a)$$
Set $\alpha = \frac{x -a}{b-a} \in (0,1)$.
Then, for every $x \in \left[ a,b \right]$ $$e^{\lambda x} \leq \frac{x - a}{b-a}e^{\lambda b} + \frac{b-x}{b-a}e^{\lambda a}$$

As a consequence, since $\mathbb{E}(X_i - \mathbb{E}X_i) = 0$, we have
$$\begin{align*}
\mathbb{E}e^{\lambda (X_i - \mathbb{E}X_i)}
&\leq \mathbb{E} \left(\frac{\mathbb{E}(X_i - \mathbb{E}X_i) -m_i}{M_i - m_i}e^{\lambda M_i} + \frac{M_i - \mathbb{E}(X_i - \mathbb{E}X_i)}{M_i - m_i}e^{\lambda m_i} \right)\\
&= \frac{\mathbb{E}(X_i - \mathbb{E}X_i) -m_i}{M_i - m_i}e^{\lambda M_i} + \frac{M_i - \mathbb{E}(X_i - \mathbb{E}X_i)}{M_i - m_i}e^{\lambda m_i}\\
&= \frac{-m_i}{M_i - m_i}e^{\lambda M_i} + \frac{M_i}{M_i - m_i}e^{\lambda m_i}\\
&= \frac{M_ie^{\lambda m_i} - m_ie^{\lambda M_i}}{M_i - m_i}
\end{align*}$$



-----
## other things

Let $Z_i = X_i - m_i$.
Since $X_i \in \left[ m_i, M_i \right]$, then $Y_i \in \left[ 0, M_i - m_i \right]$.
Let's compute the [[Some definitions#Variance|variance]] of $Z_i$.
$$\begin{align*}
\text{Var}(Z_i)
&= \mathbb{E}Z_i^2 - (\mathbb{E}Z_i)^2\\
(\text{since } Z_i \leq M_i-m_i ) &\leq \mathbb{E}(Z_i \cdot (M_i-m_i)) -(\mathbb{E}Z_i)^2\\
&= (M_i-m_i) \cdot \mathbb{E}Z_i -(\mathbb{E}Z_i)^2\\
&=\mathbb{E}Z_i \cdot ((M_i-m_i) - \mathbb{E}Z_i)
\end{align*}$$
Observe that the function $x(t - x)$ is a **concave function**, then it's maximum is when $x = t/2$.
Then we have $$\text{Var}(Z_i) \leq \frac{(M_i -m_i)^2}{2} - \frac{(M_i -m_i)^2}{4} = \frac{(M_i - m_i)^2}{4}$$
By properties of the variance we have that $\text{Var}(Z_i) = \text{Var}(X_i - m_i) = \text{Var}(X_i)$, therefore $\text{Var}(X_i) \leq \frac{(M_i-m_i)^2}{4}$.




