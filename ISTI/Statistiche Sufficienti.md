Spesso un esperimento fa uso di un [[Random Sample#Random Sample|campionamento]] $\mathbf{X} = (X_1, ..., X_n)$ per cercare di inferire qualche informazione riguardo un **parametro sconosciuto** $\theta$.
Per fare ciò fi fa uso di **statistiche**, ovvero funzioni definite sul supporto di un campionamento.

Ogni statistica $T(\mathbf{X})$ definisce una sorta di *riduzione* o *riassunto* delle informazioni contenute nei dati, estraendone solamente quelle significative.
Si può pensare alla mappa $\mathbf{X} \to T(\mathbf{X})$ come una *riduzione di complessità* che non perde informazione su $\theta$.

# Statistica Sufficiente
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



## Esempio - Media campionaria di Normali
Sia un campionamento $X_1, ..., X_n$ di normali $N(\mu, \sigma^2)$, con $\sigma^2$ noto e $\mu$ sconosciuto.
Sia la statistica $T(\mathbf{X}) = \overline{X}$.
Si vuole quindi stimare se la [[Random Sample#Media campionaria|media campionaria]] è una statistica sufficiente per $\mu$.

Iniziamo col scrivere la [[Distribuzioni Multivariate#Joint PDF|densità congiunta]] di $\mathbf{X}$
$$\begin{align*}
f_{\mathbf{X}}(x_1, ..., x_n)
&= \prod_{i=1}^{n}f_{X_i}(x_i)\\
&= \prod_{i=1}^{n} (2\pi\sigma^2)^{-1/2}\exp{\left[ -\frac{1}{2\sigma^2}(x_i - \mu)^2 \right]}\\
&= (2\pi\sigma^2)^{-n/2}\exp{\left[ -\frac{1}{2\sigma^2}\sum_{i=1}^{n}(x_i - \mu)^2 \right]}\\
&= (2\pi\sigma^2)^{-n/2}\exp{\left[ -\frac{1}{2\sigma^2}\sum_{i=1}^{n}(x_i - \overline{x} + \overline{x} - \mu)^2 \right]}
\end{align*}$$

Cerchiamo di riformulare la sommatoria all'esponente in maniera più utile
$$\begin{align*}
\sum_{i=1}^{n}(x_i - \overline{x} + \overline{x} - \mu)^2
&= \sum_{i=1}^{n}((x_i - \overline{x}) + (\overline{x} - \mu))^2\\
&= \sum_{i=1}^{n}(x_i - \overline{x})^2 + 2\sum_{i=1}^{n}(x_i - \overline{x})(\overline{x} - \mu) + \sum_{i=1}^{n}(\overline{x} - \mu)^2\\
&= \sum_{i=1}^{n}(x_i - \overline{x})^2 + 2(\overline{x} - \mu)\sum_{i=1}^{n}(x_i - \overline{x}) + n(\overline{x} - \mu)^2
\end{align*}$$

La sommatoria $\sum_i x_i - \overline{x}$ è pari a $0$, infatti
$$\begin{align*}
\sum_{i=1}^{n} x_i - \overline{x}
&= \sum_{i=1}^{n} x_i - \sum_{i=1}^{n}\overline{x}\\
&= (x_1 + ... + x_n)  - n \overline{x}\\
&= (x_1 + ... + x_n)  - (x_1 + ... + x_n) = 0\\
\end{align*}$$

Pericò proseguendo che
$$f_{\mathbf{X}}(x_1, ..., x_n) = (2\pi\sigma^2)^{-n/2}\exp{\left[ - \left( \sum_{i=1}^{n}(x_i - \overline{x})^2 + n(\overline{x} - \mu)^2 \right)/(2\sigma^2) \right]}$$

Vediamo ora la distribuzione condizionata a $\overline{X}$
$$\begin{align*}
f_{\mathbf{X} \vert \overline{X} = x^*}(x_1, ..., x_n, x^*)
&= \frac{(2\pi\sigma^2)^{-n/2}\exp{\left[ - \left( \sum_{i=1}^{n}(x_i - x^*)^2 + n(x^* - \mu)^2 \right)/(2\sigma^2) \right]}}{(2\pi\sigma^2/n)^{-1/2}\exp{\left[ -n(x^* - \mu)^2/(2\sigma^2)  \right]}}\\
&= n^{1/2} (2\pi\sigma^2)^{-(n-1)/2} \exp{\left( - \frac{1}{2\sigma^2}\sum_{i=1}^{n}(x_i - x^*)^2 \right)} 
\end{align*}$$

Si vede chiaramente che tale dinsità è **indipendente** dal valore di $\mu$, perciò abbiamo dimostrato che, nel caso di normali, la media campionaria è una statistica sufficiente per il parametro $\mu$.

--------------------------------
La ricerca di statistiche sufficienti per un campione può essere semplificata dal Factorization Theorem

# Teorema di Fattorizzazione
Sia il campione $\mathbf{X} = X_1, ..., X_n$ dipendente da un parametro $\theta$, e sia la sua [[Distribuzioni Multivariate#Joint PDF|densità congiunta]] $f_{\theta}(\mathbf{x})$.

Una statistica $T(\mathbf{X})$ è *sufficiente* per il paramtro $\theta$ **se e solo se** per ogni punto $\mathbf{x}$ e ogni parametro $\theta$, la densità $f_\theta$ può essere fattorizzata come $$f_{\theta}(\mathbf{x}) = g_{\theta}(T(\mathbf{x})) \cdot h(\mathbf{x})$$ dove $g_{\theta}$ è una funzione definita sulla statistica $T$ e dipendente dal parametro in questione $\theta$, mentre $h$ è una funzione definita nei soli valori $x_1, ..., x_n$ e **indipendente** da $\theta$.

Una notazione equivalente <u>che verrà utilizzata in maniera intercambiabile</u> è la seguente $$f(\mathbf{x} \vert \theta) = g(T(\mathbf{x}) \vert \theta) \cdot h(\mathbf{x})$$

## Esempio - continuo esempio Normale
Riprendiamo l'[[#Esempio - Media campionaria di Normali|esempio precedente]], avevamo che $$f_{\mathbf{X}}(x_1, ..., x_n \vert \mu) = (2\pi\sigma^2)^{-n/2}\exp{\left[ - \left( \sum_{i=1}^{n}(x_i - \overline{x})^2 + n(\overline{x} - \mu)^2 \right)/(2\sigma^2) \right]}$$  ^188fc0

Scomponendo l'esponente avremo 
$$f_{\mathbf{X}}(x_1, ..., x_n \vert \mu) = (2\pi\sigma^2)^{-n/2}\exp{\left( - \sum_{i=1}^{n}(x_i - \overline{x})^2 /(2\sigma^2) \right)} \exp{\left( -n(\overline{x} - \mu)^2/(2\sigma^2) \right)}$$

Perciò ponendo $$g(T(x_1, ..., x_n) \vert \mu) = \exp{\left( -n(\overline{x} - \mu)^2/(2\sigma^2) \right)}$$ e $$h(x_1, ..., x_n) = (2\pi\sigma^2)^{-n/2}\exp{\left( - \sum_{i=1}^{n}(x_i - \overline{x})^2 /(2\sigma^2) \right)}$$ troviamo la *fattorizzazione* desidarata $$f_{\mathbf{X}}(x_1, ..., x_n \vert \mu) = h(x_1, ..., x_n) \cdot g(T(x_1, ..., x_n) \vert \mu)$$

## Esempio - Statistica sufficiente Uniforme
Sia il campione $X_1, ..., X_n$ di v.a. **discrete** i.i.d. $U\left[ 1, \theta \right]$, dove $\theta$ è il parametro sconosciuto che si vuole stimare.


La densità di ogni v.a. è 
$$f(x_i \vert \theta) = \begin{cases}
\frac{1}{\theta} &\text{se } x_i \leq \theta\\
0 &\text{altrimenti}
\end{cases}$$

Mentre la densita congiuta risulta essere
$$f(x_1, ..., x_n \vert \theta) = \begin{cases}
\theta^{-n} &\text{se } x_i \in \{1, ..., \theta\} \; \forall i = 1, ..., n\\
0 &\text{altrimenti}
\end{cases}$$

La condizione $x_i \in \{1, ..., \theta\} \; \forall i = 1, ..., n$ può essere espressa come 
$$\left[ x_i \in \mathbb{N} \; \forall i = 1, ..., n \right] \;\; \land \;\; \left[ \max_{i=1,...,n} x_i \leq \theta \right]$$

Consideriamo quindi la statistica $$T(\mathbf{X}) = \max_{X_i \in \mathbf{X}} X_i$$

Possiamo quindi **fattorizzare** $f(x_1, ..., x_n \vert \theta)$ come segue
$$g(T(\mathbf{x}) \vert \theta) = \begin{cases}
\theta^{-n} &\text{se } T(\mathbf{x}) \leq \theta\\
0 &\text{altrimenti}
\end{cases}$$
e
$$h(\mathbf{x}) = \begin{cases}
1 &\text{se } \mathbf{x} \in \mathbb{N}^n\\
0 &\text{altrimenti}
\end{cases}$$

Infatti avremo che $$f(\mathbf{x} \vert \theta) = g(T(\mathbf{x}) \vert \theta) \cdot h(\mathbf{x})$$
quindi per il [[#Teorema di Fattorizzazione]] $T$ è una statistica sufficiente per $\theta$.

## Esempio Multivariato - Statistica sufficiente Normale (Media e Varianza)
Sia il campione normale $X_1,...,X_n$ di $N(\mu, \sigma^2)$ indipendenti.
In questo caso però (per sfortuna) sia $\mu$ che $\sigma^2$ non sono noti.
Perciò in questo caso avremo il **vettore** di parametri $$\pmb{\theta} = (\mu, \sigma^2)$$

Definiamo quindi una statistica multivariata $T(\mathbf{X}) = (T_1(\mathbf{X}), T_2(\mathbf{X})) = (\overline{X}, S^2)$ come segue
$$\begin{align*}
T_1(\mathbf{x}) &= \overline{x} = (x_1 + ... + x_n)\\
T_2(\mathbf{x}) &= s^2 = \frac{1}{n-1}\sum_{i=1}^{n}(x_i - \overline{x})^2
\end{align*}$$

Ci si chede se $T$ è una statistica sufficiente per il parametro (multivariato) $\pmb{\theta}$.

Vogliamo quindi trovare una fattorizzazione di $f_{\mathbf{X}}(\mathbf{x} \vert \mu, \sigma^2)$ del tipo
$$f_{\mathbf{X}}(\mathbf{x} \vert \mu, \sigma^2) = h(\mathbf{x}) \cdot g\left( T_1(\mathbf{x}), T_2(\mathbf{x}) \vert \mu, \sigma^2 \right)$$

Osserviamo che la densità congiunta $f_{\mathbf{X}}(\mathbf{x} \vert \mu, \sigma^2)$ di $n$ normali indipendenti (vedi [[#^188fc0|qua]]) è una valida funzione $g(\cdot, \cdot \vert \mu, \sigma^2))$ infatti
$$g(\mathbf{t} \vert \pmb{\theta}) = g(t_1, t_2 \vert \mu, \sigma^2) = \exp{\left[ -\frac{1}{2\sigma^2} \left( (n-1)t_2+ n(t_1 - \mu)^2 \right) \right]}$$
ricordando che $\sum_i (x_i - \overline{x})^2 = (n-1)s^2$.

In fine, ponendo $h(\mathbf{x}) = 1$ otterremo che possiamo fattorizzare $$f_{\mathbf{X}}(\mathbf{x} \vert \pmb{\theta}) = g(T(\mathbf{x}) \vert \pmb{\theta}) \cdot h(\mathbf{x})$$ ovvero che $\overline{X}, S^2$ sono una statistica sufficiente per $\mu, \sigma^2$ nel modello normale.

-----------------------------
# Statistica Sufficiente Minimale
In precedenza abbiamo visto che una statistica sufficiente è semplicemente un funzione che *"riduce la complessità"* di un campionamento, senza però perdere informazioni riguardo un determinato parametro.

Dato il fatto che possono esseistere molteplici statistiche sufficienti, ci si può chiedere se esistono statistiche *"più sufficienti"* di altre, ovvero funzioni che riducono al minimo la complessità dei dati.

Infatti, supponiamo di avere una statistica sufficiente $T(\mathbf{X})$ su un campione $\mathbf{X}$, e supponiamo che esista una fuzione $r(T(\mathbf{X})) = T^*(\mathbf{X})$.
Se $r$ è invertibile, ovvero esiste $r^{-1}$, allora si può dimostrare che anche $T^*$ è una statistica sufficiente, infatti
$$f(\mathbf{x} \vert \theta) = g(T(\mathbf{x}) \vert \theta) \cdot h(\mathbf{x})$$
$$\implies f(\mathbf{x} \vert \theta) = g\left(r^{-1}(T^*(\mathbf{x}) \vert \theta \right) \cdot h(\mathbf{x})$$
Ponendo $g^*(x \vert \theta) = g(r^{-1}(x) \vert \theta)$ avremo che $$\implies f(\mathbf{x} \vert \theta) = g^*\left(T^*(\mathbf{x}) \vert \theta\right) \cdot h(\mathbf{x})$$ ovvero $T^*$ è una statistica sufficiente.

## Def. Statistica Sufficiente Minimale
Una statistica sufficiente $T(\mathbf{X})$ è detta **statistica sufficiente minimale** se, per ogni altra statistica sufficiente $T'$, $T$ è in funzione di $T'$. 


Osserviamo dire che $T$ è in funzione di $T'$ equivale a dire che se $T'(\mathbf{x}) = T'(\mathbf{y})$ allora $T(\mathbf{x}) = T(\mathbf{y})$, per ogni coppia di punti $\mathbf{x}, \mathbf{y}$ su cui $T$ è definito.

## Teorema 6.2.13 - Condizione sufficiente
Sia una statistica sufficiente $T(\mathbf{X})$ su un campionamento $X_1,...,X_n$ con densità congiunta $f(\mathbf{x} \vert \theta)$.
Se per ogni coppia di punti $\mathbf{x}, \mathbf{y}$ tali che $T(\mathbf{x}) = T(\mathbf{y})$ abbiamo che il quoziente $f(\mathbf{x} \vert \theta) / f(\mathbf{y} \vert \theta)$ è una **costante indipendente da** $\theta$, allora avremo che $T$ è una **statistica sufficiente minimale**.

### Esempio - Campione Esponenziale
Sia il campione $X_1, ..., X_n$ di [[Distribuzioni#Esponenziale|esponenziali]]  $\text{Exp}(\lambda)$, con densità congiunta
$$f(\mathbf{x} \vert \lambda) = \lambda^n \exp(-\lambda(x_1 + ... + x_n))$$

Studiamo il quoziente $$\frac{f(\mathbf{x} \vert \lambda)}{f(\mathbf{y} \vert \lambda)} = \frac{\lambda^n \exp(-\lambda(x_1 + ... + x_n))}{\lambda^n \exp(-\lambda(y_1 + ... + y_n))} = \exp{\left(-\lambda\left(\sum_{i=1}^{n}x_i - \sum_{i=1}^{n}y_i \right)\right)}$$
Questo quoziente diventa indipendente dal parametro $\lambda$ (ovvero *scompare*) solamente quando $\sum_i x_i = \sum_i y_i$.

Per esempio, una statistica $T$ per la quale si ha un quoziente costante è $T(x_1, ..., x_n) = x_1 + ... + x_n$.
Con questa statistica, se troviamo due punti $\mathbf{x}, \mathbf{y}$ tali che $T(\mathbf{x}) = x_1 + ... + x_n = y_1 + ... + y_n = T(\mathbf{y})$, avremo un **quoziente costante** (pari ad 1 in questo caso).

È facile anche dimostrare (tramite il [[#Teorema di Fattorizzazione]]) che $T$ è anche una statistia sufficiente per $\lambda$, infatti basta porre
$$g(T(\mathbf{x}) \vert \lambda) = \lambda^n \exp{(-\lambda T(\mathbf{x}))}$$ e $$h(\mathbf{x}) = 1$$
Dimostrando così che $T$ è una **statistica sufficiente minimale** per $\lambda$.

