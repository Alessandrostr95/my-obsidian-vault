---
date: 2023-09-15
draft: true
content:
  - algorithm for big data
---
Sia un grafo $G = (V,E)$ (pesato o non, diretto o non).
Un **ADS** (All-Distance Sketches) $ADS(i)$ per un nodo $i \in V$ è definito come un insieme di nodi (o i loro rispettivi ID) e le rispettive distanze da $i$.
$$ADS(i) \subseteq V \times \mathbb{R}$$

Un $ADS(i)$ è formato da un [[Random Sample|campione]] di nodi $j$ raggiungibili da $i$, e dalle rispettive distanze $d_{ij}$.
Più precisamente $ADS(i)$ è formato dall'unione di tutti i [[MinHash Sketches]] [[MinHash Sketches#Coordinated MinHash Sketches|coordinati]] del [[MinHash Sketches#MinHash Sketches su grafi|vicinato]] $N_d(i)$ (per ogni valore di $d$).

Un ADS è definito rispetto a [[MinHash Sketches#^c7e4f7|permutazioni random]] dei nodi di $G$, è può avere le stesse tre varianti del MinHas Sketch: [[MinHash Sketches#$k$-min sketch|k-min]] ADS, [[MinHash Sketches#bottom-$k$ sketch|bottom-k]] ADS e [[MinHash Sketches#$k$-partition sketch|k-partition]] ADS.

