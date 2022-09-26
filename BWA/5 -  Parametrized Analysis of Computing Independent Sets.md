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
> Perciò (*casi favorevli su casi possibili*) avremo l'occorrenza di questo evento con frequenza $$\frac{\delta(v)!}{(\delta(v) + 1)!} = \frac{1}{\delta(v) + 1} \;\; \square$$

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
Purtroppo il parametro $\Delta$ è "*troppo delicato*".
Infatti a piccole variazioni del grafo $G$, il grado massimo $\Delta$ può variare di molto!

```ad-example
Consideriamo un grafo $G$ con un grande numero $n$ di nodi, e un grado massimo $\Delta$ molto piccolo ($o(n)$).
Il [[#^da3594|Corollario 2]] ci garantisce che l'algoritmo RG computerà (in media) un independent set con valori ragionevolmente vicini a quelli ottimi.

Modifichiamo ora $G$ aggiungendo un nodo $v^*$, ed $n$ archi da $v^*$ verso tutti gli altri nodi.
Chiamiamo questo grafo $G'$.
$$G' \equiv (V \cup \{v^*\}, E \cup \{ (v^*,u): u \in V \})$$

Supponiamo che il peso di $v^*$ sia abbastanza piccolo da non includerlo nella soluzione ottima $S$ per $G$.

In $G'$ le prestazioni di RG rimangono pressoché invariate.
Infatti avrà pessime prestazioni quando $v^*$ verrà inserito nella soluzione (e se succede nessun altro non può essere inserito), però ciò accade con probabilità $1/(n+1)$.


Perciò mediamente la soluzione ritornata sarà sempre almeno $1/(\Delta + 1)$ volte quella ottima, però l'analisi fatta in precedenza nel [[#^da3594|Corollario 2]] prova solo un fattore di approssimazione $n+1$.
```

^e33bc9

