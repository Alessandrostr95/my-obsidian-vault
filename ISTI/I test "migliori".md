Abbiamo visto che ci sono svariati modi di definire un test.
Quello che ci si può chiedere è se dobbiamo necessariamente ogni volta ricavarci un test definendo una statistica opportuna (vedi per esempio [[Test comuni#z-test|z-test]] o [[Test comuni#t-test di Student|t-test]]), oppure se esiste una famiglia di test "**migliore**" a livello qualitativo rispetto agli altri.


## Test "più potente"
Abbiamo visto che l'[[Hypothesis Testing#Ampiezza di un test|ampiezza]] di un test è una sorta di *misura* della sua qualità.

Un **test più potente** di ampiezza $\alpha$ è un test $T^*$ per le ipotesi $$H_0: \theta = \theta_0; \; H_1: \theta = \theta_1$$ tale che
1. la [[Hypothesis Testing#Potenza di un Test|potenza]] del test $T^*$ calcolata in $\theta_0$ è pari ad $\alpha$ $$\beta_{T^*}(\theta_0) = \alpha$$
   Osserviamo che dato che siamo un contesto con ipotesi **semplici**, allora avremo che $\Theta_0 = \{\theta_0\}$, perciò dire che $T^*$ ha potenza $\alpha$ equivale a dire che ha anche [[Hypothesis Testing#Ampiezza di un test|ampiezza]] $\alpha$ ([[Hypothesis Testing#Osservazione|vedi]]). Stiamo quindi **fissando** ad $\alpha$ l'errore di primo tipo.
2. Per ogni altro test $T$ di ampiezza $\leq \alpha$ allora deve valere che $$\beta_{T^*}(\theta_1) \geq \beta_{T}(\theta_1)$$ ovvero la probabilità di commettere un [[Hypothesis Testing#^f0afe5|errore di II tipo]] è più alta.

## Test del Rapporto di Verosimiglianza Semplice - simple LRT
Il **test del Rapporto di Verosimiglianza Semplice** funziona nel caso particolare in cui abbiamo **ipotesi semplici**, del tipo $$H_0: \theta = \theta_0$$
$$H_1: \theta = \theta_1$$

Tale test consiste nel **rifiutare** $H_0$ se la [[Random Sample#^46a026|statitisca]] $$\lambda = \frac{L(\theta_0 | X_1,...,X_n)}{L(\theta_1 | X_1,...,X_n)}$$ (ovvero il rapporto tra le [[Verosimiglianza#Likelihood function|funzioni di verosimiglianza]] calcolati in $\theta_0$ e $\theta_1$) è **minore** di un certo valore $k$ **fissato** in modo tale da ottenere l'[[Hypothesis Testing#Ampiezza di un test|ampiezza]] desiderata. 
$$\text{rifiuto } H_0 \iff \lambda < k$$ ^224e66

Si può dimostrare che (sempre nel caso di **ipotesi semplici**) il test del rapporto di verosimiglianza semplice è **più potente**.

## Teorema *(Lemma di Neyman-Pearson)*
Sia un [[Random Sample#Random Sample|campione]] $X_1, ..., X_n$ da una popolazione $f_X(\cdot \vert \theta)$, ovvero dipendente da un parametro $\theta$ **ignoto**.
Poniamoci in uno spazio dei parametri con soli due elementi $$\Theta = \{\theta_0, \theta_1\}$$
Perciò avremo solo due possibili ipotesi <u>semplici</u> $$H_0: \theta = \theta_0; \; H_1: \theta = \theta_1$$
Siano $k^* >0$ e la [[Hypothesis Testing#^6568cf|regione critica]] $C^* \in \mathbb{R}^n$ tali che:
1. la [[Hypothesis Testing#Potenza di un Test|potenza]] (e quindi anche l'[[Hypothesis Testing#Ampiezza di un test|ampiezza]] perché siamo nel contesto semplice) del test è $\alpha$ $$\beta(\theta_0) = P((X_1, ..., X_n) \in C^* \vert \theta = \theta_0) = \alpha$$
2. considerando il [[#^224e66|rapporto di verosimiglianza semplice]] $\lambda$ avremo che $$\begin{cases}\lambda \leq k^* &\text{se } (X_1,..., X_n) \in C^*\\ \lambda > k^* &\text{se } (X_1, ..., X_n) \not\in C^*\end{cases}$$
Osserviamo che $k^*$ è totalmente definito da $\lambda$ e $C^*$.

Sotto le ipotesi del teorema, avremo che il test $T^*$ con regione critica $C^*$ è un **test più potente di ampiezza** $\alpha$ per le ipotesi semplici $H_0, H_1$.

### Esempio - Esponenziale
Sia un campione [[Distribuzioni#Esponenziale|esponenziale]] $X_1, ..., X_n$ di parametro $\theta$ sconosciuto.

Siano le ipotesi <u>semplici</u> $$H_0: \theta = \theta_0; \; H_1: \theta = \theta_1$$

Calcoliamo il rapporto di verosimiglianza semplice $$\lambda = \frac{\theta_0^n \cdot e^{-\theta_0 \sum_i x_i}}{\theta_1^n \cdot e^{-\theta_1 \sum_i x_i} } = \left(\frac{\theta_0}{\theta_1}\right)^n e^{(\theta_1 - \theta_0)\sum_i x_i}$$
Avremo che $$\lambda \leq k^* \iff \ln{\lambda} \leq \ln{k^*} \iff \sum_{i=1}^{n}x_i \leq \frac{1}{(\theta_1 - \theta_0)}\ln{\left( \left(\frac{\theta_1}{\theta_0}\right)^n k^*\right)} = k'$$
Ricavando $k'$ si può in maniera molto semplice ricavare anche $k^*$.

Perciò il test $$\text{rifiuto } H_0 \iff X_1 + ... + X_n \leq k'$$ è un **test più potente di ampiezza** $\alpha$.

Vediamo ora come ricavare $k'$ in funzione di $\alpha$.
Osserviamo che $$\alpha = \beta(\theta_0) = P_{\theta_0}(X_1 + ... + X_n \leq k') = F_{X_1 + ... + K_n}(k')$$
Ricordiamo che la somma di $n$ v.a. esponenziali i.i.d. è una [[Distribuzioni#Gamma|gamma]] $\Gamma(n, 1/\theta)$.
Perciò avremo che $$\alpha = \beta(\theta_0) = \int_{0}^{k'} \frac{\theta_0^n}{\Gamma(n)} x^{n-1}e^{\theta_0 x} \, dx$$ ovvero $\alpha$ è un equazione in $k'$.

Fissando quindi $\alpha$ possiamo quindi ricavarci il $k'$, e di conseguenza il valore di $k^*$.

-------------
## Test del Rapporto di Verosimiglianza (generalizzato) - LRT
Consideriamo ora il caso più generale di ipotesi del tipo $$H_0: \theta \in \Theta_0$$ e $$H_1: \theta \in \Theta \setminus \Theta_0$$

Il **rapporto di verosimiglianza generalizzato** è la quantità $$\lambda_n = \frac{\sup_{\theta \in \Theta_0} L(\theta \vert X_1, ..., X_n)}{\sup_{\theta \in \Theta} L(\theta \vert X_1, ..., X_n)}$$ ovvero il rapporto del superiore della verosimiglianza per $\theta \in \Theta_0$ (ovvero nell'ipotesi nulla) e il superiore di <u>tutti</u> i valori possibili di $\theta$.

```ad-important
title: Osservazione importante!
Osservare che il valore di $\theta$ che massimizza il denominatore di $\lambda_n$ equivale al [[Stimatore di Massima Verosimiglianza#Stimatore di Massima Verosimiglianza - MLE|MLE]] $\hat\theta_{ML}$.

```


Il **test del rapporto di verosimiglianza (generalizzato)** (o **LRT**) è definito come $$\text{rifiuto } H_0 \iff\lambda_n \leq k^*$$ dove $k^*$ è definito dall'[[Hypothesis Testing#Ampiezza di un test|ampiezza]] $\alpha$ del test (come prima).

Ossevare che dato che la [[Verosimiglianza#Likelihood function|funzione di verosimiglianza]] dipende da delle distribuzioni, allora avremo che anche $\lambda_n$ seguirà una distribuzione in fuzione dei dati $X_1, ..., X_n$.
Ci riferiremo alla distribuzione di $\lambda_n$ con $$\Lambda_n(X_1, ..., X_n)$$
In genere non è sempre facile studiare $\lambda_n$ però esistono dei risultati asintotici sulla distribuzione $\Lambda_n$ che si possono sfruttare.
Si può infatti dimostrare che $$-2\log{\Lambda_n} \sim \chi^2$$ per $n$ grande (ma *"non troppo"*).

Pericò, fissando un **quantile** $\phi_{1-\alpha}$ della $\chi^2$, possiamo definire il test $$\text{rifiuto } H_0 \iff -2\log{\lambda_n} > \phi_{1-\alpha}$$ dove $\alpha$ è l'ampiezza del test.

### Esempio - Normal LRT
Sia un campione [[Distribuzioni#Normale|normale]] $X_1, ..., X_n$ da una popolazione $N(\theta, 1)$
Consideriamo le ipotesi $$H_0: \theta = \theta_0; \; H_1: \theta \neq \theta_0$$
Avremo quindi $\Theta_0 \equiv \{\theta_0\}$ e $\Theta_1 \equiv \Theta \setminus \{\theta_0\}$.

Calcoliamo il **rapporto di verosimiglianza generalizzato** $$\lambda_n = \frac{\sup_{\theta \in \Theta_0} L(\theta \vert X_1, ..., X_n)}{\sup_{\theta \in \Theta} L(\theta \vert X_1, ..., X_n)}$$

Osserviamo che essendo $\theta_0$ l'<u>unico</u> elemento di $\Theta_0$ avremo come numeratore $L(\theta_0 \vert X_1, ..., X_n)$.
Invece al denominatore avremo che il $\theta$ che lo massimizza è il [[Stimatore di Massima Verosimiglianza#Stimatore di Massima Verosimiglianza - MLE|MLE]] $\hat\theta_{ML}$ di una normale.
Sappiamo da [[Stimatore di Massima Verosimiglianza#Esempio - Normale]] che lo stimatore di ML di una normale è la sua [[Random Sample#Media campionaria|media campionaria]] $$\hat\theta_{ML} = \overline{X}$$
Perciò avremo che
$$\begin{align*}
\lambda_n
&= \frac{L(\theta_0 \vert X_1,..., X_n)}{L(\overline{X} \vert X_1, ..., X_n)}\\
&= \frac{(2\pi)^{-n/2}\exp\left[ -\sum_{i=1}^{n}(x_i - \theta_0)^2/2 \right]}{(2\pi)^{-n/2}\exp\left[ -\sum_{i=1}^{n}(x_i - \overline{x})^2/2 \right]}\\
&= \exp\left[ \frac{1}{2}\left( \sum_{i=1}^{n}(x_i - \overline{x})^2 -\sum_{i=1}^{n}(x_i - \theta_0)^2\right) \right]
\end{align*}$$

Semplifichiamo ora
$$\begin{align*}
\sum_{i=1}^{n}(x_i - \theta_0)^2
&= \sum_{i=1}^{n}((x_i - \overline{x})  + (\overline{x} - \theta_0))^2\\
&= \sum_{i=1}^{n}(x_i - \overline{x})^2 + 2(\overline{x}-\theta_0)\sum_{i=1}^{n}(x_i - \overline{x}) + \sum_{i=1}^{n}(\overline{x} - \theta_0)^2\\
&= \sum_{i=1}^{n}(x_i - \overline{x})^2 + 2(\overline{x}-\theta_0)(\underbrace{(x_1 + ... + x_n)}_{n\overline{x}} - n\overline{x}) + n(\overline{x} - \theta_0)^2\\
&= \sum_{i=1}^{n}(x_i - \overline{x})^2 + n(\overline{x} - \theta_0)^2
\end{align*}$$

Perciò $$\lambda_n = \exp\left[ -\frac{1}{2}n(\overline{x} - \theta_0)^2 \right]$$

Definiamo quindi la [[Hypothesis Testing#^6568cf|regione critica]] $$C \equiv \{\mathbf{x} \in \mathbb{R}^n : \lambda_n \leq k\} \equiv \Bigg\{\mathbf{x} \in \mathbb{R}^n : \vert \overline{x} - \theta_0 \vert \geq  \sqrt{-2\frac{\log{k}}{n}} \Bigg\}$$
Dato che $0 < k \leq 1$ avremo che $\log{k} \leq 0$, perciò l'*argomento* della radice sarà positivo (ottenendo un valore reale a destra della disuguaglianza).