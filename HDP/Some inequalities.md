---
tags:
  - HDP
date: 2025-03-17
desc: some classical inequalities for high dimensional probability
---
### Jensen's inequality
Let $f: \mathbb{R} \to \mathbb{R}$ be a **convex** function, i.e., such that $f(\lambda x + (1-\lambda)y) \leq \lambda f(x) + (1-\lambda)f(y)$, for each $x,y \in \mathbb{R}$ and $\lambda \in \left[ 0,1\right]$.
Then, for every random variable $X$ we have that $$f(\mathbb{E}X) \leq \mathbb{E}f(X)$$

In the same way, when $f$ is **concave**, we have that $$f(\mathbb{E}X) \geq \mathbb{E}f(X)$$

As a consequence, the [[Some definitions#$L p$ norm of a random variable|$L^p$ norm]] of $X$ is **increasing** in $p$, i.e.
$$\Vert X \Vert_{L^p} \leq \Vert X \Vert_{L^q},\;\; \text{for any } 0 < p \leq p \leq \infty$$

> **Proof**
> Since $\vert X \vert^{q/p}$ is **convex** (because $q/p > 1$), we have
> $$\Vert X \Vert_{L^p} = (\mathbb{E}\vert X \vert^p)^{1/p} = ((\mathbb{E}\vert X \vert^p)^{q/p})^{1/q} \leq (\mathbb{E}\vert X \vert^{q})^{1/q} = \Vert X \Vert_{L_q} \;\;\square$$
>

### Minkowski's inequality
For any $p \in \left[ 1, \infty \right]$, for any random variable $X,Y \in L^p$ we have that
$$\Vert X + Y \Vert_{L^p} \leq \Vert X \Vert_{L^p} + \Vert Y \Vert_{L^p}$$

### Cauchy-Schwarz inequality
For any random variable $X,Y \in L^2$, we have that
$$\vert \mathbb{E} XY \vert \leq \Vert X \Vert_{L^2} \cdot \Vert Y \Vert_{L^2} = \sqrt{\mathbb{E}X^2 \cdot \mathbb{E}Y^2}$$
In particular $$\vert \mathbb{E}X\vert \leq \sqrt{\mathbb{E}X^2}$$ when $Y=1$.


#### Holder’s inequality
This is a more general version of the Cauchy-Scwartz inequality.
Let $p,q \in (1, \infty)$ be two *conjugate exponents*, i.e., such that $1/p + 1/q = 1$.
Then for every random variable $X \in L^p$ and $Y \in L^q$, we have
$$\vert \mathbb{E} XY \vert \leq \Vert X \Vert_{L^p} \cdot \Vert Y \Vert_{L^q}$$
This inequality hold even when $p = 1$ and $q = \infty$.

### Integral Identity
Let $X \in \mathbb{R}^+$ be a **non-negative** random variable, then $$\mathbb{E}X = \int_{0}^{\infty}\mathbb{P}\{X>t\}\,dt$$
> **Proof**
> We can represent any non-negative random variable $x$ with the integral $$x = \int_{0}^x\,dt = \int_{0}^{\infty}\mathbb{1}\{x > t\} \,dt$$
> Therefore
> $$\begin{align*}
> \mathbb{E}X
> &= \mathbb{E}\int_{0}^{\infty}\mathbb{1}\{X > t\}\, dt\\
> &= \int_{0}^{\infty}\mathbb{E}\mathbb{1}\{X > t\}\, dt\\
> &= \int_{0}^{\infty} \mathbb{P}\{X > t\} \,dt \;\; \square
> \end{align*}$$


#### Generalization of integral identity #exercise
Let $X$ be a real random variable (not necessarily non-negative).
Then $$\mathbb{E}X = \int_{0}^{\infty}\mathbb{P}\{X > t\}\,dt - \int_{-\infty}^{0}\mathbb{P}\{X < t\}\,dt$$

> **Proof**
> We can represent any non-negative random variable $x$ with the integral $$x = \int_{0}^x\,dt - \int_{x}^0\,dt = \int_{0}^{\infty}\mathbb{1}\{x > t\} \,dt - \int_{-\infty}^{0}\mathbb{1}\{x < t\} \,dt$$
> Therefore
> $$\begin{align*}
> \mathbb{E}X
> &= \mathbb{E} \left(\int_{0}^{\infty}\mathbb{1}\{X > t\} \,dt - \int_{-\infty}^{0}\mathbb{1}\{X < t\} \,dt \right)\\
> &= \int_{0}^{\infty}\mathbb{E}\mathbb{1}\{X > t\}\, dt - \int_{-\infty}^{0}\mathbb{E}\mathbb{1}\{X < t\}\, dt\\
> &= \int_{0}^{\infty} \mathbb{P}\{X > t\} \,dt - \int_{-\infty}^{0} \mathbb{P}\{X < t\} \,dt \;\; \square
> \end{align*}$$


### $p$-moments via tails #exercise
Let $X$ be a random variable and $p \in (0, \infty)$.
Then $$\mathbb{E}\vert X \vert^p = \int_{0}^{\infty}pt^{p-1}\mathbb{P}\{\vert X \vert > t\}\, dt$$
> **Proof**
> By [[#Integral Identity|integral inequality]] we have that $$\mathbb{E}|X|^p = \int_{0}^{\infty}\mathbb{P}\{|X|^p \geq u\}\,du$$
> Set $t = g(u) = u^{1/p}$, or equivalently $u = g^{-1}(t) = t^p$.
> By changing the base, we have that
> $$\frac{du}{dt} = \frac{d}{dt}g^{-1}(t) = \frac{d}{dt}t^p = pt^{p-1} \implies du = pt^{p-1} dt$$
> Therefore
> $$\begin{align}
> \mathbb{E}|X|^p
> &= \int_{0}^{\infty}\mathbb{P}\{|X|^p \geq u\}\,du\\
> &= \int_{0}^{\infty}pt^{p-1}\mathbb{P}(\vert X \vert \geq u^{1/p})dt\\
> &= \int_{0}^{\infty}pt^{p-1}\mathbb{P}(\vert X \vert \geq t)dt\;\; \square
> \end{align}$$


### Markov's inequality
For any **non-negative** random variable $X$ and $t > 0$, we have $$\mathbb{P}\{X \geq t\} \leq \frac{\mathbb{E}X}{t}$$

> **Proof**
> Fix any $t > 0$.
> We can represent any real number as $$x = x \cdot \mathbb{1}\{x \geq t\} + x \cdot \mathbb{1}\{x < t\}$$
> Then we have
> $$\begin{align*}
> \mathbb{E}X
> &= \mathbb{E}(X\cdot\mathbb{1}\{X \geq t\}) + \mathbb{E}(X\cdot\mathbb{1}\{X < t\})\\
> &\geq \mathbb{E}(t\cdot\mathbb{1}\{X \geq t\})\\
> &= t\cdot \mathbb{E}\mathbb{1}\{X \geq t\} =  t \cdot \mathbb{P}\{X \geq t\} \;\; \square
> \end{align*}$$

### Chebyshev's inequality
Let $X$ any random variable.
Then, for every $t > 0$ we have
$$\mathbb{P}\{\vert X - \mathbb{E}X\vert \geq t \} \leq \frac{\text{Var}(X)}{t^2}$$

> **Proof** #exercise
> By using [[#Markov's inequality]] on the non-negative random variable $(X - \mathbb{E}X)^2$ we have $$\mathbb{P}\{\vert X - \mathbb{E}X\vert \geq t\} = \mathbb{P}\{(X - \mathbb{E}X)^2 \geq t^2\} \leq \frac{\mathbb{E}(X-\mathbb{E}X)^2}{t^2} = \frac{\text{Var}(X)}{t^2} \;\; \square$$



------

### Strong law of large numbers
Let $X_1, X_2, \dots$ be a sequence of independent and identical distributed random variables with mean $\mu$.
Let $S_N = S_1+...+S_N$ then [[Convergence of random variables#Almost sure convergence|almost surely]] $$S_N/N \xrightarrow{a.s.} \mu$$

### Lindeberg-Lévy central limit theorem
Let $X_1, X_2, \dots$ be a sequence of independent and identical distributed random variables with [[Some definitions#Mean|mean]] $\mu$ and finite [[Some definitions#Variance|variance]] $\sigma^2 < \infty$.
Let $S_N = S_1 + \dots + S_N$, and define the random variable $$Z_N = \frac{S_N - \mathbb{E}S_N}{\sqrt{\text{Var}(S_N)}} = \frac{1}{\sigma\sqrt{N}}\sum_{i=1}^{N}(X_i - \mu)$$
Then $Z_N$ [[Convergence of random variables#Convergence in distribution|converges in distribution]] to the **norma distribution** $N(0,1)$, i.e.,
$$Z_N \xrightarrow{d}N(0,1)$$
$$\lim_{N \to \infty} \mathbb{P}\{Z_N \geq t\} = \Phi(t) = \frac{1}{\sqrt{2\pi}}\int_{t}^{\infty} e^{-x^2/2}\,dx$$
#### Alternative version
Let $$Z_N = \frac{S_N/N - \mu}{\sqrt{\text{Var}(S_N/N)}} = \frac{S_N/N - \mu}{\sigma\sqrt{1/N}} = \sqrt{N} \frac{S_N/N - \mu}{\sigma}$$
Then $Z_N$ [[Convergence of random variables#Convergence in distribution|converges in distribution]] to the **norma distribution** $N(0,1)$.
It follow that $$\sqrt{N}\left( S_N/N - \mu\right) \xrightarrow{d}N(0, \sigma^2)$$
since $Z_N \xrightarrow{d} N(0,1)$, then $\sigma Z_N$ as varince $\sigma^2$ as $N \to \infty$, in fact
$$\text{Var}(\sigma Z) = \sigma^2\text{Var}(Z_N) \to \sigma^2\cdot 1$$


#### Exercise 1.3.3 #exercise
Let $X_1, X_2, ...$ be a sequence of i.i.d. random variables with mean $\mu$ and finite variance $\sigma^2$. Show that $$\mathbb{E} \left\vert \frac{1}{N}\sum_{i=1}^{N}X_i - \mu \right\vert = O\left( \frac{1}{\sqrt{N}} \right)$$ as $N \to \infty$.

> By [[#Jensen's inequality]] we have that $$\mathbb{E} \left\vert \frac{1}{N}\sum_{i=1}^{N}X_i - \mu \right\vert \leq \left\vert \mathbb{E} \frac{1}{N}\sum_{i=1}^{N}X_i - \mu \right\vert$$
> By [[#Cauchy-Schwarz inequality]] we have that
> $$\begin{align*}
> \left\vert \mathbb{E} \frac{1}{N}\sum_{i=1}^{N}X_i - \mu \right\vert
> &\leq \sqrt{\mathbb{E} \left( \frac{1}{N}\sum_{i=1}^{N}X_i - \mu \right)^2}\\
> &= \sqrt{\text{Var}\left(\frac{1}{N}\sum_{i=1}^{N}X_i\right)}\\
> &= \sqrt{\frac{\sigma^2}{N}} = \frac{\sigma}{\sqrt{N}}
> \end{align*}$$
> Since $\sigma$ is a finite fixed value independent from $N$, whe have that $$\frac{\sigma}{\sqrt{N}} = O\left( \frac{1}{\sqrt{N}} \right) \;\; \square$$
> 

### de Moivre-Laplace central limit theorem
Let $X_1, X_2, \dots$ a sequence of **bernoulli** variables with parameter $p$.
Recall that $$\mathbb{E}X_i = p,\;\;\; \text{Var}(X_i) = p(1-p)$$
and that the sum $S_N = X_1 + ... + X_N$ has **binomial distribution** $\text{Bin}(N,p)$ with $$\mathbb{E}S_N = Np,\;\;\; \text{Var}(S_N) = Np(1-p)$$

Then $$\frac{S_N - Np}{\sqrt{Np(1-p)}} \xrightarrow{d} N(0,1)$$

### Poisson Limit Theorem
Let $X_1, X_2, \dots$ a sequence of **i.i.d.** random variables $X_i \sim \text{Ver}(p_i)$.
Let $S_N = X_1 + \dots + X_N$, with mean $$\mathbb{E}S_N = \sum_{i=1}^{N} p_i$$
Assume that as $N \to \infty$ the probabilities $p_i$ are s.t. the mean $\mathbb{E}S_N$ is **constant** independent from $N$, say $\mathbb{E}S_N \to \lambda$, then $$S_N \xrightarrow{d} \text{Pois}(\lambda)$$ 
Remark that $\text{Pois}(\lambda)$ is the **Poisson distribution**, and $Z \sim \text{Pois}(\lambda)$ we have $$\mathbb{P}\{Z = k\} = e^{-\lambda}\frac{\lambda^k}{k!},\;\; \forall k \in \mathbb{N}$$

This theorem shows that if we have a very large number of independent trials ($N$), and the probability of success in each trial ($p_i$) is very small but such that the expected number of successes $\sum_{i=1}^{N}p_i$​ remains constant, then the total number of successes approximately follows a *Poisson distribution*.

### Berry-Esseen Central Limit Theorem
Let $X_1, X_2, \dots$ be a sequence of independent and identical distributed random variables with [[Some definitions#Mean|mean]] $\mu$ and finite [[Some definitions#Variance|variance]] $\sigma^2 < \infty$.
Let $S_N = S_1 + \dots + S_N$, and define the random variable $$Z_N = \frac{S_N - \mathbb{E}S_N}{\sqrt{\text{Var}(S_N)}} = \frac{1}{\sigma\sqrt{N}}\sum_{i=1}^{N}(X_i - \mu)$$

For **every** $N$ and **every** $t \in \mathbb{R}$, we have that
$$\vert \mathbb{P}\{Z_n \geq t\} - \mathbb{P}\{g \geq t\}\vert \leq \frac{\rho}{\sqrt{N}}$$
where $g \sim N(0,1)$, and $\rho = \mathbb{E}\vert X_1 - \mu \vert^3/\sigma^3$.





