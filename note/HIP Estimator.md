---
date: 2023-09-19
draft: true
tags:
  - Math
  - Algorithm
  - ComputerScience
  - Probability
  - NetworkAnalysis
  - BigData
  - Statistics
  - DataScience
  - DataMining
content:
  - algorithm for big data
---
Sia $ADS(v)$ l'[[All-Distances Sketches|ADS]] di un nodo $v \in V$.
Consideriamo i nodi in **ordine di distanza** da $v$.
Per ogni nodo $u$ vogliamo calcolare la probabilità che $u$ sia raggiungibile da $v$.
$$\mathbb{1}\lbrace v \leadsto u \rbrace$$

Fissiamo i [[MinHash Sketches#^c7e4f7|rank]] $r(1), ..., r(u-1)$ di tutti i nodi più vicini a $v$ rispetto a $v$.
La **HIP probability** $\tau_{uv}$ equivale alla probabilità che $u \in ADS(v)$ condizionata ai rank $r(1), ..., r(u-1)$.
$$\tau_{uv} = P(u \in ADS(v) \vert r(1), ..., r(u-1))$$

L'**HIP estimator** per $\mathbb{1}\lbrace v \leadsto u \rbrace$ è calcolato scorrendo tutti i nodi $u \in ADS(v)$ in ordine di distanza da $v$.
Per ogni nodo poniamo
$$a_{uv} = \begin{cases}
1/\tau_{uv} &u \in ADS(v)\\
0 &u \not\in ADS(v)
\end{cases}$$
Il valore $a_{uv}$ è l'**HIP estimator** per $\mathbb{1}\lbrace v \leadsto u \rbrace$.

```ad-info
title: Osservazione
$$\begin{align}
\mathbb{E}\left[ a_{uv} \right]
&= \frac{1}{\tau_{uv}} \cdot P(a_{uv} = 1/\tau_{uv}) + 0 \cdot P(a_{uv} = 0)\\
&= \frac{1}{\tau_{uv}} \cdot P(u \in ADS(v) \vert r(1), ..., r(u-1))\\
&= \frac{1}{\tau_{uv}} \cdot \tau_{uv}\\
&= 1
\end{align}$$
```

> Per il [[All-Distances Sketches#MinHash Sketches bottom-$k$ sketch bottom-k ADS|bottom-k ADS]] avremo che $$\tau_{uv} = k^{th}_r\lbrace \Phi_{<u}(v) \cap ADS(v) \rbrace$$

In fine l'HIP estimator per 

# HIP Cardinality Estimator
Possiamo usare l'HIP estimator per calcolare la cardinalità del vicinato di un nodo $v$ ad un certo raggio $d$.

Sia $N_d(v)$ l'insieme di tutti i nodi raggiungibili da $v$ a distanza al più $d$.
Lo stimatore per $n_d(v) = \vert N_d(v) \vert$ sarà quindi definito come
$$\tilde{n}_d(v) = \sum_{u \in ADS(v) \;:\; d_{uv} \leq d} a_{uv}$$

## Coefficiente di Variazione
L'[[#HIP Cardinality Estimator|HIP k-bottom estimator]] ha [[All-Distances Sketches#Coefficient of Variation|coefficiente di variazione]] $$CV \leq \frac{1}{\sqrt{2(k-1)}}$$ ovvero meglio di un fattore $\sqrt{2}$ rispetto al [[MinHash Cardinality Estimator]]