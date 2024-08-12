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

> **Claim 1.** $$E[ 2^{X_n} ] = n+1$$

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

> **Claim 2.** $$E[ 2^{2X_n} ] = \frac{3}{2}n^2 + \frac{3}{2}n+1$$

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

Questo risultato non è particolarmente informativo, dato che la probabilità che lo stimatore si discosta di un fattore $\varepsilon$ dal valore effettivo è minore di $1/2$ solo quando $\varepsilon > 1$.

--------
# Morris +
Per diminuire la probabilità di errore del Morris' Counter, dobbiamo usare il [MEAN TRICK].
Istanziamo quindi $s$ copie **indipendenti** dell'algoritmo di Morris, e facciamo la **media** dei loro output.
Siano $\tilde{n}_1, \dots, \tilde{n}_s$ gli stimatori restituiti dalle $s$ istanze indipendenti, e definiamo il nuovo stimatore
$$\tilde{n} = \frac{1}{s}\sum_{i=1}^{s}\tilde{n}_i$$
Dato che ogni $\tilde{n}_i$ è uno [[Stimatori Puntuali#^a1f3b4|stimatore unbiased]] di $n$ (vedi [[#^d58e3d|Claim 1]]), allora anche la loro media aritmetica $\tilde{n}$ è uno stimatore unbiased di $n$.
Il vantaggio di usare $\tilde{n}$ è la sua varianza più piccola.
