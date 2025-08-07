---
author: Alessandro Straziota
tags:
  - Probability
  - Math
  - IT
---

> [!definition] (Binary) Symbol Code
> A **(binary) symbol code** $c$ for an [[Shannon's Source Coding Theorem#^6980d9|ensamble]] $X$ is a **mapping** from $A_X$ to $\{0,1\}^+$.
> We will denote with $c(a_i)$ the **codeword** corresponding to $a_i$, with $l_i = l(a_i)$ the length of $c(a_i)$, for every $a_i \in A_X$.
> The **extended code** $c^+$ is the mapping from $A_X^+$ to $\{0,1\}^+$, obtained by the concatenation of the corresponding codewords, i.e.
> $$
> c^+(x_1x_2\dots x_N) = c(x_1)c(x_2)\dots c(x_N)
> $$

> [!definition] Uniquely Decodable
> A symbol code $c$ is **uniquely decodable** if $c^+$ has no two distinct strings with the same encoding, i.e.
> $$
> \forall \mathbf{x}, \mathbf{y} \in A^+_X, \; \mathbf{x} \neq \mathbf{y} \implies c^+(\mathbf{x}) \neq c^+(\mathbf{y})
> $$

> [!definition] Prefix Code
> A symbol code is called **prefix code** if no codeword is a prefix of any other codeword.
> Formally, for every $x, y \in A_X$, let $c(x) = b_1^x \dots b_n^x$ and  $c(y) = b_1^y \dots b_m^y$.
> Without loss of generality assume $n \leq m$.
> Therefore
> $$
> b_1^x = b_2^y \land \dots \land b_n^x=b_n^y \iff x = y
> $$

#### Examples

| i   | c              | Uniquely Decodable | Prefix Code |
| --- | -------------- | ------------------ | ----------- |
| 1   | {0, 101}       | ✅                  | ✅           |
| 2   | {1, 101}       | ✅                  | ❌           |
| 3   | {0,10,110,111} | ✅                  | ✅           |
| 4   | {00,01,10,11}  | ✅                  | ✅           |
| 5   | {0,1,00,11}    | ❌                  | ❌           |
| 6   | {0,01,011,111} | ✅                  | ❌           |
> [!important] 
> Observe that $C_6$ is uniquely decodable since its codewords are the reverse of $C_3$'s codewords.
> It's interesting that $C_3$ is also a prefix code, while $C_6$ (its reverse) is not.

> [!theorem] Kraft Inequality
> For any **uniquely decodable** code $C$, the codeword lengths must satisfy
> $$
> \sum_{i=1}^{\vert A_X \vert} 2^{-l_i} \leq 1.
> $$



