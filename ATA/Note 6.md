---
date: 2023-01-03
draft: true
content:
    - treewidth
---

# Treewidth
==Klainber-Tardos chapter 10.4==
La **treewidth** è un parametro che misura quanto è vicino un grafp ad essere un albero.

Esistono problemi NP-hard, che però se ci restringiamo alle sole istanze alberi si possono risolvere in tempo polinomiale (per esempio **weighted independete set**).

Si può generalizzare anche a grafi "*quasi alberi*", dove per quasi albero si intende con treewidth bassa.

### Soluzione weighted independent set
Per ogni $v \in T$:
- $T_v$ il sottoalbero radicato in $v$.
- $A\left[ v \right]$ è il peso del maximum weighted independent set di $T_v$.
- $B\left[ v \right]$ è il peso del maximum weighted independent set di $T_v$ con il vincolo però che $v$ **non appartiene** alla soluzione.

Se risolvo tutti i sottoproblemi, riesco a calcolare il valore della soluzione ottima $A\left[ r \right]$, dove $r$ è la radice.

Abbiamo duce casi:
- $v$ è una foglia: $A\left[ v \right] = w_v$ e $A\left[ v \right] = 0$
- $v$ è un nodo interno con figli $u_1, ..., u_d$. Allora $$\begin{align}
B \left[ v \right] &= \sum_{i = 1}^{d} A \left[ u \right]\\
A \left[ v \right] &= \max \lbrace B \left[ v \right], w_v + \sum_{i = 1}^{d} B \left[ u \right] \rbrace
\end{align}$$

```ad-note
L'ordine dei sottoproblemi è **bottom-up**.
```

----
## Generalizing Tree

Alcune definizioni alternative
- **Def 1**: pochi cicli.
- **Def 2**: rimuovere pochi nodi rende il grafo aciclico.
- **Def 3**: ho tante piccole parti, collegate tra di loro come in un albero.

> **Tree Decomposition**
> Una **tree decomposition** $(T, \lbrace V_t: t \in T \rbrace)$ di un grafo $G$ consiste in un albero $T$ dove ad ogni nodo $t \in T$ è associato una **pezzo/bag** $V_t \subseteq V$.
> Inoltre tali pezzi devono rispettare tali proprietà:
> 1. **Node Coverage**: ogni nodo di $G$ appartiene ad **almeno** un pezzo $V_t$.
> 2. **Edge Coverage**: per ogni arco $e \in E$ deve esistere almeno un pezzo $V_t$ che contiene entrambi gli estremi di $e$.
> 3. **Coherence**: Siano $t_1,t_2,t_3 \in T$ tali che $t_2$ appartiene al cammino tra $t_1$ e $t_3$. Allora se un nodo $v$ appartiene ad entrambi $V_{t_1}$ e $V_{t_3}$ allora $v$ appartiene anche a $V_{t_2}$.

^570fba

> **Width**
> La **width** di una [[#^570fba|tree decomposition]] $(T, \lbrace V_t: t \in T \rbrace)$ è il valore $$\max_{t \in T} \vert V_t \vert - 1$$

^e306e4

==vedi esempi==

> **Treewidth**
> La **treewidth** di un grafo $G$ è la [[#^e306e4|width]] della sua migliore [[#^570fba|tree decomposition]].

==vedi tree decomposition di un albero==

----

Sia $T'$ un sottografo di $T$ (la tree decomposition)
$G_{T'}$: è il sottografo di $G$ indotto dai nodi di $T'$, ovvero $$G_{T'}(V) = \bigcup_{t \in T'} V_t$$

> **Lemma**
> Supponiamo che $T \setminus \lbrace t \rbrace$ ha come componenti connesse $T_1, ..., T_d$.
> Allora i sottografi indotti $$G_{T_1} \setminus V_t, G_{T_2} \setminus V_t, ..., G_{T_d} \setminus V_t,$$ non hanno nodi in comune e non hanno acrhi che li collegano.
> 
> **Proof**
> **Non c'è un nodo in comune tra le componenti**
> Supponiamo per assurdo che esiste un nodo $v$ che sta in $T_i$ e $T_d$.
> Se questo è vero, allora
> - $v$ sta in un pezzo $V_a$ della componente $T_i$
> - $v$ sta in un pezzo $V_b$ della componente $T_d$
> 
> Per coerenza allora $v$ dovrebbe stare anche in $V_t$.
> 
> **Non c'è un arco tra le componenti**
> Supponiamo per assurdo che c'è un arco da un nodo $u \in G_{T_i}$ a un nodo $v \in G_{T_v}$ 
> Se per assurdo fosse vero, allora:
> - esiste un pezzo $V_a$ che contiene entrambi $u,v$ in $T_i$.
> - esiste un pezzo $V_b$ in $T_d$ che contiene $v$.
> 
> Però dato che $V_t$ sta nel cammino tra $V_a$ e $V_b$, allora $v \in V_t$.


> **Lemma**
> Sia la tree decomposition $T$, e rimuoviamo un suo arco tra due pezzi $V_x, V_y$.
> Allora i due sottografi $$G_{x} \setminus (V_x \cap V_y), G_{y} \setminus (V_x \cap V_y)$$  non hanno nodi in comune e non hanno acrhi che li collegano.
> 
> **Proof**
> ==Analoga: vedi immagini==


-----

> **Tree decomposition ridondante**
> Una [[#^570fba|tree decomposition]] è detta **ridondante** se esiste un arco $(x,y) \in T$ tale che $V_x \subseteq V_y$.


Per creare una tree decomposition **non-ridondante** basta assrobire $V_x$ in $V_y$.

> **Lemma**
> Qualsiasi tree decomposition **non-ridondante** di un grafo di $n$ nodi ha **al più** $n$ pezzi.
> 
> **Proof** ==per induzione su n==

-----
# Dynamic Programming on Graph with bounded treewidth

## Defining subproblems
Prendiamo l'albero $T$ della tree decomposition e radichiamolo in $r$.

Per ogni $t \in T$
- sia $T_t$ il sottoalbero di $T$ radicato in $t$.
- sia $G_t$ il sottografi di $G$ indotto dai pezzi del sottoalbero $T_t$, ovvero $$G_t(V) = \bigcup_{x \in T_t} V_x$$

**Sottoproblemi**:
Per ogni nodo $t$, e ogni sottoinsieme $U \subseteq V_t$:
- $f_t(U)$: il valore del maximum weighted independent set $S \in G_t$, vincolato a $$S \cap V_t \equiv U$$

**Obs 1**: $f_t(U) = -\infty$ se $U$ non è un independent set.
**Obs 2**: il numero dei sottoproblemi sarà
- $2^{w+1}$ per ogni nodo $t$ della tree decomposition.
- $2^{w+1}n$ per ogni istanza non-ridondante.

**Goal**: $$\max_{U \subseteq V_r} f_r(U)$$

------

Sia $S$ il maximum weighted independent set di $G_t$, ovvero tale che
- $$S \cap V_t \equiv U$$
- $$w(S) = f_t(U)$$

==da finire==