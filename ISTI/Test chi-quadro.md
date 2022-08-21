# Test goodness-of-fit (bontà d'adattamento)
Supponiamo di avere un campione di dati $\mathbf{X} = X_1, ..., X_n$, e che esistano $k$ **classi di eventi possibili** $A_1, ..., A_k$.
Ovvero ogni elemento $X_i$ può risolversi in uno solo dei $k$ possibili eventi.
Perciò, indichiamo con $p_j$ la probabilità che avvenga l'evento $j$-esimo $$P(X_i \in A_j) = p_j \;\; \forall i=1,...,n$$

Dato che gli eventi formano una partizione di $\Omega$, avremo quindi che $$\sum_{i=1}^{k} p_i = 1$$

Possiamo modellare il nostro esperimento con una [[Distribuzioni#Multinomiale multivariata|distribuzione multinomiale]].
Indichiamo con $$Y_j = \sum_{i=1}^{n} 1(\{X_i \in A_j\}) \;\; \forall j =1, ..., k$$ ovvero $Y_j$ **conta** il numero di volte che nel campione $\mathbf{X}$ occorre l'evento $j$-esimo $E_j$.

Avremo quindi che $$\mathbf{Y} = (Y_1, ..., Y_k) \sim \text{Multi}(n, k, (p_1, ..., p_k))$$

Dato che i parametri $\mathbf{p} = (p_1, ..., p_k)$ sono ignoti, consideriamo la seguente *[[Hypothesis Testing#^349ec3|ipotesi nulla]]* $$H_0: \mathbf{p} = \mathbf{p}^{(0)}$$

Definiamo ora la statistica $$T = \sum_{i=1}^{k}\frac{(Y_i - np^{(0)}_i)^2}{np^{(0)}_i}$$
(Vedere media distribuzione multinomiale!).

È possibile dimostrare ([TO DO]) che (**sotto ipotesi nulla**) *asintoticamente* $T \sim \chi^2_{k-1}$ segue una distribuzione [[Distribuzioni#Chi Quadro|chi-quadro]] con $k-1$ gradi di libertà.
Il numero di gradi di libertà è $k-1$ perché siamo vincolati dal fatto che $p_1 + ... + p_k = 1$.

Riconducendoci quindi ad una distribuzione *chi-quadro* siamo in gradi di eseguire uno [[Test più comuni#t-test di Student|t-test]].
Consultando quindi la [[tabella dei quinatili di una chi-quadro]] possiamo facilmente definire un test con precisione $1-\alpha$.

Un modo più semplice ed empirico per vedere questo test è il seguente:
> Supponiamo di avere $A_1, ..., A_k$ come insieme di possibili eventi.
> Prendiamo un campione di $n$ elementi.
> Siano $O_1, ..., O_k$ la frequenza (il numero di volte) con il quale i singoli eventi si sono presentati, dette anche **frequenze osservate**.
> Supponiamo che ci aspettavamo di osservare le frequenze $E_1, ..., E_k$, dette **frequenze teoriche** o **attese**.
> Nel nostro caso $E_i$ sta ad indicare $np^{(0)}_i$, per ogni evento $i$.
> 
> Evento | A_1 | A_2 | ... | A_k
> --|--|--|--|--
> Frequenze osservate | O_1 | O_2 | ... | O_k
> Frequenze attese | E_1 | E_2 | ... | E_k
> 
>Allora la nostra statistica sarà $$T = \sum_{i=1}^{k}\frac{(O_i - E_i)^2}{E_i} \sim \chi^2_{k-1}$$

