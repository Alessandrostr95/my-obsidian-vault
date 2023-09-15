---
date: 2023-09-15
draft: true
content:
  - algorithm for big data
---
# Definizioni Preliminari
## Funzione $k_r^{th}(N)$
Sia una funzione $r$ definita su un insieme $N$.
Indichiamo con $k_r^{th}(N)$ il $k$-esimo elemento in $r(N)$.
Osservare che se $\vert N \vert < k$ allora $k_r^{th}(N) = \sup r(N)$.

# MinHash Sketches su grafi
Sia un grafo $G = (V,E)$ (pesato o non, diretto o non).
Definiamo
- $$N_d(i) \equiv \lbrace j \in V: d_{ij} \leq d\rbrace$$
- $$n_d(i) := \vert N_d(i) \vert$$
- $$\Phi_{<j}(i) \equiv \lbrace \ell \in V : d_{i\ell} < d_{ij} \rbrace$$
- $$\pi_{ij} := 1 + \vert \Phi_{<j}(i) \vert$$ ovvero la posizione di $j$ nel vicinato di $i$.

-----
# ADS
Sia un grafo $G = (V,E)$ (pesato o non, diretto o non).
Un **ADS** (All-Distance Sketches) $ADS(i)$ per un nodo $i \in V$ è definito come un insieme di nodi (o i loro rispettivi ID) e le rispettive distanze da $i$.
$$ADS(i) \subseteq V \times \mathbb{R}$$

Un $ADS(i)$ è formato da un [[Random Sample|campione]] di nodi $j$ raggiungibili da $i$, e dalle rispettive distanze $d_{ij}$.
Più precisamente $ADS(i)$ è formato dall'unione di tutti i [[MinHash Sketches]] [[MinHash Sketches#Coordinated MinHash Sketches|coordinati]] del [[MinHash Sketches#MinHash Sketches su grafi|vicinato]] $N_d(i)$ (per ogni valore di $d$).

Un ADS è definito rispetto a [[MinHash Sketches#^c7e4f7|permutazioni random]] dei nodi di $G$, è può avere le stesse tre varianti del MinHas Sketch: [[MinHash Sketches#$k$-min sketch|k-min]] ADS, [[MinHash Sketches#bottom-$k$ sketch|bottom-k]] ADS e [[MinHash Sketches#$k$-partition sketch|k-partition]] ADS.

> [!warning]
> Per semplicità assumiamo che $d_{ij}$ è **unica** per $j$ differenti.

## [[MinHash Sketches#bottom-$k$ sketch|bottom-k ADS]]
Un bottom-$k$ ADS è definito rispetto a una sola permutazione random $r(V)$ di $V$.
Osserviamo che $j \in ADS(i)$ se e solo se il rank $r(j)$ è tra i primi $k$ più piccoli dei nodi che sono più vicini ad $i$.
$$j \in ADS(i) \iff r(j) < k^{th}_r(\Phi_{<j}(i))$$

