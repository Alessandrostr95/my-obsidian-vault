---
date: 2023-03-15
draft: true
content:
  - steiner tree
  - tsp
---

# Minimum Steiner Tree
- **Input**:
	- $G(V,E, c \geq 0)$
	- $R \subseteq V$ **required nodes**
	- $V \setminus R$ **steiner nodes**
- **Output**:
	- Uno spanning tree $T$ su che contiene tutti i nodi required $R$.
- **Costo**: $$\sum_{e \in T} c(e)$$

Quando $R \equiv V$ allora la soluzione è un **MST**.
Quando $R \neq V$ il problema è NP-hard.


- **Metric Steiner** instances:
	- $G$ è completo.
	- vale la disuguaglianza triangolare su ogni coppia di vertici incidenti. Dati $u,v,w$ allora $c(u,v) \leq c(u,w) + c(w,v)$

> **THM**
> è possibili ridurre minimum steiner tree in minumum metric stiner tree a meno di un fattore di approssimazione.
> **Proof**:
> Sia $I$ l'istanza del minimum Steiner tree.
> In tempo polinomiale possiamo ridurre $I$ a $I'$ (istanza metrica):
> - $G'=(V,E')$ è un grafo **completo** dove $c'(u,v) = \text{dist}_G(u,v)$
> - $R' \equiv R$
> Dato che per ogni arco $(u,v) \in E$ vale che $c'(u,v) \leq c(u,v)$, allora $\text{OPT}(I') \leq\text{OPT}(I)$
> 
> Possiamo sempre convertire $T'$ in $T$ per $I$.
> ==da finire==

D'ora in avanti parleremo di istanze sempre metriche.
> **Alg1**
> 1. Prendiamo il sottografo in $G$ **indotto** da $R$.
> 2. Facciamo il **MST** sul sottografo indotto $G\left[ R \right]$.

> **THM**
> Quella dell'algoritmo ALG1 è una soluzione 2-approssimante.
> **Proof**:
> Sia $T$ la soluzione ottima, ed $M$ il minimum spannin tree restituito da ALG1.
> **Raddoppiamo** gli archi in due bidirezionali in $T$ (chiamiamolo $T'$)
> Siò nuovo grafo possiamo trovare un **cammino euleriano**.
> Il costo di questo cammino euleriano sarà esattamente $cost(T') = 2OPT$.
> Facciamo quindi degli shortcut, che passano dai soli nodi required, ed otteniamo un ciclo $C$ con $cost(C) \leq cost(T') = 2OPT$.
> Togliendo un arco da $C$ otteniamo un albero ricoprente su $R$, il quale non può essere migliore di $M$.
> $$cost(M) \leq cost(C) \leq cost(T') = 2OPT \; \; \square$$ 


==vedi esempio tight==

### State of the art
- 2 [Takahashi & Matsuyama]
- 11/6 [Zelikovsky]
- 1.746 [Berman & Ramaiyer]
- 1 + $\ln{2}$ + $\epsilon$ [Zelivsky]
- 5/3 + $\epsilon$ []
- 1.644
- 1.598
- $1 + (\ln{3})/2 + \varepsilon$
- $\ln{4} + \varepsilon$

# Traveling Salesman Problem
- **Input**:
	- un grafo completo $G(V,E)$
- **Output**:
	- un ciclo hamiltoniano $C$
- **misura (minimizzare)**:
	- $\sum_{e \in E(C)}c(e)$


- **Metric TSP**:
	- quelle istanze di TSP che rispettano la **disuguaglianza triangolare**.

Le istanze metriche sono computazionalmente **approcciabili** e **approssimabili**.

> **THM**
> Per qualsiasi funzione $\alpha(n)$, una $\alpha(n)$-apx di TSP è NP-hard.
> **Proof**:
> Riduciamo da Hamiltonian Cycle a TSP.
> I pesi degli archi costano tutti 1.
> Per gli archi mancanti inserisco un arco di peso $= n \cdot \alpha(n)$.
> - Se $G$ ha un HC, allora l'algoritmo di TSP troverà una soluzione di valore $n$.
> - Se $G$ non ha un HC, allora l'algoritmo per TSP troverà una soluzione di valore almeno $> n \cdot \alpha (n)$.
> $\implies$ $G$ ha un HC se e solo se $G'$ ha un TSP di valore esattamente $n$ $\square$.


> **ALG2** (istanza metrica)
> 1. Trova un MST di $G$.
> 2. Raddoppia tutto gli archi di $T$ ottenendo un grafo euleriano.
> 3. Trova un **tour** euleriano $\tau$ su questo grafo.
> 4. Ritornaimo un ciclo emiltoniano $C$ partendo da $\tau$, dove uso l'arco diretto (shortcut) dei nodi già visitati.

> **THM**
> L'algoritmo è 2-approssimante.
> **Proof**:
> $cost(T) \leq OPT$
> $$cost(C) \leq cost(\tau) \leq 2 \cdot COST(T) \leq 2 \cdot OPT$$


==esempi tight==

```ad-note
- un grafo ha un cammino euleriano se tutti i vertici hanno grado **pari**.
- in ogni grafo non diretto, il numero di nodi di grado **dispari** è sempre **pari**.
```


> **ALG 3** (3/2-apx)
> 1. Triva un MST $T$ di $G$.
> 2. Calcola un **minimum cost perfect matching** $G$ per tutti i nodi $V'$ di grado **dispari** in $T$.
> 3. Aggiungi gli archi di $M$ a $T$.
> 4. Trova il tour euleriano $\tau$.
> 5. Trova il ciclo $C$ con gli shortcut che visita tutti i nodi di $\tau$.

> **Lemma**
> Sia $V' \subseteq V$, tale che $\vert V' \vert$ è **pari**.
> Sia $M$ un minimum cost perfect matching su $V'$.
> Allora $cost(M) \leq OPT/2$ del TSP.
> **Proof**:
> Partiamo da un $\tau^*$ soluzione ottima del TSP, e di cost OPT.
> Sia $\tau'$ un tour su $V'$ facendo shortcut su $\tau^*$.
> Allora $cost(\tau') \leq cost(t^*)$.
> 
> Osserviamo che $\tau'$ è un ciclo di numero pari di archi che compre $V'$.
> Se alterniamo gli archi di $\tau'$, troviamo due perfect matching $M_1, M_2$ di $V'$.
> Osserviamo che $$cost(M_1) + cost(M_2) = cost(\tau')$$
> Allora avremo che $$cost(M) \leq \min(cost(M_1), cost(M_2)) \leq \frac{1}{2}cost(\tau') \leq \frac{1}{2} OPT \; \; \square$$ 

> **THM**
> L'algoritmo ALG3 è $3/2$-apx.
> **Proof**
> $$cost(C) \leq cost(M) + cost(T) \leq \frac{1}{2}OPT + OPT = \frac{3}{2}OPT \;\; \square$$

==vedi esempio tight==


### State of the art
1. $3/2$
2. $3/2 - \varepsilon$ (**in media**) per $\varepsilon > 10^{-36}$.

