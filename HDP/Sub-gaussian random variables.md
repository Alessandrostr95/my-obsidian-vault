---
tags:
  - HDP
date: 2025-03-23
---

> [!definition] Sub-gaussian random variables
> Any random variable $X$ that satisfies one of the [[Sub-gaussian properties#Sub-gaussian properties|properties]], is said **sub-gaussian** random variable.

^8458de


> [!definition] Sub-gaussian Norm
> The **sub-gaussian norm** $\Vert \cdot \Vert_{\psi^2}$ of a [[#^8458de|sub-gaussian random variable]] $X$ is defined as the **smallests** value of $K_4$ in [[Sub-gaussian properties#^51fe83|property 4]], i.e.
> $$
> \Vert X \Vert_{\psi^2} := \inf\{t > 0 : \mathbb{E}\exp(X^2/t^2) \leq 2\}
> $$

^2d6cf4


> [!proof] $\Vert \cdot \Vert_{\psi^2}$ is a norm #exercise 
> Since $K_4$ is greather then $0$ by [[Sub-gaussian properties#^51fe83|definition]], then $\Vert X \Vert_{\psi^2} \geq 0$ for every random variable $X$.
> ==TODO==


We can restate the [[Sub-gaussian properties]] in terms of [[#^2d6cf4|sub-gaussian norm]].
More precisely we have
- [[Sub-gaussian properties#^b5dd62|Property 1]]: $\mathbb{P} \{\vert X \vert \geq t\} \leq 2 \exp(-ct^2/\Vert X \Vert_{\psi^2})$, for every $t \geq 0$; ^fedbd4
- [[Sub-gaussian properties#^51312a|Property 2]]: $\Vert X \Vert_{L^p} = (\mathbb{E}\vert X \vert^p)^{1/p} \leq C \Vert X \Vert_{\psi^2} \sqrt{p}$, for every $p \geq 1$;
- [[Sub-gaussian properties#^51fe83|Property 4]]: $\mathbb{E}\exp(X^2/\Vert X \Vert_{\psi^2}^2) \leq 2$;
- [[Sub-gaussian properties#^70f6af|Property 5]]: if $\mathbb{E}X = 0$ then $M_X(\lambda) = \mathbb{E}\exp(\lambda X) \leq \exp(\lambda^2 C \Vert X \Vert_{\psi^2}^2)$. ^7ff2f7

where $C,c > 0$ are [[Sub-gaussian properties#^47bb35|absolute constants]].

> [!important] 
> Up to absolute constant factors, $\Vert X \Vert_{\psi2}$  is the **smallest possible number** that makes each of these inequalities valid.

^e7688e




