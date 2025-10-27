
# Some definitions

> [!definition] Probability Simplex
> Given a *discrete* set $B$, we define with $\Delta(B)$ as the *probability simplex* over $B$ as 
> $$
> \Delta(B) = \left\lbrace x \in \mathbb{R}^{|B|} \mid x_i \geq 1 \text{ for every } i \land \sum_{i=1}^{|B|}x_i = 1 \right\rbrace
> $$

> [!definition] Randomized Algorithm
> A randomized algorithm $\mathcal{M}$  is a function from a domain $A$ to a discrete range $B$, which is associated a *randomized mapping* $M: A \to \Delta(B)$. For every $a \in A$, we have that $P(\mathcal{M}(a) = b) = (M(a))_b$ , for every $b \in B$.


> [!definition] Distance Between Databases
> Let $D,D'$ be two databases, i.e. multisets of elements over the types $\mathcal{X}$.
> We can represent $D,D'$ as their *histograms* $x,y \in \mathbb{N}^{|\mathcal{X}|}$, i.e. vectors where the $i$-th entry counts how many elements of type $i \in \mathcal{X}$ are in the database.
> Therefore, the distance between $D$ and $D'$ is the $\ell_1$-norm of $x-y$.

> [!definition] Differential Privacy
> A randomized algorithm $\mathcal{M}$ with with domian $\mathbb{N}^{|\mathcal{X}|}$ is $(\varepsilon, \delta)$-differentially private if for every subset of outputs $S$, and for every inputs $x,y$, with $\Vert x-y \vert_1 \leq 1$, we have
> $$
> P(\mathcal{M}(x) \in S) \leq e^{\varepsilon} P(\mathcal{M}(y) \in S) + \delta.
> $$
> If $\delta=0$, we say that $\mathcal{M}$ is $\varepsilon$-differentially pirvate.

> [!definition] Privacy Loss
> The privacy loss by observed an output $\xi$,  is defined as 
> $$
> \log\left( \frac{P(\mathcal{M}(x) = \xi)}{P(\mathcal{M}(y) = \xi)}\right).
> $$
> If $\mathcal{M}$ is $(\varepsilon, \delta)$-differentially private, then the privacy loss is at most $\varepsilon$ with probability at most $1-\delta$.

> [!proposition] Post Processing
> Let $\mathcal{M}: \mathbb{N}^{|\mathcal{X}|} \to R$ be $(\varepsilon, \delta)$-differentially private, and let $f:R\to R'$ be any *random mapping*.
> $f \circ \mathcal{M}: \mathbb{N}^{|\mathcal{X}|} \to R'$ is $(\varepsilon, \delta)$-differentially private.

`\begin{proof}`
Any randomized mapping can be decomposed into a convex combination of deterministic functions, and a convex combination of differentially private mechanisms is differentially private.
Therefore, wlog, we can think $f$ as a deterministic function.

Let $x,y$ two neighbors inputs ($\Vert x-y \Vert_1 \leq 1$), and let $S \subseteq R'$.
We now define the set
$$
T = \{ r \in R \mid f(r) \in S\},
$$
i.e. $S$ is the image of $T$ after applying $f$.
$$
\begin{align*}
P(f(\mathcal{M}(x)) \in S)
&= P(\mathcal{M}(x) \in T)\\
&\leq e^\varepsilon P(\mathcal{M}(y) \in T) + \delta\\
&\leq e^\varepsilon P(f(\mathcal{M}(y)) \in S) + \delta.
\end{align*}
$$
`\end{proof}`


[[Randomised Response Mechanism]]
[[Laplace Mechanism]]


