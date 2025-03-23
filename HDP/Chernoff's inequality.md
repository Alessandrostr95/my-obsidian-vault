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
$$
\mathbb{P}\{ S_N \geq t\} \leq e^{-\mu}\left( \frac{e\mu}{t} \right)^t
$$

### Proof
We use the same approach as for the [[Chernoff's inequality]], passing through the [[Some inequalities#Markov's inequality]].

$$
\begin{align*}
\mathbb{P}\{ S_N \geq t\}
&= \mathbb{P}\left\lbrace e^{\lambda S_N} \geq e^{\lambda t} \right\rbrace
\leq e^{-\lambda t} \mathbb{E}e^{\lambda S_n}\\
&= e^{-\lambda t} \mathbb{E} \prod_{i=1}^{N}e^{\lambda X_i}
= e^{-\lambda t} \prod_{i=1}^{N}\mathbb{E}e^{\lambda X_i}\\
&= e^{-\lambda t}\prod_{i=1}^{N}\left( e^\lambda p_i + (1-p_i) \right)\\
&= e^{-\lambda t}\prod_{i=1}^{N}\left( 1 + (e^\lambda - 1) p_i \right)\\
&\leq e^{-\lambda t}\prod_{i=1}^{N}\exp\left( (e^\lambda - 1) p_i \right)
\end{align*}
$$
where last inequality holds since $1+x \leq e^x$ for every $x \in \mathbb{R}$.

Therefore

$$
\begin{align*}
\mathbb{P}\{ S_N \geq t\}
&\leq e^{-\lambda t}\prod_{i=1}^{N}\exp\left( (e^\lambda - 1) p_i \right)\\
&= e^{-\lambda t}\exp\left( \sum_{i=1}^{N} (e^\lambda - 1) p_i \right)\\
&= e^{-\lambda t}\exp\left(  (e^\lambda - 1) \sum_{i=1}^{N} p_i \right)\\
&= e^{-\lambda t}\exp\left(  (e^\lambda - 1) \mu \right)\\
&= \exp\left( (e^\lambda - 1) \mu  - \lambda t\right)
\end{align*}
$$

Since this function is convex, we can minimize the bound finding the value of $\lambda$ s.t. the first derivative becomes $0$.
By monotonicity, it holds when the first derivative of the $(e^\lambda - 1) \mu  - \lambda t$ is $0$, i.e., when
$$
e^\lambda \mu - t = 0 \iff \lambda = \ln \frac{t}{\mu}
$$

Finally
$$
\begin{align*}
\mathbb{P}\{ S_N \geq t\}
&\leq \exp\left( \left(\frac{t}{\mu} - 1 \right) \mu  - t\ln\frac{t}{\mu}\right)\\
&= \exp\left( t- \mu  - t\ln\frac{t}{\mu}\right)\\
&= e^t e^{-\mu}\left( \frac{t}{\mu}\right)^t\\
&= e^{-\mu}\left( \frac{e t}{\mu}\right)^t\\
\end{align*}
$$



-------

### Chernoff's inequality: lower tails #exercise 
$$
\mathbb{P}\{S_N \leq t\} \leq e^{-\mu}\left( \frac{e\mu}{t} \right)^t
$$

### Poisson tails #exercise 
Let $X \sim \text{Pois}(\lambda)$. Show that for any $t > \lambda$
$$
\mathbb{P}\{X \geq t\} \leq e^{-\lambda}\left( \frac{e\lambda}{t}\right)^t
$$

### Chernoff's inequality: small deviations #exercise
Let $X_i$ be $N$ independent Bernoulli random variables with parameters $p_i$.
Let $S_N = X_1 + \dots + X_N$, with [[Some definitions#Mean|mean]] $\mu = \mathbb{E}S_N$.

Then, for every $\delta \in \left(0, 1 \right]$ we have
$$
\mathbb{P}\{\vert S_N - \mu \vert \geq \delta \mu\} \leq 2e^{-c\mu \delta^2}
$$
where $c>0$ is an absolute constant.

#### Proof
We can re-write the probability
$$
\begin{align*}
\mathbb{P}\{\vert S_N - \mu \vert \geq \delta \mu\}
&= \mathbb{P}\{S_N - \mu \geq \delta \mu\} + \mathbb{P}\{S_N - \mu \leq  - \delta \mu\}\\
&= \mathbb{P}\{S_N \geq (1 + \delta) \mu\} + \mathbb{P}\{S_N \leq  (1- \delta) \mu\}\\\\
&\leq e^{-\mu}\left( \frac{e\mu}{\mu(1+\delta)} \right)^{\mu(1+\delta)} + e^{-\mu}\left( \frac{e\mu}{\mu(1-\delta)} \right)^{\mu(1-\delta)}\\
&= e^{-\mu}\left[ \left( \frac{e}{1+\delta} \right)^{1+\delta}\right]^{\mu} + e^{-\mu}\left[ \left( \frac{e}{1-\delta} \right)^{1-\delta} \right]^{\mu}\\
&= \left[e^{-1} \cdot \left( \frac{e}{1+\delta} \right)^{1+\delta}\right]^{\mu} + \left[e^{-1} \cdot  \left( \frac{e}{1-\delta} \right)^{1-\delta} \right]^{\mu}\\
&= \left( \frac{e^{\delta}}{(1+\delta)^{1+\delta}} \right)^{\mu} + \left( \frac{e^{-\delta}}{(1-\delta)^{1-\delta}} \right)^{\mu}\\
&= p_1 + p_2
\end{align*}
$$

We want now to *maximize* $p_1$ and $p_2$, and this is equivalent to maximize $\log{p_1} = \mu(\delta - (1+\delta)\log{(1+\delta)})$ and $\log{p_2} = \mu(- \delta - (1-\delta)\log{(1-\delta)})$.

> [!note]
> See [here](https://en.wikipedia.org/wiki/List_of_logarithmic_identities#Inequalities).
> $$
> \frac{x}{1+\frac{x}{2}} \leq \log{(1+x)} \leq \frac{x+\frac{x^2}{2}}{1+x}
> $$
> And
> $$
> \log{(1-x)} \geq \frac{\frac{x^2}{2}-x}{1-x}
> $$

Since $\log{(1+x)} \geq \frac{x}{1+x/2}$, then
$$
\begin{align*}
\log{p_1}
&\leq \mu \left( \delta - \frac{(1+\delta)\cdot \delta}{1+\delta/2} \right)\\
&= \delta\mu \left( 1 - \frac{(1+\delta)}{1+\delta/2} \right)\\
&= \delta\mu\left( - \frac{\delta/2}{1+\delta/2} \right)\\
(\delta \leq 1) &\leq \delta\mu\left( - \frac{\delta/2}{3/2} \right)\\
&= - \frac{\mu\delta^2}{3}
\end{align*}
$$

Since $\log{(1-x)} \geq \frac{x^2/2 -x}{1-x}$, then
$$
\begin{align*}
\log{p_2}
&\leq \mu \left( -\delta - \frac{(1-\delta)\cdot (\delta^2/2 - \delta)}{1-\delta} \right)\\
&\leq \mu \left( -\delta - \delta^2/2 + \delta \right)\\
&= -\frac{\mu\delta^2}{2}
\end{align*}
$$

Finally
$$
\begin{align*}
\mathbb{P}\{\vert S_N - \mu \vert \geq \delta \mu\}
&\leq p_1 + p_2\\
&\leq e^{-\mu\delta^2/3} + e^{-\mu\delta^2/2}\\
&\leq 2e^{-\mu\delta^2/2} \;\;\; \square
\end{align*}
$$

