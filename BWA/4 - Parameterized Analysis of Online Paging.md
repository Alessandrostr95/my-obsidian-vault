# Parameterized Analysis of Online Paging
Ricordiamo da [[3 - Online Paging and Resource Augmentation]] che l'[[3 - Online Paging and Resource Augmentation#Competitive Analysis|analisi competitiva]] non risulta essere molto utile per soddisfare gli [[BWA/1 - Introduction#Goals of Analyzing Algorithms|obiettivi desiderati]].

Infatti per quanto riguarda il [[BWA/1 - Introduction#Goal 1 Performance Prediction|Goal 1]], abbiamo visto che il [[3 - Online Paging and Resource Augmentation#^2d11f5|competitive ratio]] non è una buona unità di misura delle prestazioni di un algoritmo online di paging, in quanto assumono valori [[3 - Online Paging and Resource Augmentation#^f725b1|troppo grandi]] per essere prese sul serio.

Per il [[BWA/1 - Introduction#Goal 2 Identify Optimal Algorithms|Goal 2]] abbiamo invece visto che l'analisi competitiva identifica [[3 - Online Paging and Resource Augmentation#^adc4cd|LRU come ottimo]], però identifica come ottimo anche algoritmi intuitivamente (e <u>empiricamente</u>) peggiori (come [[3 - Online Paging and Resource Augmentation#^cd1d79|FWF]]).

In seguito abbiamo introdotto la tecnica della [[3 - Online Paging and Resource Augmentation#Resource Augmentation|Resource Augmentation]].
Abbiamo visto che ci consentiva i fare un passo avanti rispeto al [[BWA/1 - Introduction#Goal 1 Performance Prediction|Goal 1]], infatti spiega come crescono le prestazioni (in termini di [[3 - Online Paging and Resource Augmentation#^2d11f5|competitive ratio]]) di un algoritmo online al crescere dell'**aumento di risorse** (ovvero la dimensione della *cache*).

Purtroppo, riguardo il [[BWA/1 - Introduction#Goal 2 Identify Optimal Algorithms|Goal 2]], la *resource augmentation* non ci aiuta, in quanto non distingue LRU, da FIFO da FWF.

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

^c11a0f

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
Ovvero abbiamo dimostrato che LRU è ottimale su ogni $f$ e $k$ (validi), mentre FIFO no, soddisfando quindi anche il [[BWA/1 - Introduction#Goal 2 Identify Optimal Algorithms|Goal 2]] (*finalmente*).

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

Questo ci consente di applicare il teorema in maniera più generale su qualsiasi sequenza.
```

Definiamo ora il **parametro** $\alpha_f(k)$
$$\alpha_f(k) = \frac{k-1}{f^{-1}(k+1) - 2}$$
dove $f^{-1}(m)$ è il **minimo** valore di $n$ per il quale $f(n) = m$.
Per esempio, considerando l'[[#^88c70f|immagine]] avremo che
- $f^{-1}(1) = 1$
- $f^{-1}(2) = 2$
- $f^{-1}(3) = 3$
- $f^{-1}(4) = 5$
- $f^{-1}(5) = 8$

```ad-important
title: Important !!
Un modo intuitivo per interpretare $f^{-1}(m)$ è come un **lower bound** alla dimensione delle finestre che contengono **almeno** $m$ pagine distinte.

$$f^{-1}(m) := \min{\{n \in \mathbb{N} \vert f(n) \geq m\}}$$

Per esempio:
- se $f(n) = n \implies f^{-1}(m) = m$ allora $\alpha_f(k) = 1$.
- se $f(n) \approx \sqrt{n} \implies f^{-1}(m) \approx m^2$ allora $\alpha_f(k) \approx 1/k$. Questo è buon un bound al page fault rate, anche su dimensioni della cache non troppo grandi.
- se $f(n) \approx 1 + \log_2{n} \implies f^{-1}(m) \approx 2^m$ allora $\alpha_f(k) \approx k/2^k$. Questo invece è un bound molto piccolo praticamente per qualsiasi valore di $k$.
```

^e11595

```ad-important
title: More important !!

Sia $H = m_1 + m_2 + ... + m_h$ la somma dei primi $m_j$. 
Osservando la [[#^88c70f|tabella]] dei valori di un $f$ concavo, possiamo constatare che $H$ corrisponde all'**ultimo** indice in cui troviamo come valore $h$.
O analogamente abbiamo che
$$H = f^{-1}(H+1) - 1$$
```

^249095


## Proof
Prima di procedere con la dimostrazione è necessario assumere che $f(2) = 2$ (e che quindi $m_1 = 1$).
Infatti se $f(2) = 1$ abbiamo che l'[[#^c11a0f|unica sequenza conforme è quella costante]], ed **ogni** algoritmo di paging avrà page fault rate 0. ^3cc86c

### Proof part 1
Fissiamo una funzione concava $f$, un dimensione della cache $k \geq 2$ e un algoritmo deterministico online $A$.

Assumiamo che il numero di pagine $N$ è esattamente $k+1$, così che ad ogni istante c'è un'**unica** pagina mancante dalla cache (piena).
Con $N \leq k$ avremo che lo 0% di page fault perché possiamo caricare direttamente tutte le pagine in memoria.

Dato che lo [[#^be9407|statement 1]] riguarda il **caso peggiore**, definiamo quindi una famiglia di sequenze $\sigma$ che rispettano il bound desiderato (se il lowerbound è rispettato per le sequenze di questa famigli allora lo sarà anche per le *"richieste peggiori"*).

Suddividiamo ogni sequenza $\sigma$ in un numero arbitrario di $\ell$ **fasi** $\sigma^{(1)}, ..., \sigma^{(\ell)}$, tali che ogni fase $i$ è composto da esattamente $k-1$ **blocchi** $\sigma^{(i)}_1, ...,\sigma^{(i)}_{k-1}$.

Per ogni generica fase $i$, il blocco $j$-esimo $\sigma^{(i)}_j$ sarà costituito da $m_{j+1}$ richieste consecutive di una stessa pagina $p_j$, dove $p_j$ è l'**unica** pagina mancante dalla memoria (per via dell'algoritmo $A$) all'inizio del blocco $\sigma^{(i)}_j$.
Perciò $A$ soffre di esattamente $k-1$ *page faults* per fase (uno per blocco).

Per rendere le idee, se $p_1$ è la prima pagina mancate dalla cache all'inizio di una fase $i$, allora il suo primo blocco sarà lungo $m_2$.
Il secondo blocco sarà composto da $m_3$ richieste alla pagina $p_2$ mancante dalla memoria, e così via...

```ad-example
title: Esempio
Se abbiamo $m_2 = 2, m_3 = 3, m_4 = 3$ allora i primi 3 blocchi di una fase sono

$$\underbrace{p_1p_1}_{\sigma^{(i)}_1} \;\; \underbrace{p_2p_2p_2}_{\sigma^{(i)}_2} \;\; \underbrace{p_3p_3p_3}_{\sigma^{(i)}_3} \;\; ...$$
```

Perciò ogni fase avrà lunghezza $$\vert \sigma^{(i)} \vert = m_2 + m_3 + ... + m_k = \left( \sum_{i=1}^{k}m_i \right) - 1$$(meno 1 perché ricordiamo che $m_1 = 1$).

Ricordando la [[#^249095|proprietà]] della somma dei primi $m_j$ valori, avremo quindi che
$$\vert \sigma^{(i)} \vert = (f^{-1}(k+1)-1) -1 = f^{-1}(k+1) -2$$

Se riusciamo ora a dimostrare che la sequenza $\sigma$ costruita è conforme ad $f$ abbiamo dimostrato il bound $$\text{worst-case page fault ratio} \geq \frac{\text{\# page fault}}{\vert \sigma \vert} = \frac{\ell(k-1)}{\sum_{i=1}^{\ell} \vert \sigma^{(i)}\vert} = \frac{k-1}{f^{-1}(k+1)-2} = \alpha_f(k)$$

Per $j=1,2$ abbiamo per [[#^3cc86c|definizione]] che $f^{-1}(j)=j$, quindi $m_1 = m_2 = 1$ (ovvero $m_1 \leq m_2$).
Consideriamo quindi $j=3, ..., k$.
Osserviamo che per come sono definiti i blocchi, ogni sottosequenza di $j$ **pagine distinte** deve ricoprire (almeno **parzialmente**) $j$ blocchi consecutivi.

![](BWA_04_3.png)

Quindi un $j$ **minimale** che contiene almeno $j$ pagine distinte (vedi [[#^e11595|definizione]] $f^{-1}$) è composto dall'**ultima richiesta** del primo blocco parzialemente ricoperto e dalla **prima richiesta** dell'ultimo blocco parzialmente ricoperto.

![](BWA_04_4.png)

Infatti estendendo ulteriormente la sottosequenza verde nel primo o nell'ultimo blocco, non otteniamo pagine aggiuntive ma aumentiamo solamente la lunghezza della sottosequenza.

Per via della **monotonia** della lunghezza dei blocchi ($m_1  \leq m_2 \leq ...$) tale sequenza è **effettivamente minimale** quando inizia dall'**ultima richiesta di una fase**, poi comprende i primi $j-2$ blocchi e poi tocca la prima richiesta del blocco $j-1$.

Perciò la lunghezza di tale sottosequenza minimale sarà $$1 + m_{1} + m_{2} + ... + m_{j-2} + 1 = \left( \sum_{i=1}^{j-2} m_{i}\right) + 2 = f^{-1}(j-1)+1 \leq f^{-1}(j)$$
Ovvero data una finestra con $j$ pagine distinte, essa sarà lugna al più $f^{-1}(j)$.
Oppure viceversa, data una finestra lugna $n$, in essa ci saranno al più $f(n)$ pagine distinte.

Perciò ogni fase $\sigma^{(i)}$ è **conforme** ad $f$ $\square$.

### Proof part 2
Fissiamo un $f$ e un $k$ e consideriamo una qualsiasi sequenza $\sigma$ **conforme** ad $f$.

Quello che vogliamo è cercare di **partizionare** la sequenza $\sigma$ in blocchi di lunghezza **almeno** $f^{-1}(k + 1) − 2$ (*minimizzare il denominatore*) tale che ogni blocco abbia **al più** $k − 1$ errori (*massimizzare il numeratore*).

Suddividiamo $\sigma$ in blocchi tali che LRU faccia esattamente $k-1$ page fault.
Tali blocchi devono essere **massimali**, ovvero iniziano col primo page fault e terminano alla richiesta precedente al $k$-esimo page fault.

![|500](BWA_04_5.png) ^105512

> **Claim**
> Consideriamo un generico blocco $X$, diverso dal primo e l'ultimo.
> Estendiamo $X$ con la richiesta precedente e quella immediatamente successiva.
> Tale blocco esteso comprenderà **esattamente** $k+1$ **pagine distinte**.
> 
> Infatti, dato che il blocco $X$ inizia con un *page fault*, abbiamo che la richiesta precedente è differente dalla prima in $X$.
> 
> Analogamente, dato che nella memoria ci sono esattamente $k$ differenti pagine, la prima richiesta del blocco successivo deve necessariamente essere differente dalle precedenti inserite in cache durante $X$. 

^d32a87

Dato che quindi il *[[#^d32a87|blocco esteso]]* ha $k+1$ pagine distinte, allora esso avrà lughezza **almeno** $f^{-1}(k+1)$.

Togliendo i due estremi avremo che ogni blocco ha come dimensione **almeno** $f^{-1}(k+1)-2$.

Perciò il page fault rate di ogni blocco sarà **al più** $\leq \frac{k-1}{f^{-1}(k+1)-2}$ $\square$.

### Proof part 3
Costruiamo appositamente un $f$ concava e una sequenza $\sigma$ che induca FIFO ad avere prestazioni beggiori di LRU.

La funzione $f$ è la seguente
![|600](BWA_04_6.png)

Poniamo $k = 4$ e supponiamo di avere $N=5$ pagine $\{0,1,2,3,4\}$.
Dato che ci sono 5 pagine e la cache è grande 4, possiamo assumere che $f(n) = 5$ per ogni $n \geq 7$, e che quindi $f^{-1}(m) = 7$ per ogni $m \geq 5$.

Pericò questa funzione è **convessa** e che $$\alpha_f(k) = \frac{4-1}{f^{-1}(4+1) - 2} = \frac{3}{5}$$

Il punto [[#^e14157|(2)]] del teorema dimostra che LRU avrà un page fault rate di **al più** $3/5 = 60\%$ su qualsiasi sequenza $\sigma$.

Mostriamo ora una sequenza per la quale FIFO ha un page fault rate **strettamente maggiore** del $60\%$.

La sequenza in questione è composta da un numero arbitrariamente grade di blocchi identici che si ripetono, della forma:
$$\sigma = \langle 10203040 \;\; 10203040 \;\; ... \;\; 10203040 \rangle$$

L'idea intuitiva del perché questa sequenza è critica per FIFO e buona per LRU è che LRU non rimuoverà mai la pagina $0$ dalla cache dato che si presenta ogni due richieste, mentre FIFO rimuoverà la pagina $0$ ogni due richieste.

Per vedere perché FIFO effettivamente ha un page fault ratio strettamente maggiore di $\alpha_f(k) = 0.6$ bisogna tracciarne il comportamento per almeno un paio di blocchi (dopodiché si ripete).

Dato che l'algoritmo inizia con la cache vuota, i primi page fault prima che si riempi tutta (ovvero tutte le 4 pagine) saranno in ordine 1, 0, 2 e 3.
Dopodiché avverrà una page fault sulla pagina 4, facendo così rimuovere dalla cache la pagina 1.
Con un totale di 5 page fault su 8 richieste.

Nel secondo blocco verrà richiesta la pagina 1, e quindi rimossa la pagina 0.
Dopodiché verra richiesta la pagina 0 e rimossa la 2.
Poi verrà richiesta la pagina 0 nuovamente, e nessun page fault occorre.
Poi verrà richiesta la pagina 2 e rimossa la 3.
L'altra page fault accadrà quando verrà richiesta la 3, rimuovendo la pagina 4, e in fine l'ultimo quando verrà richiesta 4 rimuovendo la pagina 1.

![](BWA_04_7.png)

Quindi ciclicamente avremo sempre 5 page fault ogni 8, con un page fault rate di
$$\frac{5}{8} = 62.5 \% > 60\% = \alpha_f(k)$$
dimostrando l'ultima parte del teorema $\square$.

```ad-note
Questa dimostrazione mostra che FIFO è avvolte strettamente peggiore di LRU, ma non di molto ($62.5\%$ vs $60\%$).
Infatti empiricamente abbiamo che FIFO è peggiore di LRU ma non di molto.
```
