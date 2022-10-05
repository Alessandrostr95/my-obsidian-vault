# Beyond Worst-Case Analysis
Confrontare algoritmi diversi è difficile.
Per quasi tutte le coppie di algoritmi, e per qualsiasi misura di prestazioni dell'algoritmo come il **tempo di esecuzione** o la **qualità della soluzione**, ogni algoritmo funzionerà meglio dell'altro su alcuni input.
Ad esempio, l'algoritmo di ordinamento **insertion sort** è più veloce del **merge sort** su array già ordinati, ma più lento su molti altri input.

Quando due algoritmi hanno prestazioni incomparabili?
Come possiamo ritenere uno di loro "**migliore**" dell'altro?

L'analisi **worst-case** è uno dei modelli più usato per l'analisi della qualità di un algoritmo.
Nel modello *worst-case*, le prestazioni complessive di un algoritmo sono riassunte dalle sue **peggiori prestazioni** su qualsiasi input: si analizzano le prestazioni nel **caso peggiore**.
L'algoritmo *"migliore"* è quindi quello che ha le prestazioni migliori nel caso peggiore.

Il *merge sort*, con il suo tempo di esecuzione (**asintotico**) nel caso peggiore di $\Theta(n \log{n})$ per gli array di lunghezza $n$, è migliore in questo senso rispetto all'*insertion sort*, che ha un tempo di esecuzione peggiore di $\Theta(n^2)$.

L'analisi *worst-case* risulta estremamente utile, ed è il paradigma dominante per l'analisi degli algoritmi nell'informatica teorica.
Un algoritmo con buone prestazioni nel caso peggiore garantisce una buona qualità per qualsiasi input possibile.

In realtà però esistono svariati problemi per i quali il "caso peggiore" risulta essere **computazionalmente difficile**, ma che all'atto pratico la maggior parte delle istanze che ci si trova a dover risolvere in contesti reali possono essere risolte in tempo più che ragionevole.

Ovvero esistono alcuni algoritmi che risultano essere **empiricamente migliori** di altri.

Vediamo alcuni esempi.

## Caching problem: LRU vs. FIFO
Abbiamo una memoria molto piccola ma molto veloce (una **cache**), e un'altra memoria molto grande ma molto letta in scrittura e lettura che funge da archivio.

Supponiamo di avere un *programma* che periodicamente fa delle richieste di scrittura/lettura a dei dati, suddivisi in **pagine** e conservate nell'archivio.

Nel caso fortunato in cui la pagina richiesta è presente in cache, la richiesta viene soddisfatta in tempi brevissimi.
Nel caso in cui invece non è presente in cache, avviene un cosiddetto **page fault**, e la pagina deve essere caricata dall'archivio alla cache.
Se c'è spazio in cache, allora la pagina viene caricata direttamente, altrimenti bisogna trovare una politica che decida quale pagina in cache dovrà essere rimossa per fare spazio a quella nuova.

```ad-example
title: Esempio

Abbiamo una cache con una capacità massima di 4 pagine.
Supponiamo che le prime richieste di pagine sono $\langle a,b,c,d \rangle$.
Essendoci spazio libero, le pagine possono essere caricate in cache direttamente.

Le successive due richieste sono $\langle d,a \rangle$.
Dato che già in cache, non si necessita di nessuna operazione.

A un certo punto viene richiesta la pagina $e$.
Dato che non presente in cache, e dato che non c'è più spazio, sarà necessario rimuovere una tra le pagine $a,b,c,d$.

Per esempio applicando la politica [FIFO](https://en.wikipedia.org/wiki/Cache_replacement_policies#First_in_first_out_(FIFO)) verrà rimossa la pagina $a$ (la prima arrivata), mentre con la politica [LRU](https://en.wikipedia.org/wiki/Cache_replacement_policies#Least_recently_used_(LRU)) verrà rimossa la pagina $b$ (quella "usata" meno recentemente).

![|600](BWA_01_1.png)

```

