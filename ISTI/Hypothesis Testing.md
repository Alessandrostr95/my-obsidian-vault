# Hypothesis Testing
A fronte di un [[Random Sample#Random Sample|campionamento]] $\mathbf{X}$ da una popolazione $f(\mathbf{X} \vert \theta)$, un **test d'ipotesi** cerca di **decidere** se il campionamento $\mathbf{X}$ è compatibile rispetto ad una data **ipotesi** riguardante il parametro **sconosciuto** $\theta$.

Dato che $\theta$ appartiene ad uno *spazio di parametri* $\Theta$, un'ipotesi è semplicemente un'asserzione del tipo $$\theta \in \Theta_0 \subset \Theta$$ detta anche **ipotesi nulla** e segnata con $H_0$.

Rifiutare l'ipotesi nulla equivale semplicemente nell'accettare il suo complemento, chiameremo **ipotesi alternativa** e la indicheremo con $H_A$ o $H_1$, e sono del tipo $$\theta \in \Theta_0^c \equiv \Theta \setminus \Theta_0$$

Un test d'ipotesi è quindi semplicemente una **regola** che, per ogni possibile campione $\mathbf{X}$, ci dice se **rifiutare o no** l'ipotesi nulla $H_0$. ^349ec3
> **Esempio**
> Prendiamo un campione $X_1,...,X_n$ di una popolazione $N(\theta, \sigma^2)$, con $\theta$ sconosciuto.
> Consideriamo una ipotesi $$H_0: \theta \leq 17$$
> Un test famoso consiste nel <u>rifutare</u> $H_0$ se e solo se $\overline{X} > 17 + \sigma/\sqrt{n}$.

Il sottoinsieme $C$ dei dati che portano al **rifuto** di $H_0$ è detta **regione critica**.
> Nell'esempio precedente, la regione critca equivale al sottoinsieme di $\mathbb{R}^n$ di tutti quei $n$-uple $\mathbf{x}$ tali che $$\frac{1}{n}\sum_{i=1}^{n}x_i > 17 + \frac{\sigma}{\sqrt{n}}$$

La probabilità quindi che $H_0$ sia rifiutata equivale quindi alla probabilità di campionare all'interno della regione critica, e tale probabilità è nota come **funzione critica** $$\psi(\mathbf{X}) = P(\text{rifiuto } H_0) = P(\mathbf{X} \in C)$$

Può capitare che un test dia un risultato errato.
Bisogna distinguere gli errori di un test di ipotesi, perché spesso nella realtà i diversi tipi di errori hanno gravità differenti.
Per esempio, dire a una persona malata che in realtà è in salute è un errore molto più grave del dire a una persona in salute che in realtà ha una malattia.
Distinguiamo quindi:
- **Errore di I tipo**: rifiutare $H_0$ quando in realtà $H_0$ è vera. In termini di eventi avremo $$\mathbf{X} \in C \;\vert\; \theta \in \Theta_0$$ ^361948
- **Errore di II tipo**: accettare $H_0$ quando in realtà $H_0$ è falsa. In termini di eventi avremo $$\mathbf{X} \in C^c \;\vert\; \theta \in \Theta^c_0$$

![|600](isti_test_ipotesi_errors.png)

Per comodità, indicheremo
$$\alpha = P(\mathbf{X} \in C \;\vert\; \theta \in \Theta_0)$$
$$\beta = P(\mathbf{X} \in C^c \;\vert\; \theta \in \Theta^c_0)$$

Osserviamo che possiamo sempre ricondurre un errore di tipo II ad un errore di tipo I e viceversa, semplicemente esprimendolo in funzione dell'*ipotesi alternativa*.
Infatti dire *"accettare $H_0$ quando in realtà $H_0$ è falsa"* equivale a dire *"rifiutare $H_1$ quando in realtà $H_1$ è vera"*.

Perciò, convenzionalmente, si sceglie sempre qual è l'errore più grave e lo si esprime sempre come errore di tipo I, utilizzando opportunamente $H_0$ o $H_1$.


## Potenza di un Test
Dato un test, definiamo la **potenza del test** come la probabilità di rifiutare l'*ipotesi nulla* in funzione del parametro $\theta$.
$$\beta(\theta) = P(\mathbf{X} \in C \vert \theta)$$
Alcune volte ci si riferisce alla potenza di un test con $\Pi(\theta)$.

> **Esempio - Bernoulli trials**
> Supponiamo di aver lanciato una moneta per $10$ volte, e vogliamo sapere se è equilibrata $p=1/2$ oppure no.
> Per esempio un test può essere quello di accettare o rifiutare l'ipotesi nulla $$H_0: p = \frac{1}{2}$$
> Consideriamo un test banale, per esempio il test che rifiuta $H_0$ se si ottengono *nessuna* oppure *tutte* teste, ovvero $$\text{rifiuto } H_0 \iff \sum_{i=1}^{10}X_i \in \{0, 10\}$$
> Avremo quindi che la potenza di questo test sarà $$\beta(p) = (1-p)^{10} + p^{10}$$
> ![|500](isti_test_ipotesi_1.png)

> **Esempio - Normale**
> Consideriamo ancora una volta un [[Distribuzioni#Normale|densità normale]] del tipo $$f(x \vert \theta) = N(\theta, 25)$$ con <u>media ignota</u>.
> Sia l'*ipotesi nulla* $$H_0: \theta \leq 17$$ e il test $$\text{rifiuto } H_0 \iff \overline{X}_n > 17 + \frac{5}{\sqrt{n}}$$
> Tale test avrà potenza $$\beta(\theta) = P(\overline{X}_n > 17 + 5/\sqrt{n})$$
> Sappiamo anche che $\overline{X}_n \sim N(\theta, 25/n)$.
> Perciò, normalizzando opportunamente, avremo che $$\begin{align*}
\beta(\theta)
&= P(\overline{X}_n > 17 + 5/\sqrt{n})\\
&= 1 - P(\overline{X}_n \leq 17 + 5/\sqrt{n})\\
&= 1 - P\bigg(\underbrace{\frac{\overline{X}_n -\theta}{5/\sqrt{n}}}_{\sim N(0,1)} \leq \frac{17 + 5/\sqrt{n} - \theta}{5/\sqrt{n}}\bigg)\\
\\
&= 1 - \Phi\left(\frac{17 + 5/\sqrt{n} - \theta}{5/\sqrt{n}}\right)
\end{align*}$$
> Ricordiamo che $\Phi(x)$ è la **funzione di ripartizione** di un [[Distribuzioni#Normale Standard|Normale Standard]].
> 
> Osserviamo cosa accade alla potenza del test con un dato $n$ fissato
> ![|500](isti_test_ipotesi_2.png)
> 
> Supponiamo che il valore reale di $\theta$, chiamiamolo $\theta^*$, sia $\theta^* = 15$.
> Al crescere di $n$ avremo che $\beta(\theta^*) = \beta(15) \to 0$ tenderà a $0$.
> Il che è giusto, in quanto $\beta$ equivale alla probabilità di incorrere in un errore di tipo I, ovvero di rifiutare $H_0$ sapendo che in realtà $H_0$ è corretta.
> ![|500](isti_test_ipotesi_4.png) 
>
> Simmetricamente avremo che se $\theta^* = 19$ avremo che la probabilità di rifiutare $H_0$, ovvero la potenza $\beta(\theta^*)$ tende a $1$.
> E ciò è corretto perché $\theta^* > 17$.
> 
> La regione invece più incerta è qulla intermedia attorno a $17$.
> Ovviamente, la funzione ideale sarebbe qulla "*a gradini*" che assume $0$ per valori $\theta \leq 17$ e $1$ per $\theta > 17$.
> Osserivamo però, che al crescere del numero di campioni $n$, la funzione potenza $\beta(\theta)$ tende sempre di più ad approssimare la funsione ideale
> ![|500](isti_test_ipotesi_3.png)

## Ampiezza di un test
Da prima abbiamo visto che la **potenza di un test** è una funzione che calcola la probabilità di *rifiutare* $H_0$ al variare del parametro incognito $\theta$.

Consideriamo un'ipotesi nulla generica, del tipo $$H_0 : \theta \in \Theta_0$$ e un test $T$ che indica qunado rifiutare $H_0$.

Allora averemo che l'**ampiezza** del test $T$ è la quantità definita come $$\sup_{\theta \in \Theta_0} \beta(\theta)$$ ovvero la probabilità pià alta che abbiamo di commettere un **[[#^361948|errore di I tipo]]**.

Perciò, più è piccola l'ampiezza di un test, più è *qualitativamente* migliore.