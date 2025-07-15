
> [!theorem]
> Let $X=(X_1, \dots, X_n) \in \mathbb{R}^n$ be a random vector with **independent**, [[Sub-gaussian random variables|sub-gaussians]] coordinates $X_i$, such that $\mathbb{E}X^2 = 1$.
> Then
> $$
> \Big\Vert \Vert X \Vert_2 - \sqrt{n} \Big\Vert_{\psi_2} \leq CK^2
> $$
> where $K = \max_i \Vert X \Vert_{\psi_2}$ and $C$ is an absolute constant.

Il precedente teorema ci dice che il vettore $X$ ha un norma $L_2$ che si concentra intorno a $\sqrt{n}$, questo perché la variabile aleatoria $\Vert X \Vert_2 - \sqrt{n}$ ha una [[Sub-gaussian random variables#^2d6cf4|sub-gaussian norm]] limitata, e quindi che la probabilità che $\Vert X \Vert_2$ si discosti da $\sqrt{n}$ sia maggiore di un certo $\varepsilon$ (ovvero la probabilità che $\vert \Vert X \Vert_2 - \sqrt{n} \vert > \varepsilon$) **decresce esponenzialmente** in $\varepsilon$.

> [!note]
> $$
> \mathbb{E}\Vert X \Vert_2^2 = \mathbb{E} \sum_{i=1}^{n} X_i^2 = \sum_{i=1}^{n}\mathbb{E} X_i^2 = n
> $$
#### Proof
Without loss of generality, we can assum $K \geq 1$[^1]
We can focus the analysis on the random variable
$$
\Vert X \Vert_2^2 - n
$$
or in its normalization
$$
\frac{1}{n} \Vert X \Vert_2^2 - 1 = \frac{1}{n} \sum_{i=1}^{n}(X_i^2 -1)
$$
Since $X_i$ is [[Sub-gaussian random variables|sub-gaussian]], then $X_i^2$ is [[Sub-exponential Random Variables|sub-exponential]] with mean $\mathbb{E}X_i^2 = 1$ (by assumption), and $X_i^2 - 1$ is sub-exponential too, with mean $0$.
By [[Sub-exponential Random Variables#Centering sub-exponential]] and [[Sub-exponential Random Variables#Sub-exponential is sub-gaussian squared]] we have that
$$
\Vert X_i^2 - 1 \Vert_{\psi_1} \leq C \Vert X_i^2 \Vert_{\psi_1} = C \Vert X_i \Vert_{\psi_2}^2 \leq CK^2
$$

By [[Bernstein's inequality]], for any $u \geq 0$ we have
$$
\mathbb{P}\left\lbrace \left\vert \frac{1}{n} \Vert X \Vert_2^2 - 1 \right\vert \geq u \right\rbrace \leq 2 \exp\left( -cn \cdot \min\left( \frac{u^2}{K^2}, \frac{u}{K}\right)\right)
$$

Since $K \geq 1$, we have that $K^4 \geq K^2 \geq K$.
Therefore
$$
\mathbb{P}\left\lbrace \left\vert \frac{1}{n} \Vert X \Vert_2^2 - 1 \right\vert \geq u \right\rbrace \leq 2 \exp\left( -\frac{cn}{K^4} \cdot \min\left( u^2, u\right)\right)
$$


> [!tldr] Fact 1
> For every $x \geq 0$, we have
> $$
> \vert x - 1 \vert \geq \delta \implies \vert x^2 - 1 \vert \geq \max(\delta^2, \delta)
> $$

> [!tldr] Fact 2
> For every $\delta \geq 0$, we have
> $$
> f(\delta) := \min\left(\max(\delta, \delta^2), \max(\delta, \delta^2)^2\right) = \delta^2
> $$
> > [!help] Proof
> > if $\max(\delta, \delta^2) = \delta \implies \delta < 1 \implies \min(\delta, \delta^2) = \delta^2$
> > if $\max(\delta, \delta^2) = \delta^2 \implies \delta \geq 1 \implies \min(\delta^2, \delta^4) = \delta^2$ $\square$.

For every $\delta \geq 0$ we have that
$$
\begin{align*}
\mathbb{P}\left\lbrace \left\vert \frac{1}{\sqrt{n}} \Vert X \Vert_2 - 1\right\vert \geq \delta\right\rbrace
&\leq \mathbb{P}\left\lbrace \left\vert \frac{1}{n} \Vert X \Vert_2^2 - 1\right\vert \geq \max(\delta, \delta^2)\right\rbrace\\
&\leq 2 \exp \left( -\frac{cn}{K^4} \cdot \min\left(\max(\delta, \delta^2), \max(\delta, \delta^2)^2\right) \right)\\
&\leq 2 \exp \left( -\frac{cn}{K^4} \delta^2 \right)
\end{align*}
$$

By setting $\delta = t/\sqrt{n}$ we obtain
$$
\mathbb{P}\left\lbrace \left\vert \Vert X \Vert_2 - \sqrt{n}\right\vert \geq t \right\rbrace
= \mathbb{P}\left\lbrace \left\vert \frac{1}{\sqrt{n}} \Vert X \Vert_2 - 1\right\vert \geq \frac{t}{\sqrt{n}} \right\rbrace
\leq 2\exp\left(  - \frac{cn}{K^4} \cdot \frac{t^2}{n} \right) = 2\exp\left(  - \frac{ct^2}{K^4} \right)
$$

By the [[Sub-gaussian random variables#^fedbd4|property 1]], we have that
$$
\mathbb{P}\left\lbrace \left\vert \Vert X \Vert_2 - \sqrt{n}\right\vert \geq t \right\rbrace \leq 2 \exp\left(-\frac{ct^2}{\Big\Vert \Vert X \Vert_2 - \sqrt{n} \Big\Vert_{\psi_2}} \right) \leq 2 \exp\left(-\frac{ct^2}{C\big\Vert \Vert X \Vert_2 \big\Vert_{\psi_2}} \right)
$$

[^1]: we ca always rescale $X$ in order to have a different value $K_4$ in the [[Sub-gaussian properties#^51fe83|property 4]].