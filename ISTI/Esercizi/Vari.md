## Esercizi 8.1
In $n = 1000$ lanci di moneta si ottengono $560$ teste.
È ragionevole assumere che la moneta sia bilanciata?

### Svolgimento 8.1
Possiamo pensare al numero di successi come una [[Distribuzioni#Binomiale|binomiale]] $\text{Bin}(n, p)$.
Perciò consideriamo l'ipotesi $$H_0: X \sim \text{Bin}(n, 1/2)$$ 
Perciò avremo che 
$$P(X \geq 560) = P(\overline{X} \geq 0.56) \approx P\left(Z \geq \sqrt{n} \frac{0.56 - p}{\sqrt{p(1-p)}}\right)$$

Sotto $H_0$ avremo che $Z \sim N(0,1)$ e quindi che $$P(Z \geq 3.79) = 7.4e-5$$
Perciò non è ragionevole pensare che la moneta sia equilibrata.

## Esercizio 8.2
In una data città si assume che il numero di incidenti in un anno segue una distribuzione di [[Distribuzioni#Poisson|poisson]].
Negli anni passati, il numero medio di incidenti per anno era 15.
Mentre in questo anno ci sono stati 10 incidenti.

È ragionevole pensare che la probabilità di fare incidenti è diminuita?

### Svolgimento 8.2
Modelliamo il problema.
Siano $X_1, ..., X_n$ il numero di incidenti negli scorsi anni.
Sappiamo che $$\overline{X}_n = 15$$
Sia l'ipotesi $$H_0: \lambda \neq 15$$
[DA FINIRE]

## Esercizio 8.5
Sia $X_1, ..., X_n$ un [[Random Sample#Random Sample|campione]] da una popolazione con distribuzione di *Peratro*, ovvero con
$$f(x \vert \theta, \nu) = \frac{\theta\nu^\theta}{x^{\theta + 1}} \cdot 1_{\left[ \nu, \infty \right)}(x)$$ per $\nu, \theta > 0$.

Più semplicemente
$$f(x \vert \theta, \nu) =
\begin{cases}
\dfrac{\theta\nu^\theta}{x^{\theta + 1}} &\text{se } x \geq \nu\\
0 &\text{altrimenti}
\end{cases}$$

1. Trovare il [[Stimatore di Massima Verosimiglianza#Stimatore di Massima Verosimiglianza - MLE|MLE]] di $\theta$ e $\nu$.
2. Dimostrare che [[I test "migliori"#Test del Rapporto di Verosimiglianza generalizzato - LRT|LRT]] di $$H_0: \theta = 1, \nu \text{ sconosciuto}$$ contro $$H_1: \theta \neq 1, \nu \text{ sconosciuto}$$
ha **regione critica** della forma $$\{ \mathbf{x} : T(\mathbf{x}) \leq c_1 \lor T(\mathbf{x}) \geq c_2 \}$$ dove $0 < c_1 < c_2$ e 
$$T(\mathbf{X}) = \log{\left( \frac{\prod_{i=1}^{n} X_i}{(\min_i X_i)^2}  \right)}$$
3. Dimostrare che sotto $H_0$, la statistica $2T$ ha distribuzione [[Distribuzioni#Chi Quadro|chi-quadro]] e trovare i gradi di libertà. (*Hint:* trova la distribuzione congiunta degli $n-1$ termini non banali $X_j/(\min_i X_i)$ condizionata $\min_i X_i$. Metti insieme questio $n-1$ termini e osserva che la distribuzione di $T$ <u>dato</u> $min_i X_i$ **non dipende** da $\min_i X_i$, ottenendo così la distribuzione del solo $T$).

### Svolgimento 8.5
#### Punto 1
Sia la [[Verosimiglianza#Likelihood function|funzione di massima verosimiglianza]] in questione
$$L(\theta, \nu \vert \mathbf{x}) = f_{\mathbf{X}}(\mathbf{x} \vert \theta, \nu) = \prod_{i=1}^{n}f(x_i \vert \theta, \nu)$$
Osservare che tale valore è $\neq 0$ se e solo se **tutti** i termini $f(x_i \vert \theta, \nu)$ della produttoria sono $\neq 0$, e ciò accade quando $\nu \leq \min_i X_i$.

Perciò possiamo riscrivere il tutto in funzione della v.a. *indicatrice* $1(\{\nu \leq \min_i x_i\})$.
$$L(\theta, \nu \vert \mathbf{x}) = \frac{\theta^n \nu^{n\theta}}{\left( \prod_{i=1}^{n} x_i \right)^{\theta+1}} \cdot 1(\{ \nu \leq \min_i x_i\})$$

Osserviamo che, fissando un $\theta > 0$, tale funzione è **crescente** in $\nu$, finché $\nu \leq \min_i x_i$.
Perciò $$\hat\nu = \min_{1 \leq i \leq n} X_i$$
Percio il MLE per $\nu$ **non dipende** da $\theta$.

Ora poniamoe $\nu = \hat\nu$, e cerchiamo di **massimizzare** $\theta$.

Sia $$\tilde{L}(\theta, \hat\nu \vert \mathbf{x}) = \ln{L(\theta, \hat\nu \vert \mathbf{x})} = n\ln{\theta} + n\theta\ln{\hat\nu} - (\theta +1)\sum_{i=1}^{n}\ln{x_i}$$
e $$S(\theta, \hat\nu \vert \mathbf{x}) = \frac{\partial}{\partial \theta} \tilde{L}(\theta, \hat\nu \vert \mathbf{x}) = \frac{n}{\theta} + n\ln{\hat\nu} - \sum_{i=1}^{n}\ln{x_i}$$
Per via della **monotonia** di $\tilde{L}(\theta, \hat\nu \vert \mathbf{x})$ avremo che un punto di massimo per quel $\hat\theta$ tale che $S(\hat\theta, \hat\nu \vert \mathbf{x}) = 0$.

Ovvero quando
$$\begin{align*}
\frac{1}{\hat\theta}
&= \frac{1}{n}\sum_{i=1}^{n}\ln{x_i} - \ln{\hat\nu}\\
&= \frac{1}{n}\ln{\left( \prod_{i=1}^{n}x_i \right)} - \ln{\hat\nu}\\
&= \frac{1}{n}\ln{\left( \prod_{i=1}^{n}x_i \right)} - \ln{\hat\nu}\\
&= \frac{1}{n}\left(\ln{\left( \prod_{i=1}^{n}x_i \right)} - n\ln{\hat\nu} \right)\\
&= \frac{1}{n}\ln{\left( \frac{\prod_{i=1}^{n}x_i}{\hat\nu^n} \right)}\\
&= \frac{1}{n}\ln{\left( \frac{\prod_{i=1}^{n}x_i}{(\min_i x_i)^n} \right)}\\
&= \frac{T}{n}
\end{align*}$$
Perciò il MLE per $\theta$ è $\hat\theta = n/T$.

#### Punto 2
