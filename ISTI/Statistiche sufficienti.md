Spesso un esperimento fa uso di un [[Random Sample#Random Sample|campionamento]] $\mathbf{X} = (X_1, ..., X_n)$ per cercare di inferire qualche informazione riguardo un **parametro sconosciuto** $\theta$.
Per fare ciò fi fa uso di **statistiche**, ovvero funzioni definite sul supporto di un campionamento.

Ogni statistica $T(\mathbf{X})$ definisce una sorta di *riduzione* o *riassunto* delle informazioni contenute nei dati, estraendone solamente quelle significative.
Si può pensare alla mappa $\mathbf{X} \to T(\mathbf{X})$ come una *riduzione di complessità* che non perde informazione su $\theta$.

# Statistica sufficiente
In genere siamo interessati a definire (e trovare) statistiche che siano in qualche modo *"migliori"* di altre.

Supponiamo di aver trovato una statistica tale che qualsiasi altra statistica non riesce ad estrapolare altre informazioni in più dal campione, ovvero una statistica *"sufficiente"* ai nostri scopi.
Una **statistica sufficiente** per un parametro sconosciuto $\theta$ è una funzione che in qualche modo racchiude tutte le informazioni riguardo $\theta$ che sono contenute all'interno di $\mathbf{X}$.

Più formalmente, una statistica $T(\mathbf{X})$ è una **statistica sufficiente** per $\theta$ se la distribuzione condizionata di $\mathbf{X}$ dato il valore di $T(\mathbf{X})$ è **indipendente** da $\theta$.

In maniera intuitiva, fissiamo lo spazio definito da $T(\mathbf{X}) = t$: siamo interessati a conosce informazioni riguardo la distribuzione di tutti quei $X_1, ..., X_n$ tali che $T(\mathbf{X}) = t$.
Ovvero vogliamo calcolare la distribuzione congiunta $$f_{\mathbf{X} \vert T(\mathbf{X}) = t}(\mathbf{x}, t)$$
Se tale distribuzione **indipendente** dal parametro $\theta$ (dal quale $\mathbf{X}$ dipdende) allora possiamo affermare che $T$ è una statistica sufficiente.

## Esempio - Bernoulliane
Consideriamo una serie di bernoulliane indipendenti $\mathbf{X} = (X_1, ..., X_n)$ di parametri $\theta = p$.
Consideriamo una prima statistica $T(\mathbf{X}) = X_1$: essa è sufficiente ?

Calcoliamo quindi $$f_{\mathbf{X} | X_1 = x^*}(x_1, ..., x_n, x^*)$$
Innanzitutto sappiamo che $$f_{\mathbf{X}}(x_1, ..., x_n) = p^{x_1 + ... + x_n}(1-p)^{n - (x_1 + ... x_n)}$$ perciò $$f_{\mathbf{X} | X_1 = x^*}(x_1, ..., x_n, x^*) = p^{x^* + x_2 ... + x_n}(1-p)^{n - (x^* + x_2 + ... + x_n)}$$
Questa purtroppo non è una statistica sufficiente perché la distribuzione condizionata dipende ancora da $p$.

Consideriamo una seconda statistica $S(\mathbf{X}) = X_1 + ... + X_n$.
Essendo $S$ una somma di bernoulliane indipendenti, allora possiamo affermare che $S \sim \text{Bin}(n, p)$.
Allora 
$$\begin{align*}
f_{\mathbf{X} | X_1 + ... X_n = s}(x_1, ..., x_n, s)
&= \frac{p^{s}(1-p)^{n - s}}{\binom{n}{s}p^s(1-p)^s}\\
&= \frac{1}{\binom{n}{s}}
\end{align*}$$
ovvero una uniforme.

Perciò possiamo dire che $S$ è una statistica sufficiente per $p$.

## Esempio - Poisson
Sia $X_1, ..., X_n$ un campionamento con v.a. di $\text{Poisson}(\lambda)$ (vedi [[Distribuzioni#Poisson]]).
La sua [[Distribuzioni Multivariate#Joint PMF|densità congiunta]] riuslta quindi essere $$f_{\mathbf{X}}(x_1, ..., x_n) = \prod_{i=1}^{n}f_{X_i}(x_i) = \frac{\lambda^{x_1 + ... + x_n}e^{-n\lambda}}{x_1!\cdots x_n!}$$
Come prima, poniamo la statistica $T(\mathbf{X}) = X_1 + ... + X_n$.

Essendo $T$ una somma di Poisson indipendenti, allora avremo che $T$ sarà anchessa una poisson di parametro $n\lambda$.
Perciò
$$\begin{align*}
f_{\mathbf{X} | X_1 + ... X_n = s}(x_1, ..., x_n, s)
&= \left(\frac{\lambda^{s}e^{-n\lambda}}{x_1!\cdots x_n!}\right) \Big/ \left( \frac{(n\lambda)^{s}e^{-n\lambda}}{s!}\right)\\
&= \frac{s!}{x_1! \cdots x_n!}\frac{1}{n^2}
\end{align*}$$
ovvero $T$ è una statistica sufficiente.

