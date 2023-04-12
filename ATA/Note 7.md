---
date: 2024-01-05
draft: true
content:
    - multicolored-clique
    - k-dominating-set
    - ETH
    - W-hardness
---

- Si può dimostrare che un problema (e.g. **k-Clique**) è non FPT?
- Si può dimostrare che un problema (e.g. **k-Vertex Cover**) non ha un algoritmo parametrizzato con tempo $2^{o(k)}n^{O(1)}$?

**Obs** Innanzitutto bisgona dimostrare che $P \neq NP$, altrimenti sarebbero tutti polinomiale in $n$.
Questo non è sufficiente: per esempio se $P = NP$, $k$-clique potrebbe essere polinomiale, e ammettere anche un algoritmo FPT.

```ad-important
title: Conditional Lower Bound
Un lower bound è **condizionale** se vero assumendo che una congettura è vera.
```


## Parametrized Complexity
Gli ingredienti per una teoria della complessità sono:
- una appropriata definizione di **riduzione**.
- una appropriata **ipotesi di hardness**.


```ad-important
La classica **riduzione polinomiale** non è sufficiente per il nostro scopo di definire una parametrized complexity.
```

==vedi esempio Independent Set - Vertex Cover==

> **Parametrized Reduction**
> Supponiamo di voler ridurre $P$ nel problema $Q$.
> Sia $\phi$ una funzione che trasforma un'istanza $(x,k) \in P$ in un'istanza $(x',k') \in Q$, tale che
> - $(x,k)$ è un'istanza SI di $P$ **SSE** $(x',k')$ è un'istanza SI di $Q$.
> - $(x',k') = \phi((x,k))$ può essere calcolato in tempo $f(k) \cdot n^{O(1)}$.
> - $k' \leq g(k)$ per qualche funzione $g$.

**NOTE** se $Q$ è FPT allora anche $P$ è FPT. Equivalentemente se $P$ non è FPT allora anche $Q$ è non FPT.

**Esempio** k-IS in k-Clique
$$(G,k) \to (\overline{G}, k)$$

------
# Multicolored Clique
- **Input**:
	- un grafo $G(V,E)$ con nodi **colorati** con $k$ colori.
	- un intero $k \geq 0$.
- **Question**:
	- esiste un **mulicolored clique** di dimensione $k$? (ovvero una clique con tutti colori differenti)
- **Parametro**: $k$


Questo problema

> **Theorem**
> Multicolored Clique è del tutto equivalente al problema della k-Clique.
> 
> **Proof**
> ==vedi immagine (semplice)==


SI può dimostrare che k-Clique può essere ridotto a Multicolored Independent Set.

# k-Dominating Set
- **Input**
	- un grafo $G(V,E)$
	- un interno $k \geq 1$
- **Question**:
	- esiste un $U \subseteq V$ tale che $\forall v \in V$ abbiamo che $v$ ha un vicino di $U$, ed $\vert U \vert \leq k$?
- **Parameter**: $k$

> **THM**
> Esiste una riduzione parametrizzata da **Multicolored Independent Set** a **k-Dominating Set**.
> 
> **Proof**:
> ==vedi immagine==


----
# Hypotesis

> **Engineers' Hypothesis (EH)**
> $k$-Clique non può essere risolto in tempo $f(k) \cdot n^{O(1)}$.
> Non riesco a trovare un algoritmo FPT per $k$-Clique, riesco a ridurre tantissimi problemi a $k$-Clique, quindi il problema è hard (come per SAT per la teoria della complessità classica).

> **Theprosts' Hypothesis (TH)**
> $k$-Step Haltin Problem non può essere risolto in tempo $f(k) \cdot n^{O(1)}$.
> Esiste una sequenza di transizioni di una Macchina di Turing per il $k$-Step Haltin Problem che termina dopo $k$ transizioni?

> **Exponential Time Hypothesis (ETH)**
> Un'istanza di 3-SAT con $n$ variabili non può essere risolto in tempo $2^{o(n)}$.

