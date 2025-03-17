---
tags:
  - HDP
date: 2025-03-17
desc: some classical inequalities for high dimensional probability
---

### Jensen's inequality
Let $f: \mathbb{R} \to \mathbb{R}$ be a **convex** function, i.e., such that $f(\lambda x + (1-\lambda)y) \leq \lambda f(x) + (1-\lambda)f(y)$, for each $x,y \in \mathbb{R}$ and $\lambda \in \left[ 0,1\right]$.
Then, for every random variable $X$ we have that $$f(\mathbb{E}X) \leq \mathbb{E}f(X)$$

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
$$\vert \mathbb{E} XY \vert \leq \Vert X \Vert_{L^2} \cdot \Vert Y \Vert_{L^2}$$


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


#### Generalization of integral identity
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


### $p$-moments via tails
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

> **Proof**
> By using [[#Markov's inequality]] on the non-negative random variable $(X - \mathbb{E}X)^2$ we have $$\mathbb{P}\{\vert X - \mathbb{E}X\vert \geq t\} = \mathbb{P}\{(X - \mathbb{E}X)^2 \geq t^2\} \leq \frac{\mathbb{E}(X-\mathbb{E}X)^2}{t^2} = \frac{\text{Var}(X)}{t^2} \;\; \square$$




