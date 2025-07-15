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
> Let $x = (x_1, \dots, x_N)$ be a string of $N$ independent and identically distributed symbols from the same ensamble $X$. Let $X^N = (X_1, \dots, X_N)$ be the ensamble of $x$, with $X_1 = \dots = X_N = X$.
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
> $H_0(X)$ is also a **lower bound** for the number of **binary question** that are always guaranteed to identify an outcome from the ensamble $X$.

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

> [!definition] The essential bit content
> The ($\delta$-)*essential bit content* of an ensambe $X$ is defined as 
> $$
> H_\delta(X) = \log_2 \vert S_\delta \vert.
> $$

> [!note] Observation
> Observe that $H_0$ is a special case of $H_\delta$, when $\delta = 0$. In fact, for $\delta = 0$, the set $A_X$ itself a smallest $\delta$-sufficient subset of $A_X$.


Now let us consider the case in which we want to encode a string $x = (x_1, \dots, x_N)$ of $N$ **independent** symbols and **identically distributed**.
We denote with $X^N = (X_1, \dots, X_N)$ the ensamble of the outcome $x$, with entropy $H(X^N) = NH(X)$ (see [[#^d038d3|here]]).

