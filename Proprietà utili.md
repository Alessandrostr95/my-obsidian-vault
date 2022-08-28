# Covarianza
- $$\text{Cov}(X,Y) = \mathbb{E}\left[ (X-\mathbb{E}\left[ X \right])(Y -\mathbb{E}\left[ Y \right])  \right] = \mathbb{E}\left[ XY \right] - \mathbb{E}\left[ X \right]\mathbb{E}\left[ Y \right]$$
- $$\text{Cov}(X,X) = \text{Var}(X)$$
- $$\text{Cov}(X,Y) = \text{Cov}(Y,X)$$
- $$\text{Cov}(aX + b, Y) = a \cdot \text{Cov}(X,Y)$$
- $$\text{Cov}(X + A,Y) = \text{Cov}(X,Y) + \text{Cov}(A,Y)$$

# Formula probabilità totali
Sia l'evento $A$ e la v.a. $X$ con supporto $\chi$.
Allora $$P(A) = \int_{\chi}P(A \vert X = x)f_{X}(x) \, dx$$

```ad-example
title: Esempio d'uso

Siano $X,Y$ due v.a. **i.i.d.** con supporto $\mathbb{R}$, e sia $D = X - Y$.

Allora avremo che 
$$\begin{align*}
F_D(t)
&= P(X-Y \leq t) = P(X \leq t + Y)\\
&= \int_{\mathbb{R}} P(X \leq t + Y \vert Y = y) f_Y(y) \, dy\\
&= \int_{\mathbb{R}} P(X \leq t + y) f_Y(y) \, dy\\
&= \int_{\mathbb{R}} F_X(X \leq t + y) f_Y(y) \, dy
\end{align*}$$
```

