> [!definition]
> The Laplace distribution with parameter $b$, has a mass funciont of
> $$
> Lap(x \mid b) = \frac{1}{2b}e^{-\frac{|x|}{b}}
> $$
> The mean i $0$ and the variance is $\sigma^2=2b^2$.
> The cdf is
> $$
> P(X \geq x) = \begin{cases}
> \frac{1}{2}\exp(x/b) &, x < 0\\
> 1- \frac{1}{2}\exp(-x/b) &, x \geq 0
> \end{cases}
> $$

> [!definition] $\ell_1$-sensitivity
> The $\ell_1$-sensitivity of a function $f:\mathbb{N}^{|\mathcal{X}|} \to \mathbb{R}^k$ is defined as 
> $$
> \Delta f = \max_{x,y : \Vert x-y \Vert_1 = 1} \Vert f(x) - f(y) \Vert_1
> $$

> [!definition] Laplace Mechanism
> Given a function $f:\mathbb{N}^{|\mathcal{X}|} \to \mathbb{R}^k$, the Laplace mechanism is defined as
> $$
> \mathcal{M}_L(x,f,\varepsilon) = f(x) + (Y_1, \dots, Y_k)^T,
> $$
> where $Y_i \sim Lap(\Delta f/\varepsilon)$.

> [!theorem]
> The Laplace mechanism is $(\varepsilon,0)$-differentially private.

`\begin{proof}`
$$\begin{align*}
\frac{P(\mathcal{M}(x,f,\varepsilon) = z)}{P(\mathcal{M}(y,f,\varepsilon) = z)}
&= \frac{P(Y = z-f(x))}{P(Y = z-f(y))}\\
&= \prod_{i=1}^{k} \frac{P(y_i = z_i -f(x)_i)}{P(y_i = z_i -f(y)_i)}\\
&= \prod_{i=1}^{k} \frac{\exp(-\frac{\varepsilon |f(x)_i - z_i |}{\Delta f})}{\exp(-\frac{\varepsilon |f(y)_i - z_i |}{\Delta f})}\\
&= \prod_{i=1}^{k} \exp\left(-\frac{\varepsilon (|f(x)_i - z_i | - |f(x)_i - z_i |)}{\Delta f}\right)\\
&\leq \prod_{i=1}^{k} \exp\left(-\frac{\varepsilon |f(x)_i - f(x)_i|}{\Delta f}\right)\\
&= \exp\left(-\frac{\varepsilon \Vert f(x) - f(x)\Vert_1}{\Delta f}\right)\\
&\leq e^{\varepsilon}.
\end{align*}$$
`\end{proof}`