L'osservazione chiave che ci suggerisce l'[[#^e33bc9|esempio precedente]] è che:
anche se $G'$ ha un grado massimo molto grande (ovvero $n+1$), la soluzione ottima $S^*$ comprende solo i nodi con **grado basso** (a più $\Delta + 1$ in $G'$, o $\Delta$ in $G$).

Perciò sfruttando in [[#^966e4e|Corollario 1]] abbaimo che $$\mathbb{E}\left[ \sum_{v \in S} w(v) \right] \geq \sum_{v \in V} \frac{w(v)}{\delta(v) + 1} \geq \sum_{v \in S^*} \frac{w(v)}{\delta(v) + 1}$$ il che è sufficiente per implicare il [[#^da3594|Corollario 2]] (*convinciti di questo*). ^c25eb1

Perciò possiamo fare un'analisi *più raffinata* basandoci sui gradi dei nodi all'interno della soluzione ottima $S^*$ (come abbiamo fatto per l'algoritmo [[2 - Instance-Optimal Geometric Algorithms#The Kirkpatrick-Seidel Algorithm|Kirkpatrick-Seidel]], in cui parametrizzavamo in accordo con la [[2 - Instance-Optimal Geometric Algorithms#More refined bound|dimensione dell'output]] $h$, oltre che rispetto a $n$).

Possiamo quindi **generalizzare** l'[[#^c25eb1|espressione precedente]] come
$$\mathbb{E}\left[ \sum_{v \in S} w(v) \right] \geq \underbrace{1}_{\text{"recoverable value"}} \cdot \max_{\text{ind. set }I} \sum_{v \in I} \frac{w(v)}{\delta(v) + 1}$$ ^c6ec29

Il punto della questione è che l'algoritmo RG soddisfa una relazione forte su qualsiasi sottoinsieme di $V$, non solo rispetto agli *independente set* $I$.
In pratica, ogni algoritmo che soddisfa la [[#^c6ec29|precedente relazione]] garantisce **ottime prestazione** ogni volta che esiste un *independente set* $I$ **quasi ottimale** che è composto da soli vertici con **grado basso**.

Ancora una volta, abbiamo identificato una famiglia di istanze per le quali si possono garantire buone prestazioni.
Perciò ci possiamo chiedere:
> Le *"istanze tipiche"* di MWIS hanno soluzioni **quasi ottime** che comprendono solo, o per lo più, nodi di grado basso?

Purtroppo non è facile da dirsi.
Abbiamo solo la vaga intuizione che i nodi di grado basso siano generalmente "*migliori*" di quelli di grado superiore, perché "*bloccano*" l'inclusione di un minor numero di altri nodi.
È anche difficile computazionalmente da verificare tale condizione (problema decisionale NP-hard), se volessimo una [[1 - Introduction#Goal 1 Performance Prediction|predizione delle performance]] data un'istanza.

Possiamo però sfruttare i risultati raggiunti come [[1 - Introduction#Goal 3 Design New Algorithms|guida]] per la progettazione di algoritmi con migliori garanzie (in pratica).

## Optimizing the Recoverable Value
Spostiamo per il momento l'obiettivo dall'**analisi** alla **progettazione** di algoritmi.

Possiamo porci come obiettivo quello di fare meglio della costante "1" (la **recoverable value**) nell'[[#^c6ec29|espressione precedente]].

Innanzitutto notiamo che non possiamo porre la *recoverable value* pari a 4 se vogliamo un algoritmo polinomiale, a meno che $P = NP$.
Infatti, per esempio, consideriamo un **grafo 3-regolare** (con tutti i nodi di grado $3$) e rimpiazziamo la *recoverable value* con 4.

Se avessimo un algoritmo che garantisce tale bound, abbiamo trovato un algoritmo che trova l'ottimo in tempo polinomiale su grafi 3-regolari.
Però è possibile dimostrare che trovare un MWIS su un grafo 3-regolare è NP-hard.

Definiamo ora un algoritmo che migliora la recoverable value di un fattore *al più* 2.
> **Feige-Reichman (FG) algorithm**
> **Input:** $\langle G=(V,E), w:V \to \mathbb{R}^+ \cup \{ 0 \} \rangle$
> **Output:** $S \subseteq V: u,v \in S \implies (u,v) \notin E$
> 
> 1. Ordina $V = \{1, 2, ..., n\}$ in accordo a una **permutazione casuale**
> 2. Poni $T \equiv \emptyset$
> 3. for $i = 1$ to $n$
> 	- se **al più** 1 dei vicini del nodo $i$-esimo è già stato visitato dall'algoritmo
> 		- $T \equiv T \cup \{ i \}$
> 4. Return un MWIS $S$ del sottografo **indotto** $G\left[T\right]$

^16c11e

> **Theorem 1**
> L'[[#^16c11e|algoritmo FG]] computa in **tempo polinomiale** un *independent set* con valore atteso **almeno** $$\geq \max_{\text{ind. set }I} \sum_{v \in I} w_v \cdot \min\bigg\lbrace 1, \frac{2}{\delta(v) + 1}\bigg\rbrace$$

Prima di procedere con la dimostrazione, sono necessari dei lemmi.

> **Lemma 2**
> Con probabilità 1, il grafo indotto $G\left[ T \right]$ computato nell'algoritmo [[#^16c11e|FG]] è una **foresta**.

^c6839f

> **Proof (Lemma 2)**
> Per dimostrare il [[#^c6839f|Lemma 2]] basta dimostrare che $G\left[ T \right]$ è **aciclico**.
> 
> Se $G$ non ha cicli allora semplicemeten neache $G\left[ T \right]$ può averne.
> Supponiamo invece che $G$ abbia cicli.
> Sia $C$ un generico ciclo in $G$.
> Nell'ordinamento casuale dell'algoritmo, l'ultimo nodo visitato non può essere inserito in $T$, in quanto (essendo l'ultimo dei nodi di $C$) avrà *almeno due* suoi vicini visitati in precedenza.
> Perciò $G\left[ T \right]$ è una foresta $\square$.

> **Lemma 3**
> L'algoritmo FR può essere implementato in **tempo polinomiale**.

^11c4f9

> **Proof (Lemma 3)**
> La permutazione e il ciclo *for* richiedono tempo *lineare* in $n$.
> Per quanto riguarda il trovare un MWIS $S$ di $G\left[ T \right]$, anche esso richiede tempo polinomiale!
> Infatti, trovare un MWIS di un albero può essere computato grazie a un algoritmo di **programmazione dinamica** in tempo polinomiale.
> Dato che $G\left[ T \right]$ è una [[#^c6839f|foresta]], possiamo calcolare i MWIS $S_1, ..., S_k$ dei $k \geq 1$ alberi che compongono $G\left[ T \right]$.
> Quindi $S = S_1 \cup ... \cup S_k$ è a sua volta un MWIS per $G\left[ T \right]$ $\square$.

> **Proof (Theorem 1)**
> Grazie al [[#^11c4f9|Lemma 3]] sappiamo che FR è un algoritmo che computa una soluzione in <u>tempo polinomiale</u>.
> 
> Calcoliamo ora il valore (in media) della soluzione.
> Fissiamo un <u>generico</u> independente set $I$ per $G$.
> Dato che $T \subseteq V$, allora $I \cap T$ è un independent set per $G\left[ T \right]$.
> Dato che FR ritorna un **massimo** independent set $S$ per $G\left[ T \right]$, allora $$\mathbb{E}\left[ \text{val}(I \cap T) \right] \leq \mathbb{E}\left[ \text{val}(S) \right]$$
> Osserviamo che un nodo $v \in I$ contribuisce alla soluzione $I \cap T$ (per $G\left[ T \right]$) se e solo se $v \in T$, il che occorre se e solo se il nodo $v$ è <u>il primo o il secondo</u> nell'ordinamento di $v$ rispetto al suo vicinato $N(v) \cup \{v\}$.
> $$P(v \text{ è primo o seconod in } N(v) \cup \{v\}) = \frac{2}{\delta(v) + 1}$$
> Osserviamo che $2/(\delta(v) + 1)$ è maggiore di 1 quando $v$ non ha vicini, perciò esse apparterrà a $I \cap T$ con probabilità 1.
> 
> Perciò $$P(v \in I \cap T) = \min\bigg\lbrace 1, \frac{2}{\delta(v) + 1}\bigg\rbrace$$
> In conclusione, applicando la linearità avremo che $$\mathbb{E}\left[ \text{val}(I \cap T)\right] = \sum_{v \in I \cap T}\mathbb{E}\left[ W_v \right] = \sum_{v \in I \cap T} w(v) \cdot  \min\bigg\lbrace 1, \frac{2}{\delta(v) + 1}\bigg\rbrace$$
> Perciò se questo è vero per ogni independent set $I \cap T$ di $G\left[ T \right]$, lo sarà anche per il massimo, implicando il teorema
> $$\mathbb{E}\left[ \text{val}(S) \right] \geq \max_{\text{ind. set } I}\mathbb{E}\left[ \text{val}(I) \right] = \max_{\text{ind. set } I} \sum_{v \in I \cap T} w(v) \cdot  \min\bigg\lbrace 1, \frac{2}{\delta(v) + 1}\bigg\rbrace \;\; \square$$