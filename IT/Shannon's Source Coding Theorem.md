---
author: Alessandro Straziota
tags:
  - Probability
  - Math
  - IT
---

> [!definition] Ensamble
> An ensamble $X$ is a triple $(x, A_X, P_x)$ where $x$ is a random variable which outcome comes from the set $A_X = \{a_1, \dots, a_n\}$ , having probabilities $P_X = \{p_1, \dots, p_n\}$ where $P(x= a_i) = p_i$, $p_i \geq 0$ for every $i = 1, \dots n$, and $\sum_{i=1}^{n}p_i = 1$.

> [!definition] The Shannon Information Content
> Let $x$ be an outcome from and ensamble $X$. Then, the *information content* of the outcome $x$ is defined as
> $$
> h(x) = \log_2\frac{1}{P(x)} = - \log_2{P(x)}
> $$

> [!definition] Entropy
> The *Entropy* of an ensamble $X = (x, A_X, P_X)$ is the **average** information content of the outcomes in $A_X$, with weight in $P_X$, i.e.
> $$
> H(X) = \sum_{x \in A_X} P(x) \log_2{\frac{1}{P(x)}} = - \sum_{x \in A_X}P(x)\log_2{P(x)}
> $$
> with the convention that for $P(x) = 0$ we have $P(x) \log{(1/P(x))} = 0$, since $\lim_{x \to 0^+}x\log{1/x} = 0$.

> [!observation]
> 1. $H(X) \geq 0$;
> 2. $H(X) = 0$ if and only if there exists an $x \in A_X$ such that $P(x) = 1$;
> 3. $H(X) \leq \log_2\vert A_X \vert$, and it is maximized if and only if $P(x) = 1 / \vert A_X \vert$ for every $x \in A_X$.

> [!definition] Joint Entropy
> The joint entropy of two ensamble $X, Y$ is defined as
> $$
> H(X,Y) = \sum_{xy \in A_X \times A_Y} P(x,y) \log_2{\frac{1}{P(x,y)}}
> $$

> [!note] Exercise
> When $X,Y$ are independent, then joint entropy is additive, i.e.
> $$
> H(X,Y) = H(X) + H(Y)
> $$

 > [!help] Proof
> $$
> \begin{align*}
> H(X,Y)
> &= \sum_{xy \in A_X \times A_Y} P(x,y) \log_2{\frac{1}{P(x,y)}}\\
> &= \sum_{xy \in A_X \times A_Y} P(x)P(y) \left(\log_2{\frac{1}{P(x)}} + \log_2{\frac{1}{P(y)}}\right)\\
> &= \sum_{xy \in A_X \times A_Y} P(x)P(y) \log_2{\frac{1}{P(x)}} + \sum_{xy \in A_X \times A_Y} P(x)P(y)\log_2{\frac{1}{P(y)}}\\
> &= \sum_{y \in A_Y}P(y)\sum_{x \in A_X} P(x)\log_2{\frac{1}{P(x)}} + \sum_{x \in A_X} P(x)\sum_{y \in A_Y} P(y)\log_2{\frac{1}{P(y)}}\\
> &= 1 \cdot \sum_{x \in A_X} P(x)\log_2{\frac{1}{P(x)}} + 1 \cdot \sum_{y \in A_Y} P(y)\log_2{\frac{1}{P(y)}}\\
> &= H(X) + H(Y). \;\;\; \square
> \end{align*}
> $$


> [!note] Observation
> Let $x = (x_1, \dots, x_N)$ be a string of $N$ independent and identically distributed symbols from the same ensamble $X$. Let $X^N = (X_1, \dots, X_N)$ be the ensamble of $x$, with $X_1 = \dots = X_N = X$, set $A_X^N = A_X \times \dots \times A_X$.
> Therefore
> $$
> H(X^N) = \sum_{i=1}^{N} H(X_i) = N H(X).
> $$

^d038d3


> [!definition] Raw Bit Content
> The *raw bit content* of an examble $X=(x, A_X, P_X)$ is defined as 
> $$
> H_0(X) = \log_2{\vert A_X \vert}.
> $$
> ==$H_0(X)$ is also a **lower bound** for the number of **binary question** that are always guaranteed to identify an outcome from the ensamble $X$.==

> [!note] Observation
> The raw information content is an additive quantity.
> Let consider the ordered pair $xy \in A_X \times A_Y$. The raw information content of $xy$ is
> $$
> H_0(XY) = \log_2(\vert A_X \vert \cdot \vert A_Y \vert) = \log_2\vert A_X \vert + \log_2 \vert A_Y \vert = H_0(X) + H_0(Y).
> $$

