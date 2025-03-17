### Convergence in distribution
Let $X_1, \dots, X_N$ be a sequence of real random random variables with [[Some definitions#CMF - Cumulative Distribution Function|CMF]] $F_1, \dots, F_N$, and $X$ be a random variable with CMF $F$.
We say tha the sequence $X_1, \dots, X_N$ converge in distribution to $X$, i.e., $X_N \xrightarrow{d} X$, if
$$\lim_{N \to \infty}F_N(x) = F(x)$$
for every $x \in \mathbb{R}$, s.t. $F$ is continuos.

### Convergence in porbability
Let $X_1, \dots, X_N$ be a sequence of real random random variables, and $X$ be any random variable.
We say tha the sequence $X_1, \dots, X_N$ converge in probability to $X$, i.e., $X_N \xrightarrow{p} X$, if for every $\varepsilon > 0$ we have
$$\lim_{N \to \infty}\mathbb{P}\{\vert X_N - X \vert > \varepsilon\} = 0$$


### Almost sure convergence
We say tha the sequence $X_1, \dots, X_N$ converge in probability to $X$, i.e., $X_N \xrightarrow{a.s.} X$, if
$$\mathbb{P} \left\{ \lim_{N \to \infty} X_N = X \right\} = 1$$

