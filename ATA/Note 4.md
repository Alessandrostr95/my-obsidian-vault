---
date: 2024-03-27
draft: true
content:
  - parametrized algorithms
---

Noi vorremmo:
1. risolvere problemi difficili (possibilmente NP-hard)
2. trovare un algoritmo **polinomiale** che lo risolva
3. trovare la soluzione **esatta**

==caso 2-3==

In genere per risolvere un problema NP-hard (1) in maniera veloce (2) bisogna rinunciare alla **qualità** della soluzione (3).
Parliamo quindi problemi di approssimazione.


==caso 1-3==

**idea**: studiamo problemi NP-hard, e vogliamo una soluzione **ottima**. Nel caso peggiore la soluzione è **esponenziale**. L'idea è quella di **confinare** l'esponenzialità ad un **parametro** fissato.

**goal**: vogliamo un algoritmo polinomiale nella grandezza dell'istanza $n$ però **esponenziale** nel parametro $k$.

**parametro**: $k(x)$ un parametro non negativo associato all'istanza $x$.

**problema parametrizzato**: un problema in cui si specifica un parametro.

# K-vertex Cover
- **Input**:
	- un grafo $G$
	- un parametro $k > 1$
- **Goal**: decidere se esiste un insieme $S \subseteq V$ di dimensione al più $S \leq k$.

**Bruteforce solution**:
- provo tutti i $O(n^k)$ sottoinsiemi di $k$ nodi
- verifico per ognuno di questi se è un vertex cover in **tempo lineare**.

**Running time** $O(n^k \times m)$

## FPT problem
Un problema parametrizzato è detto **Fixed Parameter Tractable** (**FTP**) se ammette un algoritmo che lo risolve all'ottimo con complessità $$f(k) \cdot n^{O(1)}$$
Dove l'esponente è **indipendente** da $n$ e da $k$.

# Bounded-search tree algorithm

1. prendiamo un arco non coperto $e = (u,v)$ qualsiasi.
2. perciò avremo che $u \in S \lor v \in S$.
3. provo a fare una **guess** su quali dei due inserire in $S$.
	1. Caso $u \in S$:
		- aggiungo $u$ in $S$, rimuovo $u$ e tutti i suoi archi incidenti.
		- riduco ricorsivamente sull'istanza $G' = G \setminus \lbrace u \rbrace$ con parametro $k' = k-1$
	2. Caso $v \in S$:
		- aggiungo $v$ in $S$, rimuovo $v$ e tutti i suoi archi incidenti.
		- riduco ricorsivamente a $G' = G \setminus \lbrace v \rbrace$ con parametro $k' = k-1$
4. ritorno YES se trovo una delle due ricorsioni trova una soluzione
5. caso base $k = 0$.
	1. se ho almeno un arco non coperto, ritorno NO
	2. altrimenti ritorno YES

**Running time**: ho $O(2^k)$ ricorsioni, ognuna delle quali ha complessità lineare. Ogni istanza la risolvi in tempo lineare, perciò ho complessità $$O(2^k n)$$

==vedi toolbox per FPT==


# Kernelizaton
L'idea della **kernelization** è quella di prendere un'istanza grande, fare un **pre-processing** per ridurla ad un'istanza **equivalente** più piccola.

**Kernelization** è un algoritmo polinomiale che converte un'istanza $(x,k)$ in un'istanza $(x',k')$ più piccola ed equivalente.

**Equivalenza**: $(x,k)$ è un'istanza SI $\iff$ $(x',k')$ è un'istanza SI.
**Piccola**: la dimensione di $(x',k')$ è $\leq f(k)$.


> **THM** Un problema parametrizzato è $FTP$ se e solo se esso ammette una **kernelizzazione**.
> 
> **Proof**:
> $(\Leftarrow)$
> Prendiamo l'istanza e la carnelizziamo in tempo polinomaile in una di dimensione $n' \leq f(k)$.
> Eseguiamo un algoritmo polinomiale per risolvere la nuova istanza in tempo $g(n)$.
> La complessità per risolvere il problema sarà quindi $$\text{POLY(n)} + g(f(k))$$
> $(\Rightarrow)$
> Supponiamo di avere un alg $A$ che risolve il problema in tempo $f(k) \cdot n^c$.
> Consideriamo due casi.
> 
> 1. se $n \leq f(k)$ allora l'istanza è già kernelizzata.
> 2. se $f(k) \leq n$
> 	- risolvo il problema con l'algoritmo $A$ in tempo $f(k)n^c \leq n^{c+1}$
> 	- ritorno un'istanza di dimensione $O(1)$ in maniera appropriata $\square$


## Polynomial kernel for k-Vertex Cover

