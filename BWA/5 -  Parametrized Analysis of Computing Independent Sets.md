# Parametrized Analysis of Computing Independent Sets

# The Maximum-Weight Independent Set Problem
Facciamo ora una panormaica sul problema del **maximal-weight independent set** (in breve **MWIS**).

Una istanza del MWIS è un **grafo non diretto** $G=(V,E)$ con una **funzione di peso sui nodi** $w: V \to \mathbb{R}^{+} \cup \{0\}$.

L'obiettivo è quello di identificare un sottoinsieme di nodi $S \subseteq V$ tale che:
1. $S$ sia **indipendente**, ovvero nessuna coppia di nodi in $S$ è reciprocamente vicina $$\forall u,v \in S \;\; (u,v) \notin E$$
2. $S$ deve **massimizzare** la somma dei pesi dei suoi elementi $$\text{maximize } \sum_{v \in S}w(v)$$

```ad-note
La variante di MWIS è il **caso particolare** del **Maximal Independente Set**, nel quale ogni peso è pari a $w(v) = 1$.
Perciò si vuole solo massimizzare la dimensione di $S$.
```

```ad-important
Dato che la versione decisionale di **MIS** è *NP-completo*, allora è facile dimostrare che anche la versione decisionale di **MWIS** lo è.

$$MIS \preccurlyeq_P MWIS$$

Infatti esiste una **riduzione polinomiale** abbastanza semplice da MIS a MWIS (quale? 😜).
```

È anche noto che MWIS è computazionalmente difficile da approssimare.

> **Fact**
> Per ogni $\varepsilon > 0$, approssimare MWIS entro un fattore di $n^{1-\varepsilon}$ è **NP-hard**.

^f8b049

Ricordiamo che, essendo un problema di ottimizzazione, diciamo che una soluzione $S$ approssima il valore di una soluzione ottima $S^*$ a un fattore $c \geq 1$ se 
$$1 \leq \frac{\text{val}(S^*)}{\text{val}(S)} \leq c$$
o alternativamente se $$\text{val}(S) \geq \frac{1}{c}\text{val}(S^*)$$
Perciò per il problema MWIS, trovare una soluzione $S$ tale che $$\sum_{v \in S} w(v) \geq \frac{1}{n^{1-\varepsilon}}\sum_{v \in S^*}w(v)$$
è un problema **computazionalmente** difficile!

Questo è un risultato parecchio scoragiante, in quanto non abbiamo speranza di trovare un algoritmo **polinomiale** che trova una soluzione **significativa** (ovvero migliore di $1/n^{1 - \varepsilon}$, la quale è molto piccola al crescere di $n$).

## A Randomized Greedy Algorithm
Richiamando l'[[4 - Parameterized Analysis of Online Paging#Parameterized Analysis|analisi parametrizzata]], il **primp step** è quello di **parametrizzare** (in maniera *"naturale"*) gli input.
Una buona idea è quella di parametrizzare mediante il **grado massimo** $\Delta$.
Infatti, analizzando alcuni algoritmi vedremo come una istanza di MWIS diventa più *"semplice"* al decrescere di $\Delta$.
Infatti, un algoritmo che inserice un nodo $v$ con grado $\delta(v)$ molto alto all'interno di $S$ bloccherà un gran numero di altri nodi dall'essere inseriti in seguito dentro la soluzione, perciò intuitivamente non è una buona idea.

Consideriamo un primo (banale) algoritmo greedy e randomizzato (chiamiamolo **randomized greedy** o **RG**), il quale costruisce in ordine casuale e in maniera greedy $S$.

> **Ramdom Greedy**
> **Input:** $\langle G=(V,E), w:V \to \mathbb{R}^+ \cup \{ 0 \} \rangle$
> **Output:** $S \subseteq V: u,v \in S \implies (u,v) \notin E$
> 
> 1. Ordina $V = \{1, 2, ..., n\}$ in accordo a una **permutazione casuale**
> 2. Poni $S \equiv \emptyset$
> 3. for $i = 1$ to $n$
> 	- se $S \cup \{i\}$ è un **insieme indipendente**
> 		- $S \equiv S \cup \{ i \}$
> 4. Return $S$

^935aef

> **Lemma 1**
> Sia $S$ la soluzione tornata da RG.
> Per ogni nodo $v \in V$ avremo che $$P(v \in S) \geq \frac{1}{\delta(v) + 1}$$

