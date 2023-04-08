---
date: 2023-03-13
draft: true
---

Un algoritmo $\alpha$-approssimante per un problema di ottimizzazione è un algoritmo **polinomiale** il cui valore della soluzione è al più $\alpha$ peggiore rispetto alla soluzione ottima.

- $\alpha$: **approximation ratio** o **approximation factor**.

- **minimizzazione**: $\alpha \geq 1$, per ogni soluzione $x$ restituita dall'algoritmo approssimante avremo $\text{cost}(x) \leq \alpha \cdot \text{OPT}$
- **massimizzazione**: $\alpha \leq 1$, per ogni soluzione $x$ restituita dall'algoritmo approssimante avremo $\text{cost}(x) \geq \alpha \cdot \text{OPT}$

# Minimum Vertex Cover
- **Input**: $G(V,E)$
- **Feasible solution**: $U \subseteq V$ s.t. evrey edge $(u,v) \in E$ we have $u \in U$ or $v \in U$
- **Measure (min)**: $\vert U \vert$

==esempio==

> **Matching**: un sottoinsieme $M \subseteq E$ è un **matching** di $G$ se $\forall e, e' \in M$ abbiamo che $e \cap e' \equiv \emptyset$.

> **Matching massimale**: un matching $M$ è detto **massimale** se aggiugendo un qualsiasi altro arco $e \in E \setminus M$ allora $M \cup \lbrace e \rbrace$ **non** è più un matching.

> **ALG 2-apx for Min Vertex Cover**:
> 1. find a **maximal matching** $M$ (*greedy* - *poly time*).
> 2. return all the endpoints of edges in $M$.

> **Lemma** l'algoritmo restituisce un VC per $G$.
> **Proof**:
> sia $M \subseteq E$ il matching massimale calcolato nell'algoritmo.
> Certamente i nodi della soluzione coprono tutto gli archi di $M$.
> Inoltre visto che $M$ è **massimale**, allora qualsiasi altro arco $e \in E \setminus M$ ha **almeno** una estremità nella soluzione $\square$.

> **THM**
> L'algoritmo è 2-approssimante.
> **Proof**:
> Osserviamo che gli archi di $M$ **non** condividono nodi. Dato che anche gli archi di $M$ devono essere coperti dal VC, allora ci vorrà **almeno** un nodo per ognuno degli archi di $M$. Quindi $\text{OPT} \geq \vert M \vert$.
> 
> Perciò $$\vert U \vert = 2 \vert M \vert \leq 2 \cdot \text{OPT}$$

```ad-note
title: Lower Bound Schema
La dimensione di ogni matching massimale è un **lowerbound** alla soluzione di qualsiasi VC **ottimo**.
```

^4eea88


### Questions
1. Posso sempre **migliorare** il fattore di approssimazione di un **algoritmo fissato** facendo un'analisi migliore?
2. Posso dare un algoritmo migliore con un fattore di approssimazione più stretto?
3. Esiste uno **schema di lowerbound** migliore che mi consente di creare un algoritmo migliore?

In questo caso avremo le seguenti risposte:
1. In questo caso la risposta è **NO**, l'analisi è tight. Famiglia di controesempi:
	- complete bipartite graph $K_{n,n}$
	- L'algoritmo darà una soluzione $2n$
	- Invece $\text{OPT} = n$
2. Anche in questo caso la riposta è **NO**. Famiglia di controesempi:
	- Prendo una clique $K_n$ con $n$ dispari
	- La dimensione di $M$ sarà $(n-1)/2$
	- La dimension dell'ottimo è $\text{OPT} = n - 1$
	- $\implies$ esistono grafi il cui ottimo è esattamente $2$ volte lo schema di lowerbound, perciò meglio non si può fare.
	- Anche se la soluzione calcolata dall'algoritmo è **ottima** (mi trova esattamente un VC di dimensione minima), però in analisi la soluzione è sempre 2 volte il lowerbound.
3. In questo caso la domanda è **aperta**. Per il momento non si sa fare meglio di 2 come fattore di approssimazione

