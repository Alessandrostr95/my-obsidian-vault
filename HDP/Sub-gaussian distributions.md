---
tags:
  - HDP
date: 2025-09-19
---
The [[Hoeffding’s inequality]] and [[Chernoff's inequality]] give tail bound for **bernoulli** random variables.
It would be usefull to extend those bounds to a wide class of distributions.
So a natural question would be: which random variables $X_i$ must obey a concentration inequality like [[Hoeffding’s inequality]], i.e., 
$$\mathbb{P}\left\lbrace \left\vert \sum_{i=1}^{N}a_iX_i \right\vert \geq t \right\rbrace \leq 2\exp\left(-\frac{t^2}{2\Vert a \Vert_2^2}\right)?$$

We can start observing that, when $N=1$, we have a bound of the form $$\mathbb{P}\{|X| \geq t\} \leq 2e^{-ct^2}$$
We call this tail as **sub-gaussian** tail.
This gives us an automatic restriction: if we want Hoeffding's inequality to hold, we must have random variables $X_i$ with sub-gaussian tails. ==(necessary condition)==

This class of distribution is sufficiently wide as it contains Gaussian, Bernoulli and **all bounded distributions**.

It can be proved that concentration results like Hoeffding's inequality can indeed be proved **for all** sub-gaussian distributions. ==(if sub-gaussian then we can give a Hoeffding-like tail bound)==

```ad-example
For example, let us consider a normal random variable $X \sin N(0,1)$.
Since $X$ is a sub-gaussian r.v. (we will see it later), we have that $$\mathbb{P}\{\vert X \vert \geq t\} \leq 2e^{-t^2/2}$$
```

> **Moments of the normal distribution** #exercise 
> Show that for each $p \geq 1$, the random variable $X\sim N(0,1)$ satisfies
> $$\Vert X \Vert_{L^p} = (\mathbb{E}\vert X \vert^p)^{1/p} = \sqrt{2}\left[ \frac{\Gamma\left( \frac{1+p}{2} \right)}{\Gamma\left( \frac{1}{2}\right)}\right]^{1/p}$$
> where $$\Gamma(x) := \int_{0}^{\infty}t^{x-1}e^{-t} \, dt$$
> Then deduce that $$\lim_{p \to \infty}\Vert X \Vert_{L^p} = O(\sqrt{p})$$
> 
> By the exercise [[Some inequalities#$p$-moments via tails exercise]], we have that
> $$\begin{align*}
> \mathbb{E}\vert X \vert^p
> &= \int_{0}^{\infty}pt^{p-1}\mathbb{P}\{\vert X \vert > t\}\, dt\\
> &= \int_{0}^{\infty}pt^{p-1}\frac{1}{\sqrt{2\pi}}\int_t^{\infty}e^{-x^2/2}\,dx\, dt
> \end{align*}$$
> ==TODO==




-----

# Sub-gaussian properties
Let $X$ be any random variable.
Then the following properties are equivalent

1. There exists $K_1 > 0$ such that the tails of $X$ satisfy $$\mathbb{P}\{\vert X \vert \geq t\} \leq 2 \exp(-t^2/K_1^2),\;\;\; \forall t \geq 0$$
2. There exists $K_2 > 0$ such that the [[Some definitions#$L p$ norm of a random variable|moments]] of $X$ satisfy $$\Vert X \Vert_{L^p} = (\mathbb{E}\vert X \vert^p)^{1/p} \leq K_2\sqrt{p},\;\;\; \forall p \geq 1$$
3. There exists $K_3 > 0$ such that the [[Some definitions#MGF - Moment Generating Function|MGF]] of $X^2$ satisfies $$M_{X^2}(\lambda^2) =\mathbb{E}\exp(\lambda^2 X^2) \leq \exp(K_3^2\lambda^2),\;\;\; \forall \lambda : \vert \lambda\vert \leq \frac{1}{K_3}$$
4. There exists $K_4 > 0$ such that the [[Some definitions#MGF - Moment Generating Function|MGF]] of $X^2$ is **bounded** at some point, namely $$M_{X^2}(1 / K_4^2) =\mathbb{E}\exp(X^2 / K_4^2) \leq 2$$
Moreover, if $\mathbb{E}X = 0$ we have another equivalent property
5. There exists $K_5 > 0$ such that the [[Some definitions#MGF - Moment Generating Function|MGF]] of $X$ satisfies $$M_X(\lambda) = \mathbb{E}\exp(\lambda X) \leq \exp(K_5^2\lambda^2),\;\;\; \forall \lambda \in \mathbb{R}$$


```ad-important
The parameters $K_i > 0$ appearing in these properties differ from each other by at most an absolute constant factor.
I.e., there exists an **absolute constant** $C$ such that property $i$ implies property $j$ with parameter $K_j \leq CK_i$ for any two properties $i, j = 1, ... , 5$.
```



### Proof