Supponiamo di poter prevedere il futuro e di conoscere l'intera sequenza di richieste.
Una politica di rimpiazzo ragionevole è quella di rimuovere ogni volta la pagina che verrà richiesta più in la' nel futuro (**furthest in the future** o politica **FIF**).
Infatti è stato dimostrato che tale approccio *greedy* garantisce il minor numero possibile di rimpiazzi di pagine.
Più precisamente è stato dimostrato che il **page fault rate** (# page fault / # requests) di questa politica greedy (con conoscenza del futuro) è di al più $1/k \%$.

Purtroppo in pratica non conosciamo mai la sequenza di richieste che verranno fatte in futuro, e quindi dobbiamo scegliere una politica **on-line**, ovvero che decida cosa fare man mano che riceviamo *"pezzetti"* di input (ovvero le singole richieste).

È noto che per ogni politica di rimozione deterministica, e per ogni grandezza della cache $k$, esiste **sempre** una sequenza di richieste che fa degenerare il **page fault rate** al $100\%$.
Perciò l'anali *worst-case* ci suggerisce uno scenario catastrofico in cui non abbiamo speranza di progettare una politica efficiente.

In realtà invece, all'atto pratico, esistono politiche che hanno prestazioni *empiricamente* eccellenti.
Più precisamente la politica LRU (con eventuali ottimizzazioni) è il *"gold standard"* per questo problema.

Non si può dire lo stesso della politica FIFO, che risulta empiricamente molto peggiore rispetto di LRU.

Perciò, visto che nel caso peggiore sia FIFO sia LRU risultano pessimi, perché invece dinnanzi a dati reali LRU risulta essere (empiricamente) eccellente rispetto a FIFO?

Esistono forti intuizioni riguardo alla **superiorità empirica** della politica LRU:

> le reali sequenze di richieste possiedono in genere una sorta di **località di riferimento**, ovvero le pagine richieste di recente verranno probabilmente richieste presto in futuro.

Un altro modo di pensare alla politica LRU è che tenta di simulare l'algoritmo FIF ottimale, partendo dal presupposto che

> il futuro prossimo tende ad assomigliare al passato recente.

## Linear Programming
In un problema di **programmazione lineare** si vuole **massimizzare** (o **minimizzare**) una **funzione obiettivo**, del tipo $$\max c^T x$$ dove $c,x$ sono vettori $n$ dimensionali.
Inoltre la soluzione è sottoposta a **vincoli lineari** del tipo $$Ax \leq b$$ dove $b$ è un vettore di dimensione $m$ ed $A$ è una matrice $m \times n$.
Tali vincoli definiscono quindi una **regione di soluzioni ammissibili** per $x$.

![|600](BWA_01_2.png)

L'algoritmo più famoso per risolvere un problema di programmazione lineare è il [metodo del simplesso](https://en.wikipedia.org/wiki/Simplex_algorithm), il quale risolve tale problema tramite una **ricerca locale greedy** sui vertici della regione di soluzioni ammissibili.

Tale algoritmo (e le sue varianti) rimane uno degli algoritmi più usati tuttoggi, ottenendo incredibili prestazioni in pratica.
Infatti il suo *running time* scala tipicamente bene nella dimensione dell'input (ovvero nel numero $n$ di variabili), computando (in pratica) una soluzione in tempo quasi **lineare**, consentendo quindi di poter essere usato anche su problemi con milioni di variabili.

Anche se *empiricamente* il metodo del simplesso è eccellente, cosa possiamo dire riguardo l'analisi *worst-case*?

> **Theorm**
> Esistono istanze di programmazione lineare che inducono il simplesso ad eseguire un numero di iterazioni **esponenziale** in $n$.

^cc27e3

Ovvero il simplesso è (pessimisticamente) inutilizzabile.
Ma allora perché rimane l'algoritmo più usato in assoluto in pratica?

Oltre al danno c'è anche la beffa:
esistono algoritmi di programmazione lineare che hanno <u>complessità <b>polinomiale</b> nel caso peggiore</u> (per esempio l'[ellipsoid method](https://en.wikipedia.org/wiki/Ellipsoid_method)) ma empiricamente non riescono a competere con il metodo del simplesso.

Il [[#^cc27e3|precedente teorema]] ci suggerisce che l'analisi *worst-case* non va bene per l'analisi di algoritmi per problemi di programmazione lineare, in quanto non cattura le performance empiriche degli algoritmi di ottimizzazione lineare.

Alcune motivazioni alle prestazioni empiricamente ottime del metodo del simplesso provengono dalla [smoothed analysis](https://en.wikipedia.org/wiki/Smoothed_analysis), la quale ha provato che l'algoritmo del simplesso computa in tempo polinomiale in *"quasi tutte"* le istanze.

## Clustering
Un ultimo esempio che mostra come il paradigma *worst-case* spesso non descrive bene le prestazioni di un algoritmo è tramite il problema del **clustering**.

Il problema del clustering è una forma di [unsupervised learning](https://en.wikipedia.org/wiki/Unsupervised_learning), ovvero trovare un pattern in dati non classificati.
Infatti l'obiettivo di un algoritmo di clustering è quello di identificare, dato un insieme di dati, dei **gruppi significativamente coerenti**.

Per esempio, potremmo rappresentare delle immagini tramite dei punti su uno spazio Euclideo, e usare un algoritmo di clustering per identificare "foto di cani" e "foto di gatti".

![|600](BWA_01_3.png)

Identificare dei **gruppi significativamente coerenti** è un obiettivo troppo informale, perciò è necessaria qualche formalizzazione del concetto di **coerenza** e **significatività** prima di afforntare il problema.
Infatti un approccio usato per afforntare questo problema è quello di definire una **funzione obiettivo numerica**, per poi ricercare i cluster che ottimizzano al meglio (o approssimativamente) tale funzione.

Alcuni approcci noti sono per esempio il $k$-median o il $k$-means clustering, nei quali si vogliono trovara $k$ **centri** per i cluster, i quali minimizzano la somma delle distanze tra ogni punto e il rispettivo centro più vicino.

Purtroppo conosciamo risultati negativi al riguardo:

> **Meta-Theorem**
> Ogni approccio standard alla modellizzazione del problema del clusterign come un problema di *ottimizzazione*, risulta essere un problema **NP-hard**.


Pertanto, assumendo che $P \neq NP$, nessun algoritmo polinomiale risolve questi problemi di ottimizzazione che modellizzano il clustering (almeno non in maniera esatta).

In pratica però, il clustering non è visto come un problema particolarmente difficile.
Alcuni algoritmi molto semplici di clustering, come l'[algoritmo di Lloyd](https://en.wikipedia.org/wiki/Lloyd%27s_algorithm) per $k$-means, restituiscono i cluster *"intuitivamente corretti"* sui set di punti rappresentanti dati del mondo reale.
Come possiamo quindi riconciliare l'intrattabilità nel caso peggiore dei problemi di clustering, con il *successo empirico* di questi algoritmi semplici?

Una possibile spiegazione è che

> il clustering è difficile solo quando non ha importanza

Ad esempio, se le istanze difficili di un problema di clustering *NP-hard* sembrano solamente un mucchio di punti totalmente a caso e non strutturati, chi se ne frega di classificarli?
Invece il caso d'uso più comune in cui si usano algoritmi di clustering è per punti che rappresentano immagini, documenti, proteine, o alcuni altri oggetti in cui è probabile che esista un *"clustering significativo"*.

Perciò ci si chiede: le istanze che presentano una classificazione significativa, potrebbero essere più facili da risolvere rispetto alle istanze peggiori?

------
# Goals of Analyzing Algorithms
Iniziamo con l'elencare le principali motivazione per cui noi vogliamo analizzare rigorosamente le prestazioni di un algoritmo.

### Goal 1: Performance Prediction
Un primo obiettivo è quello di spiegare o predire le prestazioni *empiriche* di un algoritmo.
Spesso questo obiettivo viene perseguito su un fissato algoritmo, e avvolte tenendo in considerazione una distribuzione dei possibili input (per esempio le distribuzioni che possono avere i dati reali).

Una delle motivazioni di questo obiettivo richiama al lavoro svolto da uno scienziato: prendere un fenomeno osservato (come per esempio *"il metodo del simplesso è veloce"*) come verità fondamentale e cercare un modello matematico che lo spieghi.

Una seconda motivazione richiama invece il lavoro di un ingegnere: volere una teoria che ti consigli se un algoritmo funzionerà bene o meno per un problema che devi risolvere.

### Goal 2: Identify Optimal Algorithms
Un secondo obiettivo è quello di **classificare** gli algoritmi in base alle loro prestazioni, ed idealmente identificarne uno come **ottimo**.

In pratica, dati due algoritmi $A$ e $B$ per uno stesso problema, vorremo una teoria che ci consenta di dire quale è *"migliore"*.
La sfida in questo obiettivo sta nel fatto che le prestazioni di un algoritmo possono variare davvero molto in base alle specifiche istanze (come precedentemente visto).

Osservare che gli obiettivi 1 e 2 sono incomparabili, ovvero possiamo risolvere il secondo senza necessariamente dover risolvere il primo.
Infatti, tramite l'analisi *worst-case*, può capitare di poter dare un consiglio molto accurato su quale algoritmo scegliere, senza però necessariamente predire quali saranno le sue reali performance su una data istanza.

### Goal 3: Design New Algorithms
L'ultimo obiettivo è quello di avere una guida che aiuti lo sviluppo di nuovi algoritmi.

Infatti una volta data una misura della qualità degli algoritmi ([[#Goal 2 Identify Optimal Algorithms|obiettivo 2]]) possiamo porci come compito quello di migliorare lo *"stato dell'arte"* (l'algoritmo migliore noto fin ora) rispetto alla data misura di qualità.

# Summarizing Algorithm Performance
L'analisi *worst-case* è il paradigma dominante nell'analisi formale degli algoritmi.

Diamo una definizione formale di questo paradigma.
Sia $A$ un algoritmo e $z$ una qualsiasi istanza ammissibile.
Definiamo la **funzione di misura** $\text{cost}(A,z)$ che descrive la quantità rilevante di **risorse** che l'algoritmo $A$ *"consuma"* quando riceve l'input $z$.
Questa misura può indicare per esempio il **running time**, lo **spazio** usato in memroia, il **numero di operazioni di I/O** fatte, oppure la **qualità dell'output**. ^db5c4b

Se quindi $\text{cost}(A,z)$ descrive le *"prestazioni"* di $A$ rispetto al dato input $z$, cosa possiamo dire riguardo le *"prestazioni complessive"* di $A$ rispetto ad ogni possibile input?

Sia $Z$ lo spazio dei possibili input.
Dato che i possibili input sono generalmente **infiniti** si parametrizza $Z$ in base a una caratteristica, generalmente la **dimensione** $n$ dell'istanza.

Perciò sia $Z_n$ l'insieme di tutte le istanze di dimensione $n$:
l'analisi *worst-case* riassume le prestazioni di un algoritmo $A$ come $$\max_{z \in Z_n} \text{cost}(A,z)$$

# Pro e Contro
## Pro dell'analisi worst-case
1. **Good worst-case upper bounds are awesome.** Qualora un algoritmo garantisce un buon (ovvero piccolo) **upper bound** al caso peggiore, allora possiamo anche dimenticarci della natura del dominio applicativo.
2. **Mathematical tractability.** Di solito è più facile stabilire un bound alle prestazioni nel caso peggiore di un algoritmo, anziché cercare di riassumere le prestazioni per tutte le istanze in generale. Ricordiamo che l'utilità di un modello matematico non cresce necessariamente in maniera monotona all'aumentare del suo realismo/complessità: infatti modello matematico più accurato non è utile se non si possono dedurne proprietà interessanti.
3. **No data model.** L'analisi worst-case non fa alcuna assunzione riguardo l'input. Per tale ragione risulta essere orientata ad "uso generale", garantendo risultati che sono utili su tutto il range di domini applicativi.

## Contro dell'analisi worst-case
1. **Overly pessimistic.** Abbiamo visto come il riassumere le prestazioni di un algoritmo tramite il caso peggiore può **sovrastimare troppo** le prestazioni sulla maggior parte degli input (come per esempio nel caso del [[#Linear Programming|metodo del simplesso]], dove l'analisi worst-case raffigurava in maniera troppo pessimistica le reali prestazioni dell'algoritmo)
2. **Can rank algorithms inaccurately.** Riassumere le prestazioni in maniera eccessivamente pessimistica può impedire all'analisi worst-case di identificare l'algoritmo (empiricamente) giusto da utilizzare nella pratica ([[#Goal 1 Performance Prediction]]). Per esempio tramite l'analisi worst-case avremo che nel problema del [[#Caching problem LRU vs FIFO|caching]] non c'è distinzione tra politica FIFO e LRU, mentre nella [[#Linear Programming|programmazione lineare]] conviene usare l'*ellipsoid method* anziché il simplesso.
3. **No data model.** O meglio, l'analisi worst-case corrisponde al modello di dati *"[Legge di Murphy](https://en.wikipedia.org/wiki/Murphy%27s_law)"*: qualunque algoritmo tu scelga, il fato cospirerà contro di te per produrre il peggior input possibile. Questo modo di pensare dipende solamente dall'algoritmo, considerando i dati coerenti all'applicazione dell'algoritmo.

# Conclusioni sull'analisi worst-case
Riespetto al [[#Goal 3 Design New Algorithms]], l'analisi worst-case ha avuto un enorme successo.
Infatti il tentativo di migliorare le performace nel caso peggiore ha portato i ricercatori a trovare migliaia di nuovi algoritmi e strutture dati, molti dei quali estramemente utili.

Rispetto al [[#Goal 2 Identify Optimal Algorithms]] l'analisi worst-case risulta *"trovarsi nel mezzo"*.
Suggerisce molto bene quale algoritmo è megliore per molti problemi importanti (come *shortest-path*, *minimum spanning tree*, *sorting*, ...) mentre suggerisce molto male per altri problemi (come il [[#Caching problem LRU vs FIFO|caching]], la [[#Linear Programming|programmazione lineare]] o il [[#Clustering|clustering]]).

In fine, l'analisi worst-case pecca parecchio rispetto al [[#Goal 1 Performance Prediction]].
Infatti considerare solamente il caso più sfortunato non è un buon modo per predire o spiegare le prestazioni empiriche delle varie tipologie di istanze.
Quando l'analisi worst-case fa una *previsione accurata* delle performance su vari input è più per caso, anziché *by design*.

------------
# Instance Optimality
Concludiamo con l'introdurre il concetto di **instance optimality**.

Per cominciare, supponiamo di avere due algoritmi per lo stesso problema, $A$ e $B$.
In quali condizioni possiamo dichiarare che $A$ è un algoritmo *"migliore"* di $B$?

Una prima condizione è che $A$ **domini** $B$, nel senso che $$\text{cost}(A,z) \leq \text{cost}(B,z)$$ per **ogni** possibile input $z$.
Infatti se ci troviamo in questa situazione, non esiste alcun motivo per voler usare $B$. ^a9ed78

Questa [[#^a9ed78|condizione]] da sola però non è ragionevole.
Infatti esistono istanze in cui $A$ è meglio di $B$ e viceversa.
In tal caso (secondo questa condizione da sola) avremo che $A$ e $B$ sono **incomparabili**, e quindi è indifferente usare l'uno anziché l'altro algoritmo.

Pericò possiamo *rilassare* questa [[#^a9ed78|condizione]] è definire 

> **Def. Instance Optimality**
> Un algoritmo $A$ per un dato problema è detto **instance optimal** con **approssimazione** $c \geq 1$ se per ogni ogni altro algoritmo $B$ (per lo stesso problema) e per ogni istanza $z$ avremo che $$\text{cost}(A,z) \leq c \cdot \text{cost}(B,z)$$
> Importante osservare che $c$ è una **costante indipendente** da $B$.

^67367e