^e518bc

> **Proof**
> Consideriamo tutte le possibili permutazioni di $N(v) \cup \{v\}$.
> **Se** nell'ordinamento (relativo) di $N(v) \cup \{v\}$ il nodo $v$ sarà il primo, **allora** $v$ verrà inserito in $S$.
> Perciò se $$v \text{ appare primo in } N(v) \cup \{v\} \implies v \in S$$ allora $$P(v \text{ appare primo in } N(v) \cup\{v\})  \leq P(v \in S)$$
> Tutte queste permutazioni *"buone"* sono in totale $\delta(v)!$, mentre tutte quelle possibili sono $(\delta(v) + 1)!$.
> Perciò (*casi favorevli su casi possibili*) avremo l'occorrenza di questo evento con frequenza $$\frac{\delta(v)!}{(\delta(v) + 1)!} = \frac{1}{\delta(v) + 1!} \;\; \square$$

```ad-warning
Osserva che la disuguaglianza nel [[#^e518bc|Lemma 1]] non per forza è una *uguaglianza*.
Infatti può capitare che un nodo $v$ venga inserito nella soluzione $S$ anche se un suo vicino $w \in N(u)$ è stato *visitato* precedentemente dall'algoritmo.
Infatti $w$ potrebbe essere stato bloccato da un suo vicino già presente in $S$.
```

Possiamo ora dare un lower bound alle prestazioni (*in media*) dell'[[#^935aef|algoritmo RG]].

> **Corollary 1**
> Sia $S \subset V$ una soluzione dell'algoritmo RG.
> Allora il *valore* di $S$ <u>in media</u> sarà $$\mathbb{E}\left[ \sum_{v \in S} w(v) \right] \geq \sum_{v \in V} \frac{w(v)}{\delta(v) + 1}$$

^966e4e

> **Proof**
> Sia la v.a. $W_v$ che descrive il contributo di un nodo $v$ alla soluzione, ovvero
> $$W_v = \begin{cases}w(v) &v \in S\\0 &v \notin S\end{cases}$$
> con media $$\mathbb{E}\left[ W_v \right] = w(v) \cdot P(v \in S) + 0 \cdot P(v \notin S) = w(v) \cdot P(v \in S) \geq \frac{w(v)}{\delta(v) + 1}$$
> Per linearità avremo quindi che la media del valore di $S$ sarà
> $$\mathbb{E}\left[ \sum_{v \in S} w(v) \right] = \mathbb{E}\left[ \sum_{v \in V} W_v \right] = \sum_{v \in V} \mathbb{E}\left[  W_v \right] = \sum_{v \in V} w(v) \cdot P(v \in S) \geq \sum_{v \in V} \frac{w(v)}{\delta(v) + 1} \;\; \square$$

Ricordando che un upper bound al valore di una soluzione ottima è $\sum_{v \in V} w(v)$, avremo che
> **Corollary 2**
> Per ogni grafo di **grado massimo** $\Delta$ avremo che in media il valore della soluzione $S$ calcolata dall'algoritmo RG è $$\mathbb{E}\left[ \sum_{v \in S} w(v) \right] \geq \frac{\text{val}(S^*)}{\Delta + 1}$$ dove $S^*$ è una **soluzione ottima** al problema MWIS.

^da3594

> **Proof**
> Sia $S^*$ una soluzione ottima, ed $S$ una soluzione calcolata dall'algoritmo RG.
> Sappiamo che $\text{val}(S^*) \leq \sum_{v \in V} w(v)$.
> Grazie al [[#^966e4e|Corollary 1]] abbiamo che $$\mathbb{E}\left[ \sum_{v \in S} w(v) \right] \geq \sum_{v \in V} \frac{w(v)}{\delta(v) + 1} \geq \sum_{v \in V} \frac{w(v)}{\Delta + 1} \geq \frac{\text{val}(S^*)}{\Delta + 1} \;\; \square$$

```ad-note
Osserviamo che il [[#^da3594|Corollario 2]] definisce un cattivo bound nel caso in cui $\Delta = n-1$ (grado massimo possibile).
Questo però ce lo aspettavamo per via del fatto che MWIS è [[#^f8b049|difficilmente approssimabile]].
```


-----
# The Recoverable Value