-----------
# Lossy compression
Let assume we want to encode of a symbol from an ensamble $X = (x, A_X, P_X)$, but we hope to use less then $H_0(X)$ bits.
For semplicty, assume
$$
\begin{aligned}
A_X &= \{a,b,c,d,e,f,g,h\}\\
P_X &= \{ {\textstyle \frac{1}{4}, \frac{1}{4}, \frac{1}{4},\frac{3}{16}, \frac{1}{64}, \frac{1}{64}, \frac{1}{64}, \frac{1}{64}}\}
\end{aligned}
$$
and $H_0(X) = 3$ bits.
A simple way to encode $X$ with less than $3$ bits is to *prune* $A_X$, selecting only the most likely elements.
More formally, let $S \subset A_X$ any subset of $A_X$.
The encode of an element $x \in A_X$ is defined as
$$
c(x) := \begin{cases}
\texttt{bin}_S(x) &\text{if } x \in S,\\
\texttt{undefined} &\text{if } x \notin S.
\end{cases}
$$
where $\texttt{bin}_S(\cdot)$ is a **binary encoding** of the elements of $S$.
Notice that $\log_2\vert S \vert < \log_2 \vert A_X \vert = H_0(X)$ bits are sufficient for $\texttt{bin}_S(\cdot)$.

By doing so, we introduced an encode of $X$ with fewer bits, but we also introduce a **risk** of not being able to decode an outcome $x$ of $X$ given its encoding $c(x)$.
The idea is to introduce a parameter $\delta$ and choose $S$ such that the probability of not being able to decode $c(x)$ is at most $\delta$, i.e.
$$
\mathbb{P}\{ c(x) \neq \texttt{undefined} \} = \mathbb{P}\{ x \notin S \} \leq \delta.
$$

> [!definition] Smallest $\delta$-suficcient subset
> The *smallest $\delta$-sufficient subset* of $A_X$ is the **smallest subset** $S_\delta \subseteq A_X$ such that
> $$
> P(x \in S_\delta) \geq 1- \delta. 
> $$

^e214df

> [!definition] The essential bit content
> The ($\delta$-)*essential bit content* of an ensambe $X$ is defined as 
> $$
> H_\delta(X) = \log_2 \vert S_\delta \vert.
> $$

^8b5cc1

> [!note] Observation
> Observe that $H_0$ is a special case of $H_\delta$, when $\delta = 0$. In fact, for $\delta = 0$, the set $A_X$ itself a smallest $\delta$-sufficient subset of $A_X$.

> [!important] Computing $S_\delta$
> There is a greedy strategy to compute $S_\delta$.
> Starting from an empyt set, we add to $S_\delta$ the elements of $A_X$ in a non-increasing order of probability $P(x)$, until the property $P(S_\delta) \geq 1-\delta$ is satisfied.


For example, considering our example sample $X$ and $\delta = 0, 1/16$ we obtain $S_0 = A_X$ and $S_{1/16} = \{a,b,c,d\}$, and encoding

| $\delta = 0$ |        |
| ------------ | ------ |
| $x$          | $c(x)$ |
| a            | 000    |
| b            | 001    |
| c            | 010    |
| d            | 011    |
| e            | 100    |
| f            | 101    |
| g            | 110    |
| h            | 111    |

| $\delta = 1/16$ |        |
| --------------- | ------ |
| $x$             | $c(x)$ |
| a               | 00     |
| b               | 01     |
| c               | 10     |
| d               | 11     |
| e               | -      |
| f               | -      |
| g               | -      |
| h               | -      |

