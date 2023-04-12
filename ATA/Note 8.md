---
date: 2023-04-12
draft: true
content:
    - algorithm for big data
---

# Hash Tables
Una **hash table** è una **implementazione randomizzata** per la struttura dati **dizionario**.

## Problema del dizionario
Abbiamo un unvierso $U$ di elementi, vogliamo mantenere un **sottoinsieme dinamico** $S \subseteq U$, tale che supporta le seguenti operazioni:
- **make-dict**: costruire un dizionario $S$ vuoto.
- **insert(u)**: inserire l'elemento $u \in U$ dentro $S$.
- **delete(u)**: rimuovere l'elemento $u \in U$ da $S$.
- **look-up(u)**: determinare se l'elemento $u \in U$ sta dentro il dizionario $S$.

Note:
- conosco a priori $U$.
- non conosco a priori l'insieme $S$ degli elementi che voglio inserire nel dizionario.

Il problema nel modo reale sta nella dimensione di $U$, il quale è potenzialmente illimitato.

- **AVL**:
	- $O(\vert S \vert)$ il spazio.
	- $O(\log{\vert S \vert})$ in tempo.

- **Hash-table**:
	- $O(\vert S \vert)$ il spazio.
	- $O(1)$ in tempo **medio**.


**Idea** mappo gli elementi di $U$ con una funzione $h: U \to \lbrace 0,1,..., m-1 \rbrace$, con $m \approx n := \vert S \vert$.
Ho quindi una tabella $H$ dove salvo in $H\left[ h(u) \right] \leftarrow u$.
Quando accade che $h(u) = h(v) = i$ allora preservo nello slot $i$ una lista dove ho sia $u$ che $v$.
Questo approccio è anche detto **hashing with chaining**.

Operazioni:
- calcolo $h(u)$.
- inserisco/rimuovo/aggiorno $H\left[ h(u) \right]$.

**GOAL**: vogliamo definire una funzione $h$ che **sparpaglia uniformemente** gli elementi di $U$.

- **obs**: se scegliamo una funzione **deterministica** $h$, esiste sempre un worst case che mappa tutti gli elementi in un singolo slot.

Usando la randomness, posso scegleire $h(u)$ **uar** in $0,1,..., m-1$.
Devo però **ricordare** $h(u)$ per poter poi fare l'operazione di look-up(u).
Ovviamente per risolvere questo problema, mi servirà poi un dizionario del tipo $H' \left[ u \right] \leftarrow h(u)$ (male).

> **Universal Hashing**
> Supponiamo di avere una **famiglia di funzioni** $\mathcal{H}$.
> $\mathcal{H}$ è detta **universale** se per ogni $u,v \in U$, campionando una generica funzione $h \in \mathcal{H}$ allora avremo che $$P(h(u) = h(v) \;\vert\; h \in \mathcal{H}) \leq \frac{1}{m}$$

^8209df

