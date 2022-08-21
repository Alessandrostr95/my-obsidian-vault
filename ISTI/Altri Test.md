## Campione Esponenziale
Abbiamo un [[Random Sample#Random Sample|campione]] [[Distribuzioni#Esponenziale|esponenziale]] $X_1, ..., X_n$ di parametro $\lambda$ <u>sconosciuto</u>.
Consideriamo l'*[[Hypothesis Testing#^349ec3|ipotesi nulla]]* $$H_0: \lambda = \lambda_0$$ e definiamo la statistica $$T = \lambda_0 \sum_{i=1}^{n}X_i$$
Sappiamo che la somma di $n$ esponenziali di parametro $\lambda$ segue una distribuzione $\Gamma(n, 1/\lambda)$, con **notazione Shape-Scale** (vedi [[Distribuzioni#^9fe8ff|proprietà distribuzione esponenziale]]).
Allora, sotto ipotesi nulla avremo che $$T \sim \lambda_0 \Gamma(n, 1/\lambda_0) = \Gamma(n,1)$$ per [[Distribuzioni#Cambiamento di scala|cambio di scala di una gamma]].

```julia
using Plots, Distributions

n = 10
λ = 3

X = rand(Exponential(λ), n, 10000)
Xₛᵤₘₛ = [sum(X[:, i]) for i=1:10000]

Γₙ₁ = Gamma(n, 1)

histogram(Xₛᵤₘₛ/λ, label="T")
plot!(0:0.01:25, x -> 5000 * pdf(Γₙ₁, x), lw=2, label="Γ(n,1)", c=:red)
plot!(yaxis=false)
```
![|600](isti_altri-test_1.png)

Possiamo quindi trovare un **intervallo di accettazione** $\left[a,b\right]$ tale che possiamo definire il seguente test $$\text{rifiuto } H_0 \iff T \not\in \left[ a,b \right]$$
Possiamo anche *parametrizzare* tale test per ottenere una *probabilità di errore di tipo I* $\alpha$ desiderata.
$$\begin{align*}
\alpha
&= P(T \not\in \left[ a,b \right])\\
&= 1 - P(a \leq T \leq b)\\
&= 1 - (F(b) - F(a))
\end{align*}$$


## Sfruttare le proprietà asintotiche di MLE
Sia un generico campione $X_1,...,X_n$ dipendente da un parametro $\theta$ <u>sconosciuto</u>.
Definiamo l'*ipotesi nulla* $$H_0: \theta = \theta_0$$

Consideriamo uno [[Stimatore di Massima Verosimiglianza#Stimatore di Massima Verosimiglianza - MLE|MLE]] $\hat\theta_{ML}$ per $\theta$.

Sotto ipotesi nulla $H_0$, sappiamo che [[Stimatore di Massima Verosimiglianza#^c3b404|asintoticamente avremo]] $$T = \sqrt{nI(\theta_0)} \cdot {(\hat\theta_{ML} - \theta_0)}\sim N(0, 1)$$
Periò possiamo applicare lo [[Test più comuni#z-test|z-test]].