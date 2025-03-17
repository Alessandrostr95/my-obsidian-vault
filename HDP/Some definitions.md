---
tags:
  - HDP
date: 2025-03-17
desc: some definition for high dimensional probability
---

### Mean
$$\mathbb{E}X$$
### Variance
$$\text{Var}(X) = \mathbb{E}(X-\mathbb{E}X)^2$$

### Standard Deviation
$$\sigma(X) = \sqrt{\text{Var}(X)}$$

### Covariance
$$\text{cov}(X,Y) = \mathbb{E}(X-\mathbb{E}X)(Y-\mathbb{E}Y)$$

### MGF - Moment Generating Function
$$M_X(t) := \mathbb{E}\;e^{tX}, \;\; \forall t \in \mathbb{R}$$

### $p$-th Moment of $X$
$$\mathbb{E}X^p,\;\; \forall p > 0$$

### $p$-th Absolute Moment of $X$
$$\mathbb{E}\vert X \vert^p\;\; \forall p > 0$$

### $L^p$ norm of a random variable
$$\Vert X \Vert_{L^p} := (\mathbb{E}\vert X \vert^p)^{1/p},\;\; p \in (0, \infty)$$

When $p = \infty$, the definition is the **[essential supremum](https://en.wikipedia.org/wiki/Essential_infimum_and_essential_supremum)** of $\vert X \vert$, i.e.,
$$\Vert X \Vert_{L^p} := \text{ess}\sup \vert X \vert$$

### $L^p$ norm of a probability space
For fixed $p$ and probability space $(\Omega, \Sigma, \mathbb{P})$, the $L^p$-norm space of $(\Omega, \Sigma, \mathbb{P})$, say $L^p=L^p(\Omega, \Sigma, \mathbb{P})$, consists of all random variable $X \in \Omega$ with **finite** [[#$L p$ norm of a random variable|$L^p$ norm]], i.e.,
$$L^p := \{ X \in \Sigma \mid \Vert X \Vert_{L^p} < \infty\}.$$

For $p \in \left[ 1, \infty \right]$,  we have that $\Vert \cdot \Vert_{L^p}$ is a [[#Norm|norm]],  and $L^p$ is a [Banach space](https://en.wikipedia.org/wiki/Banach_space) (follow from [[|Minkowski's inequality]]).

For $p < 1$, the triangular inequality fails and then $\Vert \cdot \Vert_{L^p}$ is not a [[#Norm|norm]].

For $p=2$, the space $L^2$ is an [Hilbert space](https://it.wikipedia.org/wiki/Spazio_di_Hilbert).
The [[#Mean|mean]] of the product of two random variable $XY$ can be expressed as the [[#Inner product $ langle cdot, cdot rangle$|inner product]] $\langle \cdot, \cdot \rangle_{L^2}$, in fact
$$\begin{align*}
\mathbb{E}XY
&= \int_{\mathbb{R}}\int_{\mathbb{R}} xy \cdot f_{X,Y}(x,y)\,dx\,dy\\
&= \int_{\Omega}X(w)Y(w)dP(w)\\
&= \langle X,Y \rangle_{L^2}
\end{align*}$$ where $X(\cdot), Y(\cdot)$ belong to the space $(\Omega = \mathbb{R}, \Sigma = \mathbb{R} \times \mathbb{R}, \mathbb{P} = P(\cdot) = f_{X,Y}(\cdot, \cdot))$.


Observe that the [[#Standard Deviation|standard deviation]] of $X$ che be express with the $L^2$ norm, i.e., 
$$\Vert X - \mathbb{E}X \Vert_{L_2} = \sqrt{\mathbb{E}(X- \mathbb{E}X)^2} = \sqrt{\text{Var}(X)} = \sigma(X)$$

Similarly, that the [[#Covariance|covariance]] che be express with the [[#Inner product $ langle cdot, cdot rangle$|inner product]] $\langle \cdot, \cdot \rangle_{L^2}$, i.e.,
$$\begin{align*}
\langle X-\mathbb{E}X, Y - \mathbb{E}Y \rangle_{L^2}
&= \langle \widetilde{X}, \widetilde{Y} \rangle_{L^2}\\
&= \int_{\mathbb{R}}\int_{\mathbb{R}}xy\cdot f_{\widetilde{X},\widetilde{Y}}(x,y)\,dx\,dy\\
&= \mathbb{E}\widetilde{X}\widetilde{Y}\\
&= \mathbb{E}(X-\mathbb{E}X)(Y - \mathbb{E}Y)\\
&= \text{cov}(X,Y)
\end{align*}$$


---

### Norm
A norm $\Vert \cdot \Vert : V \to \mathbb{R}$  over a vector space $V$ is a *function* s.t.:
- $\Vert x \Vert \geq 0$ for all $x \in V$;
- $\Vert x \Vert = 0$ if and only if $x = \mathbf{0}$;
- $\Vert \lambda x \Vert = \vert \lambda \vert \cdot \Vert x \Vert$, for all $x \in V$ and for all scalar $\lambda \in \mathbb{R}$;
- $\Vert x + y \Vert \leq \Vert x \Vert + \Vert y \Vert$, for all $x,y \in V$.

The pair $(X, \Vert \cdot \Vert)$ is a said **metric space**.

### Inner product $\langle \cdot, \cdot \rangle$
An inner product is a function $$\langle \cdot, \cdot \rangle: V \times V \to \mathbb{C}$$ where $V$ is a vector space. The same definition yelds when the function maps to $\mathbb{R}$.

The following properties are satisfied, for each $x,y,z \in V$ and $a,b \in \mathbb{C}$:
- **Conjugate symmetry**: $\langle x, y \rangle = \overline{\langle y, x \rangle}$. When the image is $\mathbb{R}$ the this porperity is simply the **symmetry**;
- **Linearity**: $\langle ax + by, z \rangle = a\langle x, z \rangle + b\langle y, z \rangle$;
- **Positive-definiteness**: if $x \neq \mathbf{0}$ then $\langle x,x \rangle > 0$.

Some properties follows:
- $\langle x, \mathbf{0} \rangle = \langle \mathbf{0}, x \rangle = 0$;
- $\langle x,x \rangle$ is **real** and **non-negative**;
- $\langle x,x \rangle = 0$ if and only if $x = \mathbf{0}$;
- $\langle x, ay + bz \rangle = \overline{a}\langle x, y \rangle + \overline{b}\langle x, z \rangle$;
- $\langle x+y, x+y \rangle = \langle x, x \rangle + \langle y, y \rangle + 2\text{Real}(\langle x, y \rangle)$. If the image is $\mathbb{R}$, then $\langle x+y, x+y \rangle = \langle x, x \rangle + \langle y, y \rangle + 2\langle x, y \rangle$.

Some example of inner products are:
- **Dot-product** over the vector space $\mathbb{R}^n$, i.e., $$\langle x, y \rangle = x^Ty = \sum_{i=1}^{n}x_iy_i, \;\; \forall x,y \in \mathbb{R}^n$$
- The **mean of product of random variables**, i.e., $$\langle X, Y \rangle = \mathbb{E}XY$$

### Canonical $L^2$-inner product
Let $f,g$ two function over a measurable space $(\mathcal{X},\mathcal{A}, \mu)$ , then $$\langle f,g \rangle_{L^2} := \int_{\mathcal{X}}f(x)g(x)\,d\mu(x) = \int_{\mathcal{X}}(f \cdot g)(x)\,d\mu(x)$$