---
date: 2023-09-19
tags:
  - BigData
  - Math
  - Algorithm
  - Probability
  - NetworkAnalysis
  - ComputerScience
  - DataScience
  - Network
  - DataMining
draft: true
content:
  - algorithm for big data
---
Sia $N_d(v)$ l'insieme di tutti i nodi raggiungibili da $v$ a distanza al più $d$.
Osserviamo che dall'[[All-Distances Sketches|ADS]] di $v$ possiamo ricavare il [[MinHash Sketches|MinHash Sketch]] per tutti i nodi a distanza $d$.
Infatti esso equivale all'insieme
$$MHS_d(v) \equiv ASD(v) \cap \lbrace u | d_{u,v} \leq d \rbrace$$

Lo stimatore per $n_d(v) = \vert N_d(v) \vert$ sarà quindi definito come
$$\tilde{n}_d(v) = \frac{k-1}{\tau_k}$$ dove $$\tau_k = k^{th}_r(MHS_d(v))$$
