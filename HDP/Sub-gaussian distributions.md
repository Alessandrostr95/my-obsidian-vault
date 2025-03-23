---
tags:
  - HDP
date: 2025-03-23
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

> [!note] **Moments of the normal distribution** #exercise
> Show that for each $p \geq 1$, the random variable $X\sim N(0,1)$ satisfies
> $$
> \Vert X \Vert_{L^p} = (\mathbb{E}\vert X \vert^p)^{1/p} = \sqrt{2}\left[ \frac{\Gamma\left( \frac{1+p}{2} \right)}{\Gamma\left( \frac{1}{2}\right)}\right]^{1/p}
> $$
> where 
> $$
> \Gamma(x) := \int_{0}^{\infty}t^{x-1}e^{-t} \, dt
> $$
> 
> Then deduce that 
> $$
> \lim_{p \to \infty}\Vert X \Vert_{L^p} = O(\sqrt{p})
> $$

> [!help] Solution 
> By the exercise [[Some inequalities#$p$-moments via tails exercise]], we have that
> $$
> \begin{align*}
> \mathbb{E}\vert X \vert^p
> &= \int_{0}^{\infty}pt^{p-1}\mathbb{P}\{\vert X \vert > t\}\, dt\\
> &= \int_{0}^{\infty}pt^{p-1}\frac{1}{\sqrt{2\pi}}\int_t^{\infty}e^{-x^2/2}\,dx\, dt\\
> &= \int_{0}^{\infty}pt^{p-1}\frac{1}{\sqrt{2\pi}}\left( \int_{0}^{\infty}e^{-x^2/2}\,dx- \int_{0}^{t}e^{-x^2/2}\,dx \right)\, dt\\
> &= \int_{0}^{\infty}pt^{p-1}\frac{1}{\sqrt{2\pi}}\left( \sqrt{\frac{\pi}{2}}- \sqrt{\frac{\pi}{2}} \text{erf}\left(\frac{t}{\sqrt{2}}\right) \right)\, dt\\
> &= \int_{0}^{\infty}pt^{p-1}\frac{1}{\sqrt{2\pi}} \sqrt{\frac{\pi}{2}} \left(1 - \text{erf}\left(\frac{t}{\sqrt{2}}\right) \right)\, dt\\
> &= \int_{0}^{\infty}pt^{p-1}\frac{1}{2} \left(1 - \text{erf}\left(\frac{t}{\sqrt{2}}\right) \right)\, dt
> \end{align*}
> $$
> 
> where
> $$
> \text{erf}(t) = \frac{2}{\sqrt{\pi}}\int_{0}^{t}e^{-x^2}\,dx 
> $$
> 
> Then
> 
> ==TODO==


--------
# [[Sub-gaussian properties]]