$$ETH \implies (EH \iff TH)$$


### Osservazioni
1. k-Clique e k-Step Halting Problem possono essere reciprocamente ridotti.
2. Dato che posso ridurre k-Clique a k-Dominating Set, allora anche k-Step Halting Problem può essere ridotto a k-Dominating Set.
3. Posso ridurre k-Dominating Set a k-Clique?
	1. Probabilmente no! Differentemente dall'NP-completezza, abbiamo un **gerarchia di complessità**
		1. **k-Independent Set** è $W\left[ 1 \right]$ completo
		2. **k-Dominating Set** è $W\left[ 2 \right]$ completo (la complessità è a crescere)

# W\[ x \]

> **Circuit Sat**
> Abbiamo un **circuito booleano** $C$ e vogliamo sapere se esiste un'assegnazione dei suoi input che genera un output TRUE.

> Il **peso** di un'assgnazione è il numero di input posti ad 1.

> **Weighted Circuit Sat**
> Decidere se esiste un'assegnazione degli input di $C$, con peso **al più** $k$.

```ad-info
Sia k-Independent Set che k-Dominating Set possono essere ridotti a k-Weighted Circuit Sat.
==vedi immagini==
```

**Idea** DS è più hard di IS perché abbiamo bisogno di circuiti più complicato.

> La **depth** di un circuito è la lunghezza massima di un cammino da un input all'output.

>Una **large gate** è una porta logica con più di due input.

>La **weft** di un circuito è il numero massimo di large gate in un qualsiasi cammino da input ad output.


Sia $C(t,d)$ l'insieme dei circuiti con **weft** al più $t$ e **depth** al più $d$.
> Un problema $P$ sta in nella classe $W(t)$ se esiste una costante $d$ e un una riduzione parametrica da $P$ a Weighted Circuit Sat con circuiti $C(t,d)$.

Si suppone che $$\dots \subsetneq W(3) \subsetneq W(2) \subsetneq W(1)$$

**Obs** se e esistesse una riduzione da DS a IS allora avremmo che $W(1) \equiv W(2)$.

-----
# Conseguenze di ETH

-----
> **THM**
> k-Clique non può essere risolto in tempo $f(k) \cdot n^{o(k)}$. (Quindi serve sempre un fattore lineare in $k$ al grado del polinomio in $n$).
> 
> **Proof**
> Supponiamo di poter risolvere $k$-clique in tempo $$f(k) \cdot n^{\dfrac{k}{s(k)}}$$ dove $s(k)$ è una funzione non decrescente che diverge.
> 
> Dimostriamo che possiamo trovare un 3-coloring di $G$ in tempo $2^{o(n)}$.
> 
> Assunzione: $$f(k) \geq \max(k, k^{k/s(1)})$$
> 
> Partizioniamo i nodi di $G$ in $k$ gruppi, di dimensioni al più $\leq \lceil n/k \rceil$.
> Costruiamo quindi un nuovo grafo $H$ t.c.
> - ogni vertice di $H$ corrisponde a ciascun 3-coloring, di ciascuno delle $k$ partizioni di $G$.
> - Inserisco un arco in $H$ se i corrispondenti 3-coloring di $G$ sono **compatibili**.
>
> Notare che se esiste una $k$ clique in $H$, allora esiste un 3-coloring valido in $G$.
> Abbiamo inoltre che $$\vert V(H) \vert \leq k \cdot 3^{\lceil n/k \rceil}$$
> 
> Per ogni $n$, sia $k$ il massimo valore tale che $f(k) \leq n$.
> Quindi poniamo $k = g(n)$, come una funzione non decrescente e che diverge (con $g(n) \leq n$, o analogamente $g = f'$).
> 
> ==da finire==


-----
# Strong ETH

> **Strong ETH (SETH)**
> Non esiste un algoritmo che risolve CNF-SAT in tempo $(2-\varepsilon)^n$.