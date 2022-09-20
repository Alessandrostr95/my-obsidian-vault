# Parameterized Analysis of Online Paging
Ricordiamo da [[3 - Online Paging and Resource Augmentation]] che l'[[3 - Online Paging and Resource Augmentation#Competitive Analysis|analisi competitiva]] non risulta essere molto utile per soddisfare gli [[1 - Introduction#Goals of Analyzing Algorithms|obiettivi desiderati]].

Infatti per quanto riguarda il [[1 - Introduction#Goal 1 Performance Prediction|Goal 1]], abbiamo visto che il [[3 - Online Paging and Resource Augmentation#^2d11f5|competitive ratio]] non è una buona unità di misura delle prestazioni di un algoritmo online di paging, in quanto assumono valori [[3 - Online Paging and Resource Augmentation#^f725b1|troppo grandi]] per essere prese sul serio.

Per il [[1 - Introduction#Goal 2 Identify Optimal Algorithms|Goal 2]] abbiamo invece visto che l'analisi competitiva identifica [[3 - Online Paging and Resource Augmentation#^adc4cd|LRU come ottimo]], però identifica come ottimo anche algoritmi intuitivamente (e <u>empiricamente</u>) peggiori (come [[3 - Online Paging and Resource Augmentation#^cd1d79|FWF]]).

In seguito abbiamo introdotto la tecnica della [[3 - Online Paging and Resource Augmentation#Resource Augmentation|Resource Augmentation]].
Abbiamo visto che ci consentiva i fare un passo avanti rispeto al [[1 - Introduction#Goal 1 Performance Prediction|Goal 1]], infatti spiega come crescono le prestazioni (in termini di [[3 - Online Paging and Resource Augmentation#^2d11f5|competitive ratio]]) di un algoritmo online al crescere dell'**aumento di risorse** (ovvero la dimensione della *cache*).

Purtroppo, riguardo il [[1 - Introduction#Goal 2 Identify Optimal Algorithms|Goal 2]], la *resource augmentation* non ci aiuta, in quanto non distingue LRU, da FIFO da FWF.

Intuitivamente, poiché la *superiorità empirica* di LRU sembra essere guidata dalle **proprietà** dei dati del "*mondo reale*" (ovvero la **località di riferimento**) non possiamo aspettarci di riuscire a distinguere LRU da altri algoritmi di paging naturali (come FIFO) senza almeno articolare implicitamente tali proprietà.

-----------
# Parameterized Analysis
Informalmente l'**analisi parametrizzata** consiste in due *"step"*.

Il **primo step** è quello di definire un **parametro naturale** per ogni input, il quale dovrebbe modellare la *"facilità"* o *"difficoltà"* intuitiva dell'istanza.
Ricordiamo infatti che siamo partiti dall'idea che esistono istanze più *"semplici"* da risolvere rispetto ad altre.

![|600](BWA_04_1.png)

Il **secondo step** è quello di analizzare le prestazioni di un algoritmo in funzione del parametro scelto.
Questo consente un'analisi più precisa rispetto alla classica analisi *worst-case*, la quale caratterizza le istanze **solo** in funzione della loro dimensione.

Idealmente, nell'applicazione dell'analisi parametrica il parametro dovrebbe essere **indipendente** dall'algoritmo.

```ad-example
title: Example: Running Time of the Kirkpatrick-Seidel Algorithm
Richiamando l'[[2 - Instance-Optimal Geometric Algorithms#The Kirkpatrick-Seidel Algorithm|algoritmo Kirkpatrick-Seidel]], 
abbiamo analizzato in [[2 - Instance-Optimal Geometric Algorithms]] le sue prestazioni rispetto ad ogni istanza, mostrando che è KS è un algorimo [[1 - Introduction#^67367e|instance optimal]].

Infatti siamo partiti analizzando l'algoritmo solamente rispetto al numero dei punti $n$, trovando un upperbound (poco preciso) di $O(n\log{n})$.

Abbiamo poi raffinato l'analisi, parametrizzando anche rispetto alla grandezza dell'output $h$, dando un upperbound migliore di $O(n\log{h})$ **per ogni input**.

In fine abbiamo definito una **parametrizzazione** della *"facilità"* (o *"difficoltà"*) degli input, che era **indipendente** dalla descrizione dell'algoritmo KS, e che dava un upperbound migliore.
```

-------------
# Quantifying Locality: The Working Set Model
Ciò che vogliamo quindi fare è:
1. **parametrizzare** la sequenza di richieste di pagine in accordo alla sua **località**
2. dimostrare che le prestazione del LRU sono eccellenti quando in input riceve pagine con un **alto grado di località**, sia in termini assoluti di prestazioni, sia in rapporto con altri algoritmi di paging.

Esistono svariati modi di parametrizzare il grado di località di una sequenza di richieste.
Uno tra i più famosi modelli è il **[working set model](https://en.wikipedia.org/wiki/Working_set)**, nel quale si parametrizza tramite una funzione $f: \mathbb{N} \to \mathbb{N}$ che descrive quante richieste **distinte** vengono fatte in una sottosequenza di una data lunghezza.

Formalmente diciamo che una sequenza $\sigma$ è **conforme** ad $f$ se, per ogni $n \in \mathbb{N}$ e per ogni sottosequenza di $\sigma$ di $n$ pagine consecutive, ci sono **al più** $f(n)$ **pagine distinte**.

Osserviamo che intuitivamente più $f$ è piccola, più impone **località** ad una sequenza $\sigma$ conforme ad $f$.

```ad-example
Se $f(n)=n$ avremo che ogni sequenza è conforme ad $f$.

Con $f(2)=1$ avremo che la sola sequenza conforme ad $f$ è quella del tipo $\langle a,a,a,a,...\rangle$

Con $f(n) \approx \sqrt{n}$ avremo che solamente sequenze lunghe **almeno** 100 contengono **almeno** 10 pagine differenti.
Mentre con $f(n) \approx 1 + \log_2{n}$ avremo che solamente sequenze lunghe **almeno** 1000 contengono **almeno** 10 pagine differenti.
```

Noi concetreremo la nostra analisi sulle **prestazioni assolute** (detto anche **page fault ratio**), ovvero sul rapporto $\frac{\text{\# faults}}{\vert \sigma \vert}$, anziche sul [[3 - Online Paging and Resource Augmentation#^2d11f5|competitive ratio]].
Questo perché considerare le prestazioni assolute evita confronti con gli algoritmi ottimi offline.
Infatti sappiamo che LRU ha un [[3 - Online Paging and Resource Augmentation#^adc4cd|competitive ratio ottimo]], e che la competitive anylisis non ci aiuta bene ad identificare quale algoritmo è migliore di altri.

--------------
# Main Results
## Theorem
1. Per ogni funzione **concava** $f$, per ogni dimensione della cache $k \geq 2$, e per ogni algoritmo offline deterministico $A$, il **page fault rate** nel **caso peggiore** su tutte le sequenze **conformi** ad $f$ è **almeno** $\geq \alpha_f(k)$. ^be9407
2. Per ogni funzione **concava** $f$, per ogni dimensione della cache $k \geq 2$, il **page fault rate** nel **caso peggiore** su tutte le sequenze **conformi** ad $f$ dell'algoritmo LRU è **al più** $\leq \alpha_f(k)$. ^e14157
3. Esistono una funzione **concava** $f$, una dimensione della cache $k \geq 2$ e una sequenza $\sigma$ conforme a $f$ tali che il **page fault ratio** dell'algoritmo FIFO sulla sequenza $\sigma$ è **strettamente maggiore** di $> \alpha_f(k)$. ^55127d

I punti [[#^be9407|(1)]] e [[#^e14157|(2)]] sono in qualche modo *analoghi* al [[3 - Online Paging and Resource Augmentation#A Lower Bound for all Deterministic Algorithms|lower bound del competitive ratio degli algoritmi deterministici online]] e all'[[3 - Online Paging and Resource Augmentation#An Upper Bound for LRU|uppero bound del competitive ratio dell'LRU]].
Questi risultati sono però migliori rispetto a quelli sulla **competitive analysis**.
Infatti i punti [[#^be9407|(1)]] e [[#^e14157|(2)]] implicano l'ottimalità di LRU nel caso peggiore per ogni $f$ (valido), per ogni dimensione della cache $k$ e su ogni sequenza conforme ad $f$.
Mentre la competitive analysis lo fa solo in funzione della dimensione della cache $k$.

In fine (ma non per ultimo) il punto [[#^55127d|(3)]] **separa rigorosamente** l'algoritmo LRU dall'algoritmo FIFO (finalmente).
Ovvero abbiamo dimostrato che LRU è ottimale su ogni $f$ e $k$ (validi), mentre FIFO no, soddisfando quindi anche il [[1 - Introduction#Goal 2 Identify Optimal Algorithms|Goal 2]] (*finalmente*).

```ad-note
title: Osservazione 1
Non c'è nessuna perdita di generalità nell'assumere che $f(n+1) \leq f(n) + 1$ per ogni $n$.
Infatti ogni finestra di dimensione $n$ che ha **al più** $f(n)$ pagine distinte, allora automaticamente la finestra di dimensione $n+1$ avrà **al più** $f(n) + 1$ pagine distinte.

Simmetricamente possiamo anche assumere che $f(n+1) \geq f(n)$ per ogni $n$.
```

^84a673

```ad-note
title: Osservazione 2
In base alla [[#^84a673|Osservazione 1]] possiamo pensare ad una $f$ valida come una sequenza di 1, poi di 2, poi di 3, e così via...


$f(n)$ | 1 | 2 | 3 | 3 | 4 | 4 | 4 | 5 | ...
-|-|-|-|-|-|-|-|-|-
$n$ | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | ...

Possiamo quindi descrivere l'intero $f$ definendo per ogni $j \in \mathbb{N}$ il valore $m_j$ come il **numero di valori consecutivi di** $n$ tali che $f(n) = j$.

![|600](BWA_04_2.png)
```

^88c70f

```ad-note
title: Osservazione 3
Grazie alla [[#^84a673|osservazione 1]] e [[#^88c70f|osservazione 2]] possiamo dare una **definizione alternativa** al concetto di **funzione concava**.
Nel nostro caso infatti avremo che $f$ è convessa se $m_1 \leq m_2 \leq m_3 \leq ...$
```

```ad-important
Considerare solo funzione $f$ **concave** non è una restrizione eccessiva.
Infatti su dati reali la concavità è **empiricamente presente**.

Anche nel caso di dati nei quali si osserva una **funzione empirica di working set** $\hat{f}$ **non concava**, si può sempre trocare un $f$ concava tale che $\hat{f}(n) \leq f(n)$ per ogni $n$.
```




