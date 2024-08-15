Nel 1978 l'informatico Robert Morris affrontò il problema di contare un grande numero di eventi in registri molto piccoli, ad 8 bits. L'approccio banale di utilizzare un contatore ad 8 bit consente di tenere traccia di soli 255 eventi.
Se si rilassa la richiesta di contare **esattamente** tutti gli eventi, allora con una piccola quantità di bits consente di tenere traccia di decisamente molti più eventi, purché ammettendo un piccolo errore di approssimazione.

Più formalmente il problema richiede di costruire una struttura dati che mantiene nel tempo un contatore $n$, ammettendo le seguenti operazioni:
- **update()**: incrementa il contatore $n$ di una unità
- **query()**: ritorna (una stima di) $n$

Come accennato in precedenza, la soluzione banale del contatore esatto richiederebbe $\Omega(\log_2{n})$ bits di memoria.
L'obiettivo è quello di definire un **contatore probabilistico** che utilizzi $o(\log{n})$ bit di spazio e che ritorni una stima $\tilde{n}$ del valore $n$ tale che $$P(\vert \tilde{n} - n \vert \geq \varepsilon n) \leq \delta$$ per alcuni parametri $\varepsilon, \delta \in (0,1)$ dati in input all'inizio dell'algoritmo.

 
$$\begin{align*}\\ 
&\textbf{Algorithm } \text{Morris' counter}\\ 
&\textbf{Input: } \text{A stream of $n$ events and a desired relative error $0 < ϵ < 1$.}\\ 
&\textbf{Output: } \text{A $(1\pm\varepsilon)$-approximation $\tilde{n}$ of $n$ with failure probability $\leq \delta$}\\
\\
&1.\;\;\text{Initialize a counter } X\\
&2.\;\;\textbf{For each update do: }\\
&3.\;\;\;\;\;\;\text{increment } X \text{ with probability } 2^{-X}\\
&4.\;\;\textbf{For each query do: }\\
&5.\;\;\;\;\;\;\textbf{return } 2^X - 1\\
\end{align*}$$

In questo algoritmo lo stimatore $\tilde{n}$  è $2^X-1$.
## Analisi
Sia $X_n$ il valore del contatore $X$ a seguito di $n$ operazioni di update.

> [!claim]
>   $$E[ 2^{X_n} ] = n+1$$
^d58e3d

Il [[#^d58e3d|Claim 1]] si può dimostrare per *induzione* su $n$.
Il caso base per $n=0$ è chiaro.
Per la [[Basic concept and properties#Law of Total Expectation|Law of Total Expectation]] segue il passo induttivo
$$\begin{align*}
E\left[ 2^{X_{n+1}} \right]
&= \sum_{j \geq 0} P(X_n = j) \cdot E\left[ 2^{X_{n+1}}\mid X_n = j \right]\\
&= \sum_{j \geq 0} P(X_n = j) \cdot \left( 2^{j+1} \cdot 2^{-j} + 2^{j} \cdot (1-2^{-j}) \right)\\
&= \sum_{j \geq 0} P(X_n = j) \cdot \left( 2^{j} + 1 \right)\\
&= \sum_{j \geq 0} P(X_n = j) \cdot 2^{j} + \sum_{j \geq 0} P(X_n = j)\\
&= E\left[ 2^{X_n} \right] + 1\\
&= (n + 1) + 1 = n + 2
\end{align*}$$

Il [[#^d58e3d|Claim 1]] inoltre implica che $\tilde{n}$ è un [[Stimatori Puntuali#^a1f3b4|unbiased estimator]] di $n$.

Il nostro obiettivo ora è dimostrare che $P(\vert \tilde{n} - n \vert \geq \varepsilon n) \leq \delta$.
Usiamo ora il [[Concentration Inequalities#Chebyshev's inequality]] 
$$\begin{align*}
P(\vert \tilde{n} - n \vert \geq \varepsilon n)
&\leq \frac{\text{Var}(\tilde{n})}{\varepsilon^2n^2}\\
&= \frac{ E\left[ \tilde{n}^2 \right] - E[\tilde{n}]^2}{\varepsilon^2n^2}\\
&= \frac{E\left[ (2^{X_n}-1)^2\right] - n^2}{\varepsilon^2n^2}\\
&= \frac{E[2^{2X_n}-2\cdot2^{X_n}+1]-n^2}{\varepsilon^2n^2}\\
&= \frac{E[2^{2X_n}] - 2 \cdot E[2^{X_n}]-n^2+1}{\varepsilon^2n^2}
\end{align*}$$

^ad5899

> [!claim]
> $$E[ 2^{2X_n} ] = \frac{3}{2}n^2 + \frac{3}{2}n+1$$
^610e3d

Anche il [[#^610e3d|Calim 2]] può essere dimostrato per induzione su $n$.
Per $n = 0$ è chiaro che $E[2^0]= 1$.
Per la [[Basic concept and properties#Law of Total Expectation|Law of Total Expectation]] segue il passo induttivo
$$\begin{align*}
E[2^{2X_n}]
&= \sum_{j\geq 0} P(X_j=j) \cdot E[2^{2X_{j+1}} \mid X_j=j]\\
&= \sum_{j\geq 0} P(X_j=j) \cdot (2^{2(j+1)}2^{-j} + 2^{2j}(1-2^{-j}))\\
&= \sum_{j\geq 0} P(X_j=j) \cdot (2^{j+2} + 2^{2j}-2^{j})\\
&= 4 \cdot \sum_{j\geq 0} P(X_j = j) \cdot 2^j + \sum_{j \geq 0} P(X_j = j)\cdot2^{2j}- \sum_{j\geq 0} P(X_j = j) \cdot 2^j\\
&= 3 \cdot E[2^{X_{n-1}}] + E[2^{2X_{n-1}}]\\
&= 3 n + \frac{3}{2}(n-1)^2+ \frac{3}{2}(n-1)+1\\
&= 3n + \frac{3}{2}n^2 -3n +\frac{3}{2} + \frac{3}{2}n - \frac{3}{2}+1\\
&= \frac{3}{2}n^2+\frac{3}{2}n + 1
\end{align*}$$


Andando a sostituire alla [[#^ad5899|precedente disuguaglianza]]
$$\begin{align*}
P(\vert \tilde{n} - n \vert \geq \varepsilon n)
&\leq \frac{E[2^{2X_n}] - 2 \cdot E[2^{X_n}]-n^2+1}{\varepsilon^2n^2}\\
&= \frac{\frac{3}{2}n^2+\frac{3}{2}n+1-2(n+1)-n^2+1}{\varepsilon^2n^2}\\
&= \frac{\frac{1}{2}n^2-\frac{1}{2}n}{\varepsilon^2n^2}\\
&\leq \frac{1}{2\varepsilon^2}
\end{align*}$$

Questo risultato non è particolarmente informativo, dato che la probabilità che lo stimatore si discosta di una quantità $\varepsilon n$ dal valore effettivo è minore di $1/2$ solo quando $\varepsilon > 1$.

--------
# Morris+
Per diminuire la probabilità di errore del Morris' Counter, dobbiamo usare il [MEAN TRICK].
Istanziamo quindi $s$ copie (**almeno**) [[#k-wise indipendence|pair-wise indipendenti]] dell'algoritmo di Morris, e facciamo la **media** dei loro output.
Siano $\tilde{n}_1, \dots, \tilde{n}_s$ gli stimatori restituiti dalle $s$ istanze indipendenti, e definiamo il nuovo stimatore
$$\tilde{n} = \frac{1}{s}\sum_{i=1}^{s}\tilde{n}_i$$
Dato che ogni $\tilde{n}_i$ è uno [[Stimatori Puntuali#^a1f3b4|stimatore unbiased]] di $n$ (vedi [[#^d58e3d|Claim 1]]), allora anche la loro media aritmetica $\tilde{n}$ è uno stimatore unbiased di $n$.
Il vantaggio di usare $\tilde{n}$ è la sua varianza più piccola.
Dato che la [[Basic concept and properties#Varianza di somma di v.a. indipendenti|varianza della somma]] di v.a. [[#k-wise indipendence|pair-wise indipendenti]] è esattamente la somma delle varianze, allora avremo che
$$\begin{align*}
\text{Var}(\tilde{n})
&= \text{Var}\left( \frac{1}{s}\sum_{i=1}^{s}\tilde{n}_i\right)\\
&= \frac{1}{s^2}\sum_{i=1}^{s}\text{Var}(\tilde{n}_i)\\
&= \frac{1}{s}\text{Var}(\tilde{n}_1)
\end{align*}$$
dove
$$\begin{align*}
\text{Var}{(\tilde{n}_i)}
&= E[2^{2X_n}] - 2 \cdot E[2^{X_n}]-n^2+1\\
&= \frac{n^2-n}{2}\\
&\leq \frac{n^2}{2}
\end{align*}$$

Perciò avremo che la probabilità di errore ora è 
$$P(\vert \tilde{n} - n \vert \geq \varepsilon n) \leq \frac{1}{2s\varepsilon^2} \leq \delta$$
Per sapere quanto deve valere $s$ affinché tale probabilità sia minore di un certo valore $\delta$ basta porre $s \geq \dfrac{1}{2\delta \varepsilon^2} \in \Omega\left( \delta^{-1}\varepsilon^{-2} \right)$.

-----
# Morris++
Osserviamo che in [[#Morris +]] ci sta una dipendenza dalla probabilità di errore delta $\delta$.
Infatti, il numero di istanze $s$ di Morris che dobbiamo utilizzare è proporzionale a $1/\delta$, i quali potrebbero risultare troppi se si vuole sia una probabilità di errore bassa che uno scarto $\varepsilon n$ piccolo.

Supponiamo di avere a disposizione l'algoritmo [[#Morris +]] con probabilità di errore **costante** $\delta$.
Possiamo usare il [MEDIA TRICK] per ridurre la dipendenza dalla probabilità di errore $\delta$ da $1/\delta$ a $\log(1 / \delta)$.

Supponiamo che l'algoritmo a disposizione abbia error probability $\delta = 1/3$, ed eseguiamo $t$ istanze **indipendenti** di Morris+.

```ad-important
Dato che ogni istanza di Morris+ ha un error probability **costante**, allora avremo che $s = \Theta(\varepsilon^{-2})$, abbiamo quindi rimosso da $s$ un fattore $1/\delta$.
```

Una volta eseguite queste $t$ istanze indipendenti di Morris+ ritorniamo come stimatore il **valore mediano**.
Questo è noto come algoritmo **Morris++**.

Dato che l'error probability è $1/3$, allora il numero **medio** di istanze di Morris+ che ritorneranno una buona stima sarà $2t/3$.

```ad-attention
Ricordiamo che l'output $\tilde{n}$ di una istanza di Morris+ è una buona stima se $\vert \tilde{n} - n \vert \leq \varepsilon n$.
```


Per avere che il valore *mediano* sia una **cattiva stima** (ovvero lo scarto eccede $\varepsilon n$), deve essere vero che **non più** di $t/2$ istanze di Morris+ restituiscono una buona stima.
Questo equivale a dire che il numero di istanze che danno una buona stima si discosta dal numero atteso di istanze che danno una buona di stima di una quantità $t/6$.
Più formalmente, definiamo la variabile aleatoria *indicatrice*
$$Y_i = \begin{cases}
1 &\text{se la $i$-esima istanza di Morris+ da una bunoa stima}\\
0 &\text{altrimenti}
\end{cases}$$
Come discusso in precedenza il valore mediamo restituito da Morris++ è una cattiva stima di $n$ **se e solo se** al più $t/2$ delle istanze ritornano una buona stima, ovvero se $Y = \sum_{i=1}^{t}Y_i \leq t/2$.
Utilizzando il [[Concentration Inequalities#Multiplicative Chenroff's Bound]], con $\mu = 2t/3$ e $\varepsilon = 1/4$, avremo che
$$\begin{align*}
P\left( Y \leq (1-\varepsilon) \frac{2t}{3}\right)
&=  P\left( \sum_{i=1}^{t} Y_i \leq \frac{t}{2}\right)\\
&\leq e^{-\tfrac{2t}{3}\cdot\tfrac{1}{32}}\\
&= e^{- \tfrac{t}{48}} \leq \delta
\end{align*}$$

Morris++, per avere una probabilità di errore $\leq \delta$, richiede quindi $t \geq \left\lceil 48 \ln{\dfrac{1}{\delta}} \right\rceil \in \Theta(\log{1/\delta})$.

-----
# Analisi memoria
Per ottenere un errore di $\varepsilon n$ e una probabilità di errore $\delta$, il contatore [[#Morris+]] richiede $s = \Theta(\varepsilon^{-2}\delta^{-1})$ istanze di Morris base, mentre [[#Morris++]] richiede solo $st = \Theta(\varepsilon^{-2}\log{\delta^{-1}})$ istanze **indipendenti**.

Osserviamo ora che quando uno qualsiasi dei $st$ contatori di Morris $X$ raggiunge il valore $\log{(stn/\delta)}$ esso verrà in seguito incrementato con probabilità $\delta/(stn)$.
Perciò la probabilità che esso venga incrementato *in uno dei successivi* $n$ updates è $\delta / (st)$.

Applicando **union bound** su tutte le $st$ ripetizioni, abbiamo che la probabilità che almeno uno dei contatori ecceda la quantità $\log{(stn/\delta)}$ nei successivi $n$ updates è $\delta$.
Quindi con probabilità $1-\delta$ tutti i contatori hanno come valore nel contatore $X \leq \log{(stn/\delta)}$, ovvero ognuno di essi utilizza al più $O(\log\log{(stn/\delta)})$ bit di spazio.

> [!theorem] Morris++ Space
> Per ogni errore $\varepsilon > 0$ e probabilità di fallimento $\delta > 0$, dopo $n$ operazioni di updates l'algoritmo Morris++ utilizza
> $$O\left(\varepsilon^{-2}\log\frac{1}{\delta} \cdot\log\log{\frac{n\log{(1/\delta)}}{\varepsilon\delta}}\right)$$
> bits di memroia con probabilità $1-\delta$.
> Inoltre con probabilità $1-\delta$ ritorna uno stimatore $\tilde{n}$ di $n$ con errore relativo $\vert \tilde{n} -n \vert \leq \varepsilon n$.


Una analisi più semplice è tramite la media.
Sappiamo che ogni contatore $X$ in media ha come valore $\log{n}$ (vedi [[#^d58e3d|Claim 1]]).
Perciò abbiamo il seguente risultato

> [!theorem] Morris++ Space in expectation
> Per ogni errore $\varepsilon > 0$ e probabilità di fallimento $\delta > 0$, dopo $n$ operazioni di updates l'algoritmo Morris++ utilizza
> $$O\left(\varepsilon^{-2}\log\frac{1}{\delta} \cdot\log\log{n}\right)$$
> bits di memroia **in media**.
> Inoltre con probabilità $1-\delta$ ritorna uno stimatore $\tilde{n}$ di $n$ con errore relativo $\vert \tilde{n} -n \vert \leq \varepsilon n$.
