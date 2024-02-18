---
draft: true
date: 2024-02-18
---

> [!definition] Periodo
> Sia $S = s_1 \dots s_n$ una stringa di lunghezza $n$.
> Un prefisso $S_l = s_1 \dots s_l$ di $S$ è detto **periodo** di $S$ se e solo se $s_i = s_{i+l}$ per ogni $1 \leq i \leq n-l$.

^63645a


> [!lemma]
> Sia un **test** $T = t_1t_2 \dots t_n$, un **pattern** $P = p_1 p_2 \dots p_m$ (con $m < n$), e sia il $P_l = p_1p_2\dots p_l$ il più piccolo [[#^63645a|preiodo]] di $P$.
> Allora se il pattern $P$ combacia il testo $T$ a un certo indice $i$ (ovvero $t_i = p_1, ..., t_{i+m} = p_m$) allora non ci potrà essere alcun match con $P$ tra le posizioni $i$ e $i + l$
> 
> > **Proof**: questo perché $P_l$ è il minimo.
> > Infatti, per assurdo assumiamo che c'è un altro match di $P$ all'indice $i < i+j < i + l$.
> > Se fosse vero allora $p_1 = p_j, p_2 = p_{j+1}, ...$, ovvero avremo trovato un periodo di lunghezza $j < l$ (contraddicendo la minimalità di $P_l$). $\square$

------
Abbiamo una sequenza $x =x_1 ... x_m$e un pattern $y = y_1 ... y_n$ (con $n \leq m$). 
Vogliamo sapere quante volte $y$ occorre in $x$.
Assumiamo per semplicità che $n = 2^e$ per qualche valore di $e$.
Indichiamo con $y^{(i)} = y_1 ... y_{2^i}$ un **prefisso** lungo $2^i$ di $y$.

Manteniamo gli hash di tutti i prefissi $y^{(0)}, y^{(1)}, ..., y^{(e)}$.
In totale abbiamo $1+e = 1 + \log{n}$ hash.

