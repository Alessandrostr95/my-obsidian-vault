Consideriamo il **distinct element counting**, noto anche come $F_0$ problem.
È dato in input uno **stream** $x_1, \dots, x_m \in [n]$ di $m$ interi in $[n]$.
L'obiettivo è quello di tenere traccia di quanti elementi **distinti** sono presenti nello stream, utilizzando meno memoria possibile.

Due soluzioni banali sono le seguenti:
1. Tenere un **bit-vector** $V$ di $n$ elementi, e ad ogni elemento in input dello stream impostare $V[x_i] = 1$. Per sapere quanti elementi distinti ci sono nello stream basta ritornare la **norma zero** $\Vert V \Vert_0$. Questa soluzione richiede $n$ bit di spazio, ha un update time costante $O(1)$ e un query time lineare $\Theta(n)$.
2. Tenere esplicitamente in memoria l'intero stream, utilizzando $\Theta(m\log{n})$ bit di spazio.

Il **contatore di Flajolet-Martin** è un algoritmo che conta in maniera **approssimata** gli elementi distinti.
Questo algoritmo è **idealizzato**, in quanto assume di poter salvare in memoria una funzione **completamente random** e **reale** in spazio $o(n)$. #todo==mettere discussione hash functions.==

Sia quindi $x_1, \dots x_m \in [n]$ un data stream di $m$ elementi in $[n] = \{1, \dots, n\}$.
Sia $t$ il numero di **elementi distinti** dello stream.
Sia $h: [n] \to [0,1]$ una funzione che mappa **uniformemente a caso** il range $[n]$ nell'intervallo compatto $[0,1] \subseteq \mathbb{R}$.
L'algoritmo di Flajolet-Martin è il seugente

 
$$\begin{align*}\\ 
&\textbf{Algorithm } \text{Flajolet-Martin's idealized counter}\\ 
&\textbf{Input: } \text{A stream of integers }x_1, \dots, x_m \in [n]\text{An estimate $\tilde{t}$ of the number of distinct elements in the stream}\\ 
&\textbf{Output: } \text{An estimate $\tilde{t}$ of the number of distinct elements in the stream}\\ 
\\
&1.\;\;\text{Initialize a counter } X = 1\\
&2.\;\;\textbf{For each element $x_i$ of the stream do: }\\
&3.\;\;\;\;\;\;X \leftarrow \min(X, h(x_i))\\
&4.\;\;\textbf{For each query do: }\\
&5.\;\;\;\;\;\;\textbf{return } \frac{1}{X}-1\\
\end{align*}$$