> **THM**
> Se assumiamo la **unique games conjecture** allora non si può approssimare minimum vertex cover con un fattore d'approssimazione $\alpha < 2$, a meno che $P = NP$.

> **Unique Games Conjecture**: Si pensa che tale problema sia NP-completo.

# Minimum Set Cover
- **Input**:
	- un universo di elementi $U$ di $n$ elementi.
	- una collezione di sottoinsiemi di $U$, $\mathcal{S} = \lbrace S_1, ..., S_k \rbrace$
	- un costo $c:\mathcal{S} \to \mathbb{R}^+$
- **Feasible solution**:
	- un sottoinsieme $\mathcal{C} \subseteq \mathcal{S}$ tale coprono tutti gli oggetti di $U$.
- **Measure (min)**:
	- **minimizzare** la somma dei costi degli elementi della soluzione.ù


Questa è una **generalizzazione** del VC dove:
- gli oggetti sono gli archi.
- i sottonsiemi $S$ sono gli archi incidenti a un nodo.

> **Greedy Alg**
> Ad ogni istante selezionare il sottoinsieme $S$ che **minimizza** la **cost-effectiveness**.
> 
> Sia $C \subset U$ gli oggetti già coperti.
> - **cost-effectiveness** di $S$ = $c(S) / (S \setminus C)$ (il rapporto tra il costo di $S$ e quanti nuovi oggetti esso ricopre).

^489290

> **Obs**: osserviamo che sommando i $\text{price}(e)$ otteniamo esattamente il costo della soluzione.

### Analisi
Ordiniamo gli oggetti $e_1, ..., e_n$ in ordine in cui **vengono coperti**.

> **Lemma** Per ogni $k \in \left[ n \right]$, allora $\text{price}(e_k) \leq \text{OPT}/(n-k+1)$.
> **Proof**:
> Fissiamo una iterazione in cui riusciamo a coprire l'oggetto $e_k$.
> Consideriamo gli insiemi che fanno parte della soluzione OTTIMA, che riescono a coprire i restanti elementi $\mathcal{C}' = U \setminus \mathcal{C}$ .
> Questi insiemi rimanenti, hatto insieme valore al più OPT.
> Il minimo di questi (per questioni di media) ha valore **al più** $\leq OPT/\vert C ' \vert$.
> Dato che sono rimasti al più $\leq n-k+1$ elementi nel momento quando ricorpo $e_k$.
> Dato che l'algoritmo sceglierà il sottoinsieme di valore minimo, allora avremo che $$\text{price}(e) \leq OPT/(n-k+1)$$ 


> **THM**
> L'algoritmo greedy ha approssimazione $H_n$.
> **Proof**:
> Il costo della soluzione sarà
> $$\sum_{k = 1}^{n} \text{price}(e_k) \leq \sum_{k = 1}^{n} OPT/(n-k+1) \leq H_n \cdot OPT$$

1. Domanda 1: l'analisi è tight, esiste una famiglia di istanze in cui fa esattamente $H_n$
2. Domanda 2: è complicato, perché abbiamo che l'OPT è almeno qualcosa che dipende dalla somma dei prezzi della soluzione.
3. Domanda 3: NO, vedi thm

> **THM** esiste una costante $c > 0$ t.c. se si vuole fare un algoritmo di approssimazione con fattore di approssimazione migliore di $c\log{n}$, allora $P = NP$

```ad-tldr
Asintoticamente non si può fare meglio di $\log{n}$.
```

> **THM** Se esiste un algoritmo $(c \log{n})$-apx per il problema unweighted (con tutti costi di $S$ pari ad 1) per un certo $c < 1$, allora un algoritmo $O(n^{O(\log\log{n})})$ per SAT.


# Approximation Game

- $n^k$
- $\log^k{n}$
- $O(1)$
- **PTAS** $f(1/\varepsilon) n^{O(1/\varepsilon)}$
- **EPTAS** $f(1/\varepsilon) n^{O(1)}$
- **FPTAS** $\text{POLY}(1/\varepsilon) n^{O(1)}$
