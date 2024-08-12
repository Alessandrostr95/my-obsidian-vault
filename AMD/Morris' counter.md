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

Il [[#^d58e3d|Claim 1]] inoltre implica che $\tilde{n}$ è un 
