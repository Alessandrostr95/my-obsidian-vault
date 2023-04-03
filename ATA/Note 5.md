---
date: 2024-03-29
draft: true
content:
  - parametrized algorithms
  - k-vertex cover
  - FTP
  - bounded-search tree
  - kernelization
---


# K-Path
- **Inptu**:
	- un grafo $G(V,E)$
	- un intero $k \geq 1$
- **Question**:
	- esiste un cammino di lunghezza $k$ ?
- **Parametro**: $k$


```ad-note
Quando $k = n$ abbiamo il problema del ciclo hamiltoniano, e quindi il problema diventa NP-hard.
```


> **THM**
> $k$-Path può essere risolto in tempo $2^{O(k)} \cdot n^{O(1)}$.

Precedenti algoritmi risolvevano il problema in tempo $k^{O(k)} \cdot n^{O(1)}$.

# Color Coding
Idea:
1. coloriamo indipendentemente i nodi con colori da $1,...,k$.
2. cerca un cammino **colorato** con colori per esempio $(1,2,...,k)$. 
	1. Ritorna YES se esiste questo cammino
	2. Ritorna NO se non esiste

**OBS1**: Se non esiste alcun k-cammino allora non esisterà alcun cammino ben colorato

**OBS2**: Se l'istanza iniziale è YES esiste una probabilità che non esista un cammino ben colorato.
In questo la probabilità che risponsa si è **almeno** $\geq k^{-k}$.

```ad-info
Se la probabilità di successo di un evento è $p$, allora la probabilità di sbagliare dopo $1/p$ tentativi sarà $$(1-p)^{1/p} \leq (e^{-p})^{1/p} = 1/3 \approx 0.38$$
```

Se invece di eseguire $k^k$ tentativi facessimo $100 k^k$ tentativi, allora la probabilità di sbagliare sarà al più $(1/e)^{100}$.

## Trovare un cammino ben colorato 1,2,...,k
==vedi immagine==
Divido il grafo in $k$ livelli in base ai colori, e vedo se c'è un cammino dal livello 1 al livello $k$.


## Improve color coding
Per migliorare il fattore $k^k$ basta cercare un cammino non necessariamente nell'ordine $1,2,...,k$ bensì in qualsiasi ordine, basta che copre tutti i colori (**cammino colorfull**).

In questo caso la probabilità di avere suecceso sarà almeno $$\geq \frac{k! n^{n-k}}{k^n}=\frac{k!}{k^k} \underbrace{\geq}_{\text{stirlings}} \frac{(k/e)^k}{k^k}=e^{-k}$$
A questo punto basta cercare un cammino colorfull, il quale è un po' più complicato.

### Finding Colorful Path
- **Subproblems**:
Per ogni nodo $v \in V$, per ogni sottoinsieme non vuoto di colori $S \subseteq \lbrace 1,...,k \rbrace$.
Voglio trovare tutti i cammini $P(v,S)$ che **finiscono** in $v$ e che toccano tutti i colori di $S$.

**OBS1** esiste un colorful path se e solo se esiste un $v$ t.c. $P(v, \lbrace 1,...,k \rbrace) = TRUE$.

**OBS2** Esistono $n \cdot 2^k$ sottoproblemi.


Caso $\vert S \vert = 1$
$$P(v,S) = TRUE \iff S = \lbrace \text{color}(v) \rbrace$$


Caso $\vert S \vert \geq 2$
$$P(v,S) = \begin{cases}
\bigvee_{(u,v) \in E} P(u, S \setminus \lbrace \text{color}(v) \rbrace) &\text{if } \text{color}(v) \in S\\
\\
FALSE &\text{altrimenti}
\end{cases}$$

> **THM**
> Esiste un algoritmo randomizzato per il k-path con tempo di esecuzione $(e2)^k n^{O(1)}$ t.c.
> - è corretto con probabilità 1 se l'istanza è NO
> - è corretto con probabilità arbitrariamente costante se l'istanza è YES

