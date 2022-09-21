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

> **Lemma 1**
> Sia $S$ la soluzione tornata da RG.
> Per ogni nodo $v \in V$ avremo che $$P(v \in S) \geq \frac{1}{\delta(v) + 1}$$

> **Proof**
> Consideriamo tutte le possibili permutazioni di $N(v) \cup \{v\}$.
> Se nell'ordinamento (relativo) di $N(v) \cup \{v\}$ il nodo $v$ sarà il primo, allora $v$ verrà inserito in $S$.
> Tutte queste permutazioni *"buone"* sono in totale $\delta(v)!$, mentre tutte quelle possibili sono $(\delta(v) + 1)!$.
> Perciò (casi favorevli su casi possibili) avremo l'occorrenza di questo evento con frequenza $$\frac{\delta(v)!}{(\delta(v) + 1)!} = \frac{1}{\delta(v) + 1!} \;\; \square$$