Now let us consider the case in which we want to encode a string $x = (x_1, \dots, x_N)$ of $N$ **independent** symbols and **identically distributed**.
We denote with $X^N = (X_1, \dots, X_N)$ the ensamble of the outcome $x$, with entropy $H(X^N) = NH(X)$ (see [[#^d038d3|here]]).

It can be shown that, as $N$ grows, the function $\frac{1}{N}H_\delta(X^N)$ tends to a **constant independent** from the error probability $\delta$.
More formally, we have the following theorem

> [!theorem] Shannon's Source Coding Theorem
> Let $X$ be an ensamble with entropy $H(X)$ bits.
> For every $\varepsilon > 0$ and $0 < \delta < 1$, there exists an integer $N_0$ such that for every $N > N_0$ we have
> $$
> \left\vert \frac{1}{N}H_\delta(X^N) - H \right\vert < \varepsilon.
> $$

> [!note] Observation
> An alternative form is using the limit 
> $$
> \lim_{N \to \infty} \frac{1}{N}H_\delta(X^N) = H(X).
> $$


----
## Preliminaries: Typical Set
Let consider any ensamble $X = (x, A_X, P_X)$.
If we consider the string from $X^N$ (with large values of $N$), we have that most of them have roughly $Np_1$ occurence of the symbols $a_1$, $Np_2$ occurence of the symbol $a_2$, and so on.
Let $x_{\text{typ}}$ be a string that is **typical** in this way.
So we except that its probability is
$$
P(x_{\text{typ}}) \approx p_1^{Np_1} p_2^{Np_2} \dots p_{\vert A_X \vert}^{N p_{\vert A_X \vert}}
$$
and information content
$$
\begin{aligned}
h(x_{\text{typ}})
&= \log_2\frac{1}{P(x_{\text{typ}})}\\
&\approx \sum_{i=1}^{A_X} \log{ \left( \frac{1}{p_i^{Np_i}} \right)}\\
&= \sum_{i=1}^{A_X} \log{ \left( \frac{1}{p_i} \right)^{Np_i}}\\
&= \sum_{i=1}^{A_X} Np_i \log{\frac{1}{p_i}}\\
&= N \sum_{i=1}^{A_X} p_i \log{\frac{1}{p_i}} = N \cdot H(X).
\end{aligned}
$$

Therefore, if we consider the random variable $h(x) = \log_2{1/P(x)}$ (where $x$ is sampled at random) is more likely to be close to $N \cdot H(X)$.

Based on the previous observation, we can define the set of the **typical outcomes** of $A_X^N$ as the set of all those outcomes with probability close to $2^{-NH(X)}$.

> [!definition] The Typical Set
> Given a parameter $\beta$ that indicates how close the probability $P(x)$ of an outcome is to the probability $P(x_{\text{typ}})$, we define the *typical set* of $X^N$ as follow
> $$
> T_{N\beta} := \left\lbrace x \in A_X^N : \left\vert \frac{1}{N}\log_2\frac{1}{P(x)} - H(X) \right\vert < \beta \right\rbrace.
> $$

> [!important] An alternative definition (==INUTILE==)
> An alternative (and equivalent) definition of a typical set $T_{N\beta}$ is the following
> $$
> T_{N\beta} := \left\lbrace x \in A_X^N : \left( \frac{1}{N}\log_2\frac{1}{P(x)} - H(X) \right)^2 < \beta^2 \right\rbrace.
> $$
> Indeed, 
> $$
> \begin{aligned}
> \left( \frac{1}{N}\log_2\frac{1}{P(x)} - H(X) \right)^2 &< \beta^2\\
> \iff - \beta < \frac{1}{N}\log_2\frac{1}{P(x)} - H(X) &< \beta\\
> \iff \left\vert \frac{1}{N}\log_2\frac{1}{P(x)} - H(X) \right\vert &< \beta. \;\;\; \square
> \end{aligned}
> $$

Therefore, for every $x \in X^N$ we have
$$
\begin{aligned}
&x \in T_{N\beta}\\
\iff& - \beta < \frac{1}{N}\log_2\frac{1}{P(x)} - H(X) < \beta\\
\iff& N(H(X) -\beta) < \log_2\frac{1}{P(X)} < N(H(X)+\beta)\\
\iff& -N(H(X) + \beta) < \log_2P(X) < -N(H(X)-\beta)\\
\iff& 2^{-N(H(X)+\beta)} < P(X) < 2^{-N(H(X)- \beta)}.
\end{aligned}
$$


We are now interested in calculating the probability of $P(T_{N\beta}) = P(x \in T_{N\beta})$.
First observe that the random variable $\mathbf{x} = \frac{1}{N}\log_2\frac{1}{P(x)}$ can be written as the summation
$$
\frac{1}{N}\log_2\frac{1}{P(x)} = \frac{1}{N} \sum_{x_i \in x} \log_2\frac{1}{P(x_i)},
$$
i.e. the average of the information contents $h_i := \log_2\frac{1}{P(x_i)}$.
First observe that
$$
\mathbb{E}h_i = \sum_{j=1}^{\vert A_X \vert} p_j \cdot P(x_i = a_j) = H(X), \;\; \forall i = 1, \dots, n.
$$
While we indicate with $\sigma^2 = \text{Var}(h_1) = \dots = \text{Var}(h_n)$ the **variance** of $h_i$ for every $h_i$.

First observe that
$$
\mathbb{E}\mathbf{x} = \mathbb{E}\left( \frac{1}{N} \sum_{i=1}^{N}h_i\right) = \frac{1}{N} \sum_{i=1}^{N} \mathbb{E}h_i = H(X)
$$
while, [[Some definitions#Variance|is it known]] that
$$
\sigma^2_\mathbf{x} = \text{Var}(\mathbf{x}) = \text{Var}\left( \frac{1}{N}\sum_{i=1}^N h_i\right) = \frac{\text{Var}\left( \sum_{i=1}^N h_i\right)}{N^2} = \frac{\sigma^2}{N}.
$$
By applying [[Some inequalities#Chebyshev's inequality]] we obtain
$$
\begin{aligned}
P(x \notin T_{N\beta})
&= P\left( \left\vert \frac{1}{N}\log_2\frac{1}{P(x)} - H(X) \right\vert \geq \beta \right)\\
&= P\left( \left\vert \mathbf{x} - \mathbb{E}\mathbf{x} \right\vert \geq \beta \right)\\
&\leq \frac{\sigma^2_{\mathbf{x}}}{\beta^2} = \frac{\sigma^2}{N \beta^2}
\end{aligned}
$$
and therefore
$$
P(x \in T_{N\beta}) \geq 1 - \frac{\sigma^2}{N \beta^2}.
$$


------
## Proof of Shannon's Source Coding Theorem
### Part 1: $\frac{1}{N}H_{\delta}(X^N) < H(X) + \varepsilon$.
The smallest possible probability that a member of $T_{\beta N}$ can have is $2^{-N(H(X) + \beta)}$ (see [[#Preliminaries Typical Set]]), while the total probability of $T_{\beta N}$ can not be bigger then $1$.
Therefore
$$
\vert T_{N \beta} \vert \cdot 2^{-N(H(X)+\beta)} \leq P(T_{N\beta}) < 1.
$$
This upper-bounds the size of $T_{N\beta}$ as
$$
\vert T_{N\beta} \vert < 2^{N(H(X)+\beta)}
$$

Now, by choosing $\beta = \varepsilon$ and $N$ sufficiently large such that $\frac{\sigma^2}{N \beta^2} \leq \delta$, we have that $T_{N\beta}$ becomes a [[#^e214df|$\delta$-sufficient subset]] for $A_X$, but not necessarily the **smallest** one.
Therefore, by definition of [[#^e214df|smallest $\delta$-sufficient subset]] and [[#^8b5cc1|essential bit content]] we have that
$$
H_{\delta}(X^N) = \log_2 \vert S_\delta \vert \leq \log_2 \vert T_{N\beta} \vert < N(H(X)+ \varepsilon).
$$


### Part 2: $\frac{1}{N}H_{\delta}(X^N) > H(X) - \varepsilon$.
By contraddiction, let us assume there exists a smallest $\delta$-sufficient subset with size less then $2^{N(H(X) - \varepsilon)}$.
Let $S'$ be such subset of $A_X$.
Since $T_{N\beta}, \overline{T_{N\beta}}$ is a partition of $A_X$, we have
$$
P(x \in S') = P(x \in S'\cap A_X) = P(x \in S' \cap T_{N\beta}) + P(x \in S' \cap \overline{T_{N\beta}}).
$$
Fix $\beta = \varepsilon / 2$.
Since $T_{N\beta}$ contains all the sequences with probability at most $2^{-N(H(X) - \beta)}$, we have that first term can be upperbounded by
$$
P(x \in S' \cap T_{N\beta}) \leq \vert S' \cap T_{N\beta} \vert \cdot 2^{-N(H(X)+\beta)}.
$$
By pushing to the limit $\vert S' \cap T_{N\beta} \vert$ we obtain
$$
P(x \in S' \cap T_{N\beta}) \leq \vert S' \vert \cdot 2^{-N(H(X)+\beta)} < 2^{N(H(X) -2\beta)} \cdot 2^{-N(H(X) -\beta)} = 2^{-N\beta}.
$$

For the second term we have
$$
P(x \in S'\cap \overline{T_{N\beta}}) \leq P(x \in \overline{T_{N\beta}}) = P(x \notin T_{N\beta}) \leq \frac{\sigma^2}{N \beta^2}.
$$
By putting al together we have
$$
P(x \in S') \leq 2^{-N\beta} + \frac{\sigma^2}{N\beta^2} = 2^{-N\varepsilon/2} + \frac{\sigma^2}{N \varepsilon^2/4}.
$$

It is easy to find an $N_0$ such that for every $N \geq N_0$ we have that $P(x \in S') < 1-\delta$.
This contradict the fact that $S'$ is a $\delta$-sufficient subset of $A_X$, therefore can not exists a subset $S' \subset A_X$ such that $P(x \in S') \geq 1-\delta$ and with size at most $2^{N(H(X) - \varepsilon)}$.
$$
H_\delta(X^N) \geq N(H(X)-\varepsilon). \;\;\; \square
$$

-------
## An alternative statement

> [!theorem] Shannon’s source coding theorem (verbal statement)
> $N$ i.i.d. random variables each with entropy $H(X)$ can be compressed into more than $NH(X)$ bits with negligible risk of information loss, as $N \to \infty$; conversely if they are compressed into fewer than $NH(X)$ $bits it is virtually certain that information will be lost.
