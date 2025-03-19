---
tags:
  - HDP
date: 2925-03-19
desc: Chernoff's inequality
---
The [[Hoeffding’s inequality]] is not **sensitive** to the magnitude of the probabilities $p_i$ of the bernoulli random variables $X_i$.
In fact, when $p_i$ are that small such that $S_N = X_1 +\dots+X_N$ is [[Some inequalities#Poisson Limit Theorem|approximately a Poisson]], the *Gaussian* tail of the form $e^{-t^2/2}$  is far from the real *Poisson* tail, that is of the form $(\lambda/t)^t$.

# Chernoff's inequality
Let $X_i$ be $N$ independent Bernoulli random variables with parameters $p_i$.
Let $S_N = X_1 + \dots + X_N$, with [[Some definitions#Mean|mean]] $\mu = \mathbb{E}S_N$.

Then, for every value $t > \mu$, we have
$$\mathbb{P}\{ S_N \geq t\} \leq e^{-\mu}\left( \frac{e\mu}{t} \right)^t$$

### Proof
We use the same approach as for the [[Chernoff's inequality]], passing through the [[Some inequalities#Markov's inequality]].

$$\begin{align*}
\mathbb{P}\{ S_N \geq t\}
&= \mathbb{P}\left\lbrace e^{\lambda S_N} \geq e^{\lambda t} \right\rbrace
\leq e^{-\lambda t} \mathbb{E}e^{\lambda S_n}\\
&= e^{-\lambda t} \mathbb{E} \prod_{i=1}^{N}e^{\lambda X_i}
= e^{-\lambda t} \prod_{i=1}^{N}\mathbb{E}e^{\lambda X_i}\\
&= e^{-\lambda t}\prod_{i=1}^{N}\left( e^\lambda p_i + (1-p_i) \right)\\
&= e^{-\lambda t}\prod_{i=1}^{N}\left( 1 + (e^\lambda - 1) p_i \right)\\
&\leq e^{-\lambda t}\prod_{i=1}^{N}\exp\left( (e^\lambda - 1) p_i \right)
\end{align*}$$
where last inequality holds since $1+x \leq e^x$ for every $x \in \mathbb{R}$.

Therefore

$$\begin{align*}
\mathbb{P}\{ S_N \geq t\}
&\leq e^{-\lambda t}\prod_{i=1}^{N}\exp\left( (e^\lambda - 1) p_i \right)\\
&= e^{-\lambda t}\exp\left( \sum_{i=1}^{N} (e^\lambda - 1) p_i \right)\\
&= e^{-\lambda t}\exp\left(  (e^\lambda - 1) \sum_{i=1}^{N} p_i \right)\\
&= e^{-\lambda t}\exp\left(  (e^\lambda - 1) \mu \right)\\
&= \exp\left( (e^\lambda - 1) \mu  - \lambda t\right)
\end{align*}$$

Since this function is convex, we can minimize the bound finding the value of $\lambda$ s.t. the first derivative becomes $0$.
By monotonicity, it holds when the first derivative of the $(e^\lambda - 1) \mu  - \lambda t$ is $0$, i.e., when
$$e^\lambda \mu - t = 0 \iff \lambda = \ln \frac{t}{\mu}$$

Finally
$$\begin{align*}
\mathbb{P}\{ S_N \geq t\}
&\leq \exp\left( \left(\frac{t}{\mu} - 1 \right) \mu  - t\ln\frac{t}{\mu}\right)\\
&= \exp\left( t- \mu  - t\ln\frac{t}{\mu}\right)\\
&= e^t e^{-\mu}\left( \frac{t}{\mu}\right)^t\\
&= e^{-\mu}\left( \frac{e t}{\mu}\right)^t\\
\end{align*}$$



-------

### Chernoff's inequality: lower tails #exercise 

### Poisson tails #exercise 

### Chernoff's inequality: small deviations #exercise

