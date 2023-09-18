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

## [[MinHash Sketches#$k$-partition sketch|k-partition ADS]]
Un $k$-partition ADS è definito rispetto a una **permutazione random** $r$ e rispetto ad una **partizione random**
$$\text{BUCKET}: V \to \left[ k \right]$$
che mappa i nodi nei sottoinsiemi
$$V_h \equiv \lbrace i \in V \vert \text{BUCKET}(i) = h\rbrace$$

Osserviamo che un nodo $j \in ADS(i)$ se e solo se il rank $r(j)$ è tra i $k$ rank più piccoli tra i nodi del suo bucket, i quali sono più vicini ad $i$.
$$j \in ADS(i) \iff r(j) < \min \lbrace r(\ell) \vert \text{BUCKET}(j) = \text{BUCKET}(\ell) \land \ell \in \Phi_{<j}(i)\rbrace$$

## [[MinHash Sketches#$k$-min sketch|k-min ADS]]
Osservare che un $k$-min ADS è semplicemente l'unione di $k$ bottom-1 **indipendenti** tra di loro, rispetto a $k$ permutazioni differenti e **indipendenti**.

------
# Proprietà
> **Lemma**: la dimensione **media** di un bottom-$k$ $ADS(v)$ è $$k + k(H_{n_v} - H_k) \approx k(1+\ln{n_v} - \ln{k})$$ dove $n_v$ è il numero di nodi raggiungibili da $v$.
> La dimensione **media** di un $k$-partition $ADS(v)$ è $$kH_{n_v/k} \approx k(\ln{n} -\ln{k})$$


```ad-info
title: Serie armonica
$$H_n = \sum_{j=1}^{n} \frac{1}{j} \approx 1 + \log{n}$$
```

> **Proof**: consideriamo i nodi in ordine di vicinanza da $v$, ovvero $i$ è l'i-esimo nodo più vicino a $v$.
> Per semplicità assumiamo le distanze tutte differenti, e consideriamo $i > k$, altrimenti $P(i \in ASD(v)) = 1$.
> Ricordiamo che in un bottom-$k$ ASD abbiamo che $$i \in ADS(v) \iff r(i) < k^{th}_r(\Phi_{<i}(v))$$
> Consideriamo ora il vettore $R$ che contiene tutti i rank degli elementi.
> Per semplicità possiamo ignorare tutti i rank dei nodi $> i$.
> Avremo quindi che $$P(r(i) < k_r^{th}(\Phi_{< i}(v))) = P\left( \bigcup_{j=1}^{k} R\left[ j \right] = r(i) \right) =  \sum_{j=1}^{k} P\left( R\left[ j \right] = r(i) \right) = \sum_{j=1}^{k}\frac{1}{i} = \frac{k}{i}$$
> Avremo quindi che
> $$\begin{align}
\mathbb{E}\left[ \vert ADS(v) \vert \right]
&= \mathbb{E}\left[ \Big\vert \bigcup_{i \in V} \lbrace i \in ADS(v) \rbrace  \Big\vert \right]\\
&= \sum_{i=1}^{n} P(i \in ADS(v))\\
&= \sum_{i=1}^{k} 1 + \sum_{i=k+1}^{n} P(r(i) < k_r^{th}(\Phi_{< i}(v)))\\
&= k + k(H_n - H_k)
\end{align}$$

