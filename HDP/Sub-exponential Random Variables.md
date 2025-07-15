The class of [[Sub-gaussian distributions|sub-gaussian distributions]] is natural and quite large.
Nevertheless, it leaves out some important distributions whose tails are heavier than gaussian.

For example, consider the random vector $g = (g_1, \dots, g_N) \in \mathbb{R}^N$, where $g_i \in N(0,1)$ are **independent**.
In many application it is usefull to hace concentration inequality for the Euclidean norm
$$
\Vert g \Vert_2 = \left( \sum_{i=1}^{N} g_i^2 \right)^{1/2}
$$

Since $\Vert g \Vert_2^2$ is a sum of independent random variables $g_i^2$, we should expect some concentration to hold.
On the other hand, although $g_i$ are [[Sub-gaussian random variables]], $g_i^2$ are not.
Indeed, by [[Why concentration inequalities?#^e0aac6|Tails of the normal distribution]] 
$$
\mathbb{P}\{g^2 \geq t\} = \mathbb{P}\{\vert g \vert \geq \sqrt{t}\}
= 2\mathbb{P}\{g \geq \sqrt{t}\} \approx e^{-(\sqrt{t})^2/2} = e^{-t/2}
$$

# Sub-exponential properties
Let $X$ be any random variable.
Then the following properties are equivalent [[#^81bd75|up to constant factor]]:

1. There exists $K_1 > 0$ such that the tails of $X$ satisfy $$\mathbb{P}\{\vert X \vert \geq t\} \leq 2 \exp(-t/K_1),\;\;\; \forall t \geq 0$$
2. There exists $K_2 > 0$ such that the [[Some definitions#$L p$ norm of a random variable|moments]] of $X$ satisfy $$\Vert X \Vert_{L^p} = (\mathbb{E}\vert X \vert^p)^{1/p} \leq K_2p,\;\;\; \forall p \geq 1$$
3. There exists $K_3 > 0$ such that the [[Some definitions#MGF - Moment Generating Function|MGF]] of $\vert X \vert$ satisfies $$M_{\vert X \vert}(\lambda) =\mathbb{E}\exp(\lambda \vert X \vert) \leq \exp(K_3\lambda),\;\;\; \forall \lambda : 0 \leq \lambda \leq \frac{1}{K_3}$$
4. There exists $K_4 > 0$ such that the [[Some definitions#MGF - Moment Generating Function|MGF]] of $\vert X \vert$ is **bounded** at some point, namely $$M_{\vert X \vert}(1 / K_4) =\mathbb{E}\exp(\vert X \vert / K_4) \leq 2$$
 ^3f06fc
5. Moreover, if $\mathbb{E}X = 0$ we have another equivalent property, i.e., there exists $K_5 > 0$ such that the [[Some definitions#MGF - Moment Generating Function|MGF]] of $X$ satisfies $$M_X(\lambda) = \mathbb{E}\exp(\lambda X) \leq \exp(K_5^2\lambda^2),\;\;\; \forall \lambda:\vert \lambda \vert \leq \frac{1}{K_5}$$


```ad-important
The parameters $K_i > 0$ appearing in these properties differ from each other by at most an absolute constant factor.
I.e., there exists an **absolute constant** $C$ such that property $i$ implies property $j$ with parameter $K_j \leq CK_i$ for any two properties $i, j = 1, ... , 5$.
```

^81bd75


-----

# Sub-exponential Random Variables

> [!definition] Sub-exponential random variables
> Any random variable $X$ that satisfies one of the [[#Sub-exponential properties|properties]], is said **sub-exponential** random variable.

^5dbae2


> [!definition] Sub-exponential norm
> The **sub-exponential norm** $\Vert \cdot \Vert_{\psi_1}$ of a [[#^5dbae2|sub-exponential random variable]] $X$ is defined as the **smallests** value of $K_4$ in [[#^3f06fc|property 4]], i.e.
> $$
> \Vert X \Vert_{\psi_1} := \inf\{t > 0 : \mathbb{E}\exp(\vert X \vert/t) \leq 2\}
> $$


## Sub-exponential is sub-gaussian squared
A random variable $X$ is [[Sub-gaussian random variables|sub-gaussian]] if and only if $X^2$ is [[#^5dbae2|sub-exponential]].
Moreover
$$
\Vert X^2 \Vert_{\psi_1} = \Vert X \Vert_{\psi_2}^2
$$

By definition, $\Vert X^2 \Vert_{\psi_1}$ is the minimum $K \geq 0$ s.t. $\mathbb{E}\exp(X^2/K) \leq 2$, while $\Vert X \Vert_{\psi_2}$ is the minimum $L \geq 0$ s.t. $\mathbb{E}\exp(X^2/L^2) \leq 2$.
Therefore, since $K = L^2$, we have
$$
\Vert X^2 \Vert_{\psi_1} := K = L^2 =: \Vert X \Vert_{\psi_2}^2 \;\;\; \square
$$

## Product of sub-gaussians is sub-exponential
Let $X,Y$ two [[Sub-gaussian random variables]].
Then $XY$ is [[#^5dbae2|sub-exponential]].
Moreover
$$
\Vert XY \Vert_{\psi_1} \leq \Vert X \Vert_{\psi_2} \cdot \Vert Y \Vert_{\psi_2} 
$$

==TODO==


------
# Centering sub-exponential
Let $X$ a [[#Sub-exponential Random Variables|sub-exponential random variable]], with [[Some definitions#Mean|mean]] $\mathbb{E}X$.
Then
$$
\Vert X - \mathbb{E}X \Vert_{\psi_1} \leq C \Vert X \Vert_{\psi_1}
$$
for some absolute constant $C$.