----
# 2k kernel per il Vertex Cover

## Integer-LP formula per VC
$$\text{minimize}\sum_{v \in V} x_v$$
$$\text{s.t.} x_u + x_v \geq 1 \;\; \forall (u,v) \in E$$
$$x_v \in \lbrace 0,1 \rbrace \;\; \forall v \in V$$

Ora lo **rilassiamo** ponendo il vincolo $$x_v \geq 0 \;\; \forall v \in V$$

Ricordiamo che $$OPT_f \leq OPT$$
Sia $x$ una soluzione frazionaria
- $$V_0 = \lbrace v \in V: x_v < 0.5 \rbrace$$
- $$V_{0.5} = \lbrace v \in V: x_v = 0.5 \rbrace$$
- $$V_1 = \lbrace v \in V: x_v > 0.5 \rbrace$$

> **THM**
> Esiste un minimum vertex cover $S$ di $G$ tale che $$V_1 \subseteq S \subseteq V_1 \cup V_{0.5}$$
> Ovvero il min vertex cover $S$ contiene tutti i vertici di $V_1$ e forse qualcuno di $V_{0.5}$.


> **Proof**
> Sia $S^*$ un minimum vertex cover.
> Poniamo $S = (S^* \setminus V_0) \cup V_1$.
> Vogio dimostrare che $\vert S^* \vert = \vert S \vert$.
> 
> Dimostriamo prima che $S$ è un VC.
> Osserviamo che tutti i nodi in $V_0$ hanno necessariamente un vicino in $V_1$.
> 
> Dimostriamo che $S$ è minimo.
> Assumiamo per assurdo che $\vert S \vert > \vert S^* \vert$.
> $$\underbrace{\vert S^* \cap V_0 \vert}_{B} < \underbrace{\vert V_1 \setminus S^* \vert}_{A}$$
> Aggiungiamo quindi $+\epsilon$ a nodi di $B$ e $-\epsilon$ ai nodi di $A$.
> $$\epsilon = \min{\lbrace \vert x_v - 0.5 \vert: v \in V_0 \cup V_1 \rbrace}$$
> Sia $y$ una nuova soluizione frazionaria valida, t.c.
> $$y_v = \begin{cases}
x_v - \epsilon &v \in A\\
x_v + \epsilon &v \in B\\
x_v &\text{altrimenti}
\end{cases}$$
> Si può dimostrare che:
> - $y$ è una soluzione **strettamente migliore** di $x$
> - $y$ è ammissibile
> contraddicendo l'ipotesi di ottimalita di $S^*$.
> 
> La soluzione $y$ è strettamente migliore semplicemente perché **guadagno** un $\epsilon$ per ogni elemento di $B$ ma **perdo** un $\epsilon$ per ogni elemento di $A$, però stiamo assumendo che $B < A$.
> ==vedi i casi==


### Kernelizzazione di size 2k
- troviamo una soluzione frazionaria $x$ **rilassata**
- troviamo gli insiemi $V_0, V_{0.5}, V_1$.
- Se $\sum_{v \in V} x_v > k$ allora $(G,k)$ è una istanza no (perché $OPT_f \leq OPT$).
- Altrimenti, in maniera greedy prendiamo i nodi di $V_1$ e li mettiamo in un vertex cover. La nuova istanza sarà $$(G' = G \setminus (V_1 \cup V_0), k' = k - \vert V_1 \vert)$$

> **THM** $k$-vertex cover ammette un kernel di $2k$ vertici
> **Proof**
> $(G,k)$ è un'istanza SI se e solo se $(G', k')$ è un'istanza SI
> ==da finire==

-----
### Party Problem
- **problem**: invitare le persone a un party
- **maximize**: massimizzare il **fattore di divertimento**. Ogni dipendente ha un numero che indica quanto è divertente.
- **vincolo**: tutti si devono divertire. Se inviti il boss diretto di una persona questa non si diverte più.

==maximum weighted independente set==