---
tags:
  - HDP
date: 2025-09-19
---
# Sub-gaussian properties
Let $X$ be any random variable.
Then the following properties are equivalent

1. There exists $K_1 > 0$ such that the tails of $X$ satisfy $$\mathbb{P}\{\vert X \vert \geq t\} \leq 2 \exp(-t^2/K_1^2),\;\;\; \forall t \geq 0$$ ^b5dd62
2. There exists $K_2 > 0$ such that the [[Some definitions#$L p$ norm of a random variable|moments]] of $X$ satisfy $$\Vert X \Vert_{L^p} = (\mathbb{E}\vert X \vert^p)^{1/p} \leq K_2\sqrt{p},\;\;\; \forall p \geq 1$$ ^51312a
3. There exists $K_3 > 0$ such that the [[Some definitions#MGF - Moment Generating Function|MGF]] of $X^2$ satisfies $$M_{X^2}(\lambda^2) =\mathbb{E}\exp(\lambda^2 X^2) \leq \exp(K_3^2\lambda^2),\;\;\; \forall \lambda : \vert \lambda\vert \leq \frac{1}{K_3}$$ ^32045f
4. There exists $K_4 > 0$ such that the [[Some definitions#MGF - Moment Generating Function|MGF]] of $X^2$ is **bounded** at some point, namely $$M_{X^2}(1 / K_4^2) =\mathbb{E}\exp(X^2 / K_4^2) \leq 2$$
Moreover, if $\mathbb{E}X = 0$ we have another equivalent property ^51fe83
5. There exists $K_5 > 0$ such that the [[Some definitions#MGF - Moment Generating Function|MGF]] of $X$ satisfies $$M_X(\lambda) = \mathbb{E}\exp(\lambda X) \leq \exp(K_5^2\lambda^2),\;\;\; \forall \lambda \in \mathbb{R}$$ ^70f6af


```ad-important
The parameters $K_i > 0$ appearing in these properties differ from each other by at most an absolute constant factor.
I.e., there exists an **absolute constant** $C$ such that property $i$ implies property $j$ with parameter $K_j \leq CK_i$ for any two properties $i, j = 1, ... , 5$.
```

^47bb35


### Proof
#### $(1 \implies 2)$

Assume that [[#^b5dd62|property 1]] holds.
Without loss of generality, we can assume $K_1 = 1$.

> [!tldr]
> In fact, by scaling $X$ as $X/K_1$, we have
> $$
> \mathbb{P}\left\lbrace \left\vert \frac{X}{K_1} \right\vert\geq t \right\rbrace = \mathbb{P}\{\vert X \vert \geq tK_1\} \leq 2 \exp(- (tK_1)^2/K_1) = 2\exp(-t^2)
> $$

Remark that
$$
\Gamma(x) := \int_{0}^{\infty}t^{x-1}e^{-t} \, dt
$$
 
By applying [[Some inequalities#$p$-moments via tails exercise]] we have that
$$
\begin{align*}
\mathbb{E}\vert X \vert^2
&= \int_{0}^{\infty}pt^{p-1} \mathbb{P}\{\vert X \vert > t\}\,dt\\
&\leq \int_{0}^{\infty}pt^{p-1} 2e^{-t^2}\,dt\\
&= \int_{0}^{\infty}pt^{p-1} 2e^{-t^2}\,dt\\
(\text{by } s=t^2)&= \int_{0}^{\infty} p s^{(p-1)/2}2e^{-s}\frac{1}{2\sqrt{s}} \, ds\\
&= \int_{0}^{\infty} p s^{p/2 -1}e^{-s}\, ds\\
&= p\int_{0}^{\infty} s^{p/2-1}e^{-s} \, ds\\
&= p\Gamma(p/2)
\end{align*}
$$

> [!tldr] Change base
> Set $s = t^2$, or $t = \sqrt{s}$
> There forse
> $$
> dt = d\sqrt{s} \implies \frac{dt}{ds} = \frac{d\sqrt{s}}{ds} = \frac{1}{2\sqrt{s}} \implies dt =\frac{ds}{2\sqrt{s}}
> $$
> 

It can be prooved that $\Gamma(x) \leq 3x^x$  for every $x \geq 1/2$, the for every $p \geq 1$ we have
$$
\mathbb{E}\vert X \vert^p \leq p\Gamma(p/2) \leq 3p(p/2)^{p/2} \leq K_2\sqrt{p}
$$

Now, taking the $p$-th root we have 
$$
\Vert X \Vert_{L^p} = (\mathbb{E}\vert X \vert^p)^{1/p} \leq (3p)^{1/p} \sqrt{p/2} \leq K_2 \sqrt{p}
$$
with $K_2 \leq 3$.

> [!tldr]
> The function $x^{1/x}$ is a *concave* function with maximum value in $x = e$.
> Therefore $(3x)^{1/x} \leq (3e)^{1/e} \leq 3\sqrt{2}$

#### $(2 \implies 3)$
Assume that [[#^51312a|property 2]] holds.
We can assume $K_2 = 1$
Using [Taylor's expansion](https://en.wikipedia.org/wiki/Taylor_series), we have
$$
\mathbb{E}e^{\lambda^2 X^2} = \mathbb{E} \left[ 1 + \sum_{j=1}^{\infty}\frac{(\lambda^2X^2)^j}{j!}\right] =  1 + \sum_{j=1}^{\infty}\frac{\lambda^{2j}\mathbb{E}X^{2j}}{j!}
$$

By using [[#^51312a|property 2]], we have that $\mathbb{E}X^{2j} \leq 6j(j)^{j} \leq 2^j(j)^j = (2j)^j$.
Using [Stirling's approximation](https://en.wikipedia.org/wiki/Stirling%27s_approximation) we have that $j! \geq (j/e)^j$, then 
$$
\begin{align*}
\mathbb{E}e^{\lambda^2 X^2} 
&= 1 + \sum_{j=1}^{\infty}\frac{\lambda^{2j}\mathbb{E}X^{2j}}{j!}\\
&\leq 1 + \sum_{j=1}^{\infty}\frac{\lambda^{2j}(2j)^j}{j!}\\
&\leq 1 + \sum_{j=1}^{\infty}\frac{\lambda^{2j}(2j)^j}{(j/e)^j}\\
&= 1 + \sum_{j=1}^{\infty}(2e\lambda^2)^j\\
&= \sum_{j=0}^{\infty}(2e\lambda^2)^j
\end{align*}
$$
The previous series **converges** only when $2e\lambda^2 < 1$, i.e. when $\vert \lambda \vert < 1/\sqrt{2e}$.


> [!tldr] Geometric series
> Finite series
> $$
> \sum_{k=0}^{n}x^k = \frac{1-x^{n-1}}{1-x}
> $$
> 
> When $|x| \leq 1$, the infinite series converges
> $$
> \sum_{k=0}^{\infty}x^k = \frac{1}{1-x}
> $$

	
Therefore we have that
$$
\mathbb{E}e^{\lambda^2X^2} \leq \sum_{j=0}^{\infty}(2e\lambda^2)^j \leq \frac{1}{1-2e\lambda^2}
$$


We can now bound $\frac{1}{1-x} \leq e^{2x}$ for every $0 \leq x \leq \frac{1}{2}$.
Then, by setting $2e\lambda^2 \leq 1/2$, or $\vert \lambda \vert \leq 1/(2\sqrt{e})$, we obtain the bound
$$
\mathbb{E}e^{\lambda^2X^2} \leq \exp(4e\lambda^2)
$$

Finally, the [[#^32045f|property 3]] yields for $K_3 = 2 \sqrt{e}$.

#### $(3 \implies 4)$ #exercise
Assume that [[#^32045f|property 3]] holds.

We only have to solve the following equation
$$
M_{X^2}(1/K_4) \leq e^{1 / K_4^2} \leq 2
$$

It holds when $K_4 \geq \sqrt{1/\ln{2}}$.

#### $(4 \implies 1)$
Assume that [[#^51fe83|property 4]] holds.

Assume that $K_4 = 1$.
Then, by using [[Some inequalities#Markov's inequality]]
$$
\begin{align*}
\mathbb{P}\{\vert X \vert \geq t\}
&= \mathbb{P}\left\lbrace e^{X^2}\geq e^{t^2} \right\rbrace\\
&\leq \frac{\mathbb{E}e^{X^2}}{e^{t^2}}\\
&\leq 2e^{-t^2}
\end{align*}
$$
This proves [[#^b5dd62|property 1]], with $K_1 = 1$.

#### $(3 \implies 5)$
As before, assume that [[#^32045f|property 3]] holds and $K_3 = 1$, and that $\mathbb{E}X = 0$.

> [!note] Remark
> $$e^x \leq x+e^{x^2}\;\; \forall x \in \mathbb{R}$$

Then
$$
\begin{align*}
\mathbb{E}e^{\lambda X}
&\leq \mathbb{E}(\lambda X + e^{\lambda^2 X^2})\\
&= \mathbb{E}e^{\lambda^2 X^2}\\
&\leq e^{\lambda^2}
\end{align*}
$$
The last inequality holds if $\vert \lambda \vert \leq 1$, by requirement of [[#^32045f|property 3]].
Thus, if $\vert \lambda \vert \leq 1$ the [[#^32045f|property 3]] implies the [[#^70f6af|property 5]] with $K_5 = 1$.

Now, assume $\vert \lambda \vert \geq 1$.

> [!note] Remark
> $$
> 2\lambda x \leq \lambda^2 + x^2\;\;\; \forall \lambda, x \in \mathbb{R}
> $$

Therefore
$$
\begin{align*}
\mathbb{E}e^{\lambda X}
&\leq \mathbb{E}e^{(\lambda^2 + X^2)/2}\\
&= e^{\lambda^2/2}\mathbb{E}e^{X^2/2}\\
&\leq e^{\lambda^2/2} \cdot e^{1/2}\\
(\text{since } \vert \lambda \vert \geq 1)&\leq e^{\lambda^2}
\end{align*}
$$
And this implies the [[#^70f6af|property 5]] with $K_5 = 1$.

#### $(5 \implies 1)$
Assume that [[#^70f6af|property 5]] holds.

==TODO==
