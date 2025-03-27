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