> **THM**
> Sia $\mathcal{H}$ una [[#^8209df|famgilia universale]]. Fissiamo un $S \subseteq U$, e scegliamo un $u \in S$.
> Campioniamo un $h \in_{uar} \mathcal{H}$.
> Sia $X$ la v.a. che conta quanti elementi sono mappati nello slot $h(h)$.
> Si può dimostrare che $$\mathbb{E}\left[ X \right] \leq 1 + \frac{n}{m}$$
> **Proof**
> Per ogni $s \in S$ definiamo la v.a. indicatrice
> $$X_s = \begin{cases}
1 &\text{if } h(s) = h(u)\\
0 &\text{otherwise}
\end{cases}$$
> Certamente avremo che $$X = \sum_{s \in S}X_s$$
> Per **linearità** della media avremo che
> $$\begin{align}
\mathbb{E}\left[ X \right]
&= \mathbb{E}\left[ \sum_{s \in S}X_s \right]\\
&= \sum_{s \in S} \mathbb{E}\left[ X_s \right]\\
&= \sum_{s \in S} P(h(s) = h(u))\\
&\leq 1 + \sum_{s \in U \setminus S} P(h(s) = h(u))\\
&= 1 + \frac{n}{m} \;\; \square
\end{align}$$

## Funzioni Hash-Universali

#### Scegliere $m$
Dobbiamo scegleire la dimensione di $m$.
Scegliamo $m$ come un numero **primo** tale che $n \leq m \leq 2n$.

```ad-info
Esiste sempre un primo $n \leq m \leq 2n$, e si può anche trovare in maniera veloce.
```


#### Integer encoding
Codifichiamo qualsiasi elemento $x \in U$ come un numero di $r$ cifre in base $m$.
$$x = (x_1,...,x_r)_{m}$$
Osserviamo che nella rappresentazione in base $m$ avremo che $$0 \leq x_i \leq m-1 \;\; \forall 1 \leq i \leq r$$

#### Hash Function
Dato un elemento $a \in U$, con rappresentazione $a = (a_1,...,a_r)$, ogni volta che arriva un nuovo elemento $x \in U$ calcoliamo $$h_a(x) = \left( \sum_{i=1}^{r} a_ix_i \right) \mod m$$


#### Definiamo la famiglia universale
Poniamo $$\mathcal{H} \equiv \lbrace h_a: a \in U \rbrace$$

```ad-important
title: Modello di calcolo
Stiamo assumendo che il modello di calcolo che stiamo usando è il **word RAM model**.
Si sta assumendo che:
1. stiamo manipolando oggetti di dimensione $O(1)$ in tempo $O(1)$.
2. ogni oggetto di interesse si trova in una **machine word**.
```


> **THM**
> La famgilia definita [[#Definiamo la famiglia universale|prima]] è **universale**.
> 
> **Proof**
> Prendo due $x,y \in U$, e campiono a caso $a \in U$.
> Si vuole dimostrare che $$P(h_a(x) = h_a(y)) \leq 1/m$$
> 
> Se $x \neq y$ allora esiste un intero $j \leq r$ tale che $x_j \neq y_j$.
> Però $$h_a(x) = h_a(y) \iff \sum a_ix_i \equiv_m \sum a_iy_i$$
> Riscriviamo quindi $$a_j(y_j - x_j) \equiv_m \sum_{i \neq j} a_i(x_i - y_i)$$
> Qual è la probabilità che accade ciò, scegliendo $a$ u.a.r. da $U$.
> 
> Osserviamo che campionare $(a_1,...,a_r) \in_{uar} \lbrace 0, ..., m-1 \rbrace^r$ equivale a campionare indipendentemente $a_i \in_{uar} \lbrace 0,...,m-1 \rbrace$.
> 
> Assumiamo di aver scelto a priori tutte le cifre $i \neq j$ u.a.r.
> Ora voglio calcolare con che probabilità ho, campionando $a_j$, di avere $$a_j z = \alpha \mod m$$ (con $z \neq 0$ perché $y_j \neq x_j$).
> 
> Dato che $m$ è primo e $x \neq 0$ allora esiste un **inverso moltiplicativo** $z^{-1}$ t.c. $z \cdot z^{-1} = 1 \mod m$.
> Quindi $$P(h_a(x) = h_a(y)) = P(a_j z = \alpha \mod m) = P(a_j = \alpha z^{-1} \mod m) \leq \frac{1}{m} \;\; \square$$

-----
# Un'altra classe
Scegliamo un **primo** $p \geq \vert U \vert$.

Prendiamo due elementi $a,b \in U$, e definiamo la funzione $$h_{ab}(x) := (ax+b \mod p) \mod m$$

La famiglia universale sarà quindi $$\mathcal{H} \equiv \lbrace h_{ab} : a,b \in U \rbrace$$


```ad-warning
Generalmente scegliamo $m \geq n := \vert S \vert$.
Dato che $S$ è **dinamico**, non conosco $n$ a priori.
```

Ad ogni istante ho deifiniti tre **parametri**:
- $n$: il numero degli elementi che $S$ ha in un dato momento.
- $N$: la dimensione **virtuale** della tabella.
- $m$: la dimensione **attuale** della tabella (un numero primo tra $N$ e $2N$).

### Doubling/halving
Tecnica del **doubling/halving** serve per mantere **dinamica** la dimensione del dizionario, in funzione della dimensione di $S$.
1. Inizializzo $n = N = 0$.
2. Quando $n$ supera $N$
	- $N := 2N$.
	- scelgo un nuovo primo $m$ tra $N$ e $2N$.
	- faccio il **re-hash** di tutti gli elementi (in tempo $O(n)$).
3. Quando $n < N/4$ allora
	- $N := N/2$.
	- scelgo un nuovo primo $m$ tra $N$ e $2N$.
	- faccio il **re-hash** di tutti gli elementi (in tempo $O(n)$).




