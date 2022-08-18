# Test più comuni
## Distribuzione normale, varianza nota, media sconosciuta
Sia il [[Random Sample#Random Sample|campionamento]] $X_1, ..., X_n$ da una popolazione [[Distribuzioni#Normale|normale]] $N(\theta, \sigma^2)$, con $\theta$ sconosciuto.
Consideriamo l'*[[Hypothesis Testing#^349ec3|ipotesi nulla]]* $$H_0: \theta = \theta_0$$ e quella *alternativa* $$H_1: \theta = \theta_1$$

Consideriamo ora il valore $$T = \frac{\overline{X} - \theta_0}{\sigma/\sqrt{n}}$$ e definiamo il test $$\text{rifiuto } H_0 \iff |T| > c$$ dove $c$ verrà scelto appositamente per rendere il valore di $\alpha = P(\mathbf{X} \in C \vert \theta = \theta_0)$ (rifiuto $H_0$ nonstante sia vera) il più piccolo possibile.

Calcoliamo qual è il valore di $c$ migliore che ci da una probabilità di errore $\alpha$ abbastanza piccola
Sotto l'ipotesi nulla (ovvero se $\theta = \theta_0$) avremo che $T \sim N(0,1)$, perciò scgliendo per esempio $c = 1.96$ avremo che
$$\begin{align*}
\alpha
&= P(\mathbf{X} \in C \vert \theta = \theta_0)\\
&= P(|T| > 1.96)\\
&= 1 - P(-1.96 \leq T \leq 1.96)\\
&= 1 - (\Phi(1.96) - \Phi(-1.96))\\
&\approx 5\%
\end{align*}$$
 Se per esempio invece volessimo trovare un $c$ che ci dia una probabilità di errore più bassa, per esempio $\alpha \approx 2.5\%$ dobbiamo risolvere l'equazione in $c$
 $$\begin{align*}
0.025
&= P(\mathbf{X} \in C \vert \theta = \theta_0)\\
&= P(|T| > c)\\
&= 1 - P(-c \leq T \leq c)\\
&= 1 - (\Phi(c) - \Phi(-c))
\end{align*}$$
ovvero risolvendo $$0.975 = \Phi(c) - \Phi(-c)$$
Nel nostro caso, un buon $c$ è $2.25$.