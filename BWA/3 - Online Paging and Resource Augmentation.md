# Online Paging and Resource Augmentation

# Online Paging
L'**Online Paging** (o **[[1 - Introduction#Caching problem LRU vs FIFO|Online Caching]]**) è un comune problema del mondo reale, abbastanza "semplice" però per dimostrare alternative all'analisi *worst-case*.

Richiamando la struttura del problema vista in [[1 - Introduction]], abbiamo:
1. Una memoria di archivio lenta, contenente $N$ **pagine**.
2. Una memoria molto veloce (la **cache**) che può contenere al più $k < N$ pagine nello stesso momento.
3. Le richieste delle pagine arrivano **online**, ovvero consecutivamente nel tempo. Indichiamo con $\sigma$ la sequenza di richieste.
4. Se la pagina $p_t$ richiesta al tempo $t$ è già presente, allora nessun costo aggiuntivo è richiesto.
5. Se invece $p_t$ non è presente nella cache al momento della sua richiesta, occorrerà un **page fault**.
	1. Nel caso in cui c'è spazio nella cache, nessun problema, basterà caricare la pagina $p_t$.
	2. Se invece la cache risulta satura, occorrerà adottare una **politica di sostituzione** de decidere quale pagina rimuovere per fare spazio a $p_t$.

In questo caso, per **efficienza** di un algoritmo (o *running time*) intederemo il numero di **page fault** che occorreranno.
Osserviamo che l'efficienza di una politica di sostituzione non dipende solamente dalla sequenza di richieste $\sigma$, bensì anche dalla dimensione della cache $k$.
Perciò, formalmente, per ogni algoritmo $A$, dimensione della cache $k$ e sequenza $\sigma$ avremo che $$\text{cost}(A, k, \sigma) = \#\text{page faults}$$
```ad-important
title: Ricorda

L'algoritmo **offline** (ovvero che funziona conoscendo a priori l'intera sequenza $\sigma$) **Furthest-in-the-Future** (o **FIF**) minimizza sempre il numero di page fault.

Tale algoritmo rimuove la pagina che verrà richiesta più lontano nel futuro, preservando quelle che verranno richieste nel futuro prossimo.
```

^fe8b62

```ad-important
title: Ricorda

Invece lo standard per gli algoritmi **online** è la politica **LRU** (**Leat-Recently-Used**).
Essendo una politica online, certamente non funziona meglio di FIF, però *empiricamente* funziona molto bene.

Questa evidenza empirica mostra una **principio di località** nei dati reali, ovvero che le pagine vengono richieste in un intorno temporale, anche noto come **working set**.
```


---------------
# Competitive Analysis
Il paradigma dominante per l'analisi degli algoritmi *online* è noto come **competitive analysis**.

> **Def. Competitive Ratio**
> Il **competitive ratio** di un algortimo <u>online</u> $A$ è la sua performance nel caso peggiore diviso le performance dell'algoritmo **ottimo** <u>offline</u> $OPT$.
> $$\max_{z} \frac{\text{cost}(A, z)}{\text{cost}(OPT, z)}$$

^2d11f5

> [!note]
> Il **competetivie** ratio di un algoritmo online è sempre **almeno** $1$.
> 

Nell'**analisi competitiva** degli algoritmi online, interpretiamo *"l'algoritmo $A$ megliore dell'algoritmo $B$"* se e solo se il *competitive ratio* di $A$ è più piccolo di quello di $B$ (perché si avvicina di più alle performance di $OPT$).
Inoltre, possiamo identificare come algoritmo ottimo quello col minor competitive ratio possibile.
Così facendo stiamo cercando di soddisfare il [[1 - Introduction#Goal 2 Identify Optimal Algorithms]].

Possiamo interpretare intuitivamente il competitive ratio come una sorta di [[1 - Introduction#^67367e|instace optimality]]:
se un algoritmo $A$ ha competitive ratio $\alpha$ allora esso sarà un algoritmo **instance optimal** con fattore di ottimalità $\alpha$.
Infatti, per ogni possibile input $w$, avremo che 
$$\frac{\text{cost}(A, w)}{\text{cost}(OPT, w)} \leq \max_{z} \frac{\text{cost}(A, z)}{\text{cost}(OPT, z)} = \alpha \implies \text{cost}(A,w) \leq \alpha \cdot \text{cost}(OPT, w)$$

Inoltre, per ogni altro algoritmo $B$ sappiamo che esso non potrà essere più efficiente dell'algoritmo offline ottimo $OPT$, perciò
$$\text{cost}(OPT,w) \leq \text{cost}(B,w) \implies \text{cast}(A,w) \leq \alpha \cdot \text{cost}(B,w)$$

Ovvero $A$ avrà una efficienze **al più** $\alpha$ volte quella di un qualsiasi altro algoritmo, che sia *offline* oppure *online*.

## A Lower Bound for all Deterministic Algorithms
> **Teorema (Lower Bound for All Paging Algorithms)**
> Ogni algoritmo di pagind **deterministico** ha un [[#^2d11f5|competitive ratio]] di **almeno** $k$.

^f725b1

> **Proof**
> Supponiamo di avere $N = k+1$ pagine, e fissiamo un qualsiasi algoritmo di paging deterministico $A$.
> Dato che dalla *cache* mancherà **sempre** una pagina si può sempre definire una sequenza di richieste $\sigma$ che induce in tutti **page fault** consecutivi.
> 
> Fissata quindi la **sequenza critica** $\sigma$ che induce $A$ ad avere tutti page faults, proviamo a calcolare le prestazioni dell'algoritmo [[#^fe8b62|FIF]].
> Dato che FIF è **offline**, sappiamo a priori come è fatta la sequanza critica $\sigma$.
> Ogni volta che FIF incorre in un page fault, abbiamo $k$ candidati da poter rimuovere, e una di queste non sarà più presente tra le successive $k − 1$ richieste.
> Perciò FIF avrà un page fault una volta dopo **almeno** $k-1$ pagine.
> Ovvero $\text{cost}(FIF, k, \sigma)$ è nel **caso peggiore** $\vert \sigma \vert / k$.
> Avremo quindi che $$\frac{\text{cost}(A, k, \sigma)}{\text{cost}(FIF, k, \sigma)} = \underbrace{\frac{\vert \sigma \vert}{\text{cost}(FIF, k, \sigma)}}_{A\text{'s competitive ratio}} \geq \frac{\vert \sigma \vert}{\vert \sigma \vert/k} = k \;\; \square$$

## An Upper Bound for LRU
Purtroppo il [[#A Lower Bound for all Deterministic Algorithms|lower bound]] potrebbe risultare ridicolmente enorme, dato che in contesti reali $k$ risulta essere molto grande.
Il che non spiegherebbe le ottime prestazioni osservate empiricamente di molti algoritmi online (come *LRU*) quando applicati su **dati reali**.

Tali osservazioni quindi escludono l'**analisi competitiva** dal [[1 - Introduction#Goal 1 Performance Prediction|goal di predire/descriver]] le prestazioni empiriche di un algoritmo.

Infatti il [[#^f725b1|bound]] ci dice che le performance degradano (rispetto all'ottimo <u>offline</u>) **linearmente** al crescere della dimensione della cache.
Ciò suggerirebbe che aumentare la memoria non è una strategia buona da adottare, contrariamente ad ogni buon senso! ^85a713

Però, ripensando alla [[#^2d11f5|definizione di competitive ratio]], non dovremmo sorprenderci di vedere valori molto alti, infatti si basa sul modello **caso peggiore**.

Questo non significa però che è inutile proseguire su questa strada.
Infatti, se $A$ ha un competitive ratio migliore di quello di $B$ (anche se entrambi enormi), rimane sempre verò che $A$ è più competitivo rispetto a $B$. ^2482c0

Perciò possiamo sempre utilizzarlo come [[1 - Introduction#Goal 2 Identify Optimal Algorithms|metodo di paragone]] per la ricerca di algoritmi ottimi.

> **Teorema (Upper Bound for LRU)**
> Il competitive ratio dell'algoritmo di paging online **LRU** è **al più** $k$.

^adc4cd

> **Proof**
> Fissiamo una generica sequenza $\sigma$.
> Per provare l'upperbound, vogliamo **massimizzare il numeratore** $\text{cost}(LRU, k, \sigma)$ e **minimizzare il denominatore** $\text{cost}(OPT, k, \sigma)$.
> 
> Iniziamo col **partizionare** $\sigma$ in sottosequenze $\sigma_1, ..., \sigma_b$.
> Il blocco $\sigma_1$ è (prima) **massima** sottosequenza nella quale sono presente $k$ richieste distinte. Il blocco $\sigma_2$ è la seconda sottosequenza **massima** (subito dopo $\sigma_1$) nella quale sono presenti $k$ richieste distinte, e così via per le altre...
> 
> La prima osservazione è che LRU *fallisce* **al più** $k$ volte per ogni blocco (dato che ci saranno solamente $k$ pagine **distinte**).
> Perciò $$\text{cost}(A,k, \sigma) \leq k \cdot b$$
> ![|500](BWA_03_1.png)
> 
> Consideriamo un nuovo blocco $\hat{\sigma}_1$ compost dal primo blocco $\sigma_1$ più la prima richiesta del secondo blocco $\sigma_2$.
> Dato che $\sigma_1$ è massimale, sicuramente la prima richiesta di $\sigma_2$ sarà una nuova pagina **mai** richiesta in $\sigma_1$ (convincersi di questo).
> Perciò in $\hat{\sigma}_1$ ci saranno $k+1$ richieste distinte, e nessun algoritmo (memmeno l'ottimo offline) può evitare di fare **almeno** un page faul.
> 
> Consideriamo ora $\sigma_2$, e supponiamo che la sua prima richiesta sia $p$.
> Dopo che un algoritmo soddisfa la richiesta $p$, in cache ci saranno altre $k-1$ pagine diverse da $p$.
> Però, per massimalità di $\sigma_2$, avremo che nel resto di $\sigma_2$ ci saranno altre $k-1$ richieste distinte, e **diverse** da $p$.
> Aggiungendo quindi al resto di $\sigma_2$ la prima richiesta di $\sigma_3$ (che sarà ovviamente diversa da $p$ per definizione), avremo creato un nuovo blocco $\hat{\sigma}_2$ con $k$ richieste **diverse** da $p$.
> Però visto che in cache ci stava $p$ all'inizio di $\hat{\sigma}_2$, per soddisfare tutte le $k$ richieste diverse da $p$ prima o poi ci sarà un **page fault**.
> ![|500](BWA_03_2.png)
> 
> Applicando questo questo ragionamento per tutti i $b$ blocchi, avremo che qualsiasi algoritmo (e quindi anche quello ottimo) incorrerà in **almeno** un page fault per blocco.
> Quindi $$\text{cost}(OPT,k,\sigma) \geq b$$
> 
> In conclusione $$\max_{k,\sigma}\frac{\text{cost}(LRU,k,\sigma)}{\text{cost}(OPT,k,\sigma)} \leq \frac{kb}{b} = k \;\; \square$$

^e1e690


## An Upper Bound for Flush-When-Full
I teoremi precedenti ([[#^f725b1|lowerbound]] e [[#^adc4cd|upperbound]]) mostrano che l'algoritmo LRU ha il [[#^2d11f5|competitive ratio]] più piccolo possibile, e questo è un punto a favore della **ompetitive analysis**.

Consideriamo ora un nuovo algoritmo di paging offline, il **Flush-When-Full** (o **FWF**).

> **Algoritmo Flush-When-Full**
> Quando la cache è piena, e occore un page fault, l'algoritmo **FWF** svuota l'intera cache.

^cd1d79

```ad-note
title: Osserva
Ricordando la notazione della [[#^e1e690|dimostrazione]] del [[#^f725b1|teorema per il lowerbound]], avremo che i **flush** della cache nell'algoritmo FWF corrispondono esattamente alla fine dei blocchi $\sigma_1, ..., \sigma_{b-1}$.
```

Dato che FWF fa un *flush* della dell'intera memoria, ogni $k$ fault, avremo quindi che il numero comlessivo di fault dell'algoritmo è **esattamente** $kb$.
Perciò anche il **competitive ratio** di FWF è $k$, come LRU.

Purtroppo per FWF è *"ovviamente"* peggiore di LRU, nel senso che FWF non subisce mai meno page fault di LRU (perché LRU fa $\leq k$ fault per blocco, mentre FWF $=k$ fault), e sicuramente LRU è strettamente migliore su una gra parte degli input.

--------
```ad-summary
title: Recap
Come abbiamo visto fin ora, l'**analisi competitiva** sembra non promettere molto bene rispetto ai [[1 - Introduction#Goals of Analyzing Algorithms|goals dell'analisi degli algoritmi]].

Per quanto riguarda il [[1 - Introduction#Goal 1 Performance Prediction|goal esplicativo]], abbiamo [[#^85a713|già discusso]] di come non bisogna prendere eccisvamente alla lettera l'analisi competitiva per predire le prestazioni di un algoritmo (infatti il competitive ratio cresce al crescere della cache).

Un po' meglio rispeto al [[1 - Introduction#Goal 2 Identify Optimal Algorithms|goal comparativo]], abbiamo [[#^2482c0|visto]] che può essere sempre usato come strumento di comparazione tra algoritmi, anche se certe volte non va propriamente bene (come nel caso del [[#An Upper Bound for Flush-When-Full|FIF]]).

Nel resto delle note vedremo altre tecniche che migliorano la qualità degli obiettivi [[1 - Introduction#Goal 1 Performance Prediction|1]] e [[1 - Introduction#Goal 2 Identify Optimal Algorithms|2]].
```

# Resource Augmentation
L'idea della tecnica della **resource augmentation** è quella di comparare l'algoritmo interessato (nel nostro caso LRU) con **extra risorse** con un algoritmo migliore (nel nostro caso FIF) il quale con **risorse ridotte**.
Naturalmente, l'indebolimento delle capacità dell'algoritmo ottimale *offline* può solo portare a [[#^2d11f5|competitive ratio]] migliori.

Lasciamo a dopo l'[[#Interpretazione|interpretazione]] del perché di questo approccio.

> **Teorema (Resource Augmentation Bound for LRU)**
> Il [[#^2d11f5|competitive ratio]] dell'algoritmo online LRU con memoria $k$ è al più $$\frac{k}{k-h+1}$$ rispetto all'algoritmo ottimale con memoria di dimensione $h \leq k$.
> Più formalmente $$\max_{\sigma} \frac{\text{cost}(LRU, k, \sigma)}{\text{cost}(OPT, h, \sigma)} \leq \frac{k}{k-h+1}$$

^b37ec7

> **Proof**
> Molto simile alla [[#^e1e690|dimostrazione per l'upperbound dell'LRU]].
> Partizioniamo sempre $\sigma$ in $\sigma_1, ..., \sigma_b$ in modo tale che in ogni sottosequenza appaiano solamente $k$ richieste diefferenti, e in modo tale che la prima richiesta di ogni blocco sia la $k+1$-esima richiesta differente rispetto al blocco precedente (vedi altra dimostrazione).
> 
> Ricordiamo che LRU incorrerà in **al più** $k$ page fault per ognuno dei $b$ blocchi.
> 
> Ciò che cambia rispetto all'altra dimostrazione è il numero di confronti che $OPT$ farà **almeno** per ogni blocco.
> Come prima, consideriamo la nuova partizione con i nuovi blocchi **traslati**.
> 
> Il primo blocco traslto $\hat{\sigma}_1$ comprende tutto $\sigma_1$ più la prima richiesta di $\sigma_2$.
> Dato che in questa sequenza ci sono $k+1$ pagine differenti e la memoria è solo $h$, avremo che $OPT$ farà almeno $(k+1) - h$ page fault.
> $$\underbrace{k+1}_{\text{pagine differenti}} - \underbrace{h}_{\text{pagine in memoria}}$$
> 
> Il secondo blocco traslato $\hat{\sigma}_2$ sarà composto dal restante $\sigma_2$ (della seconda richiesta in poi), più la prima richiesta di $\sigma_3$.
> Sia $p$ la pagina richista come prima richiesta di $\sigma_2$.
> All'inizio di $\hat{\sigma}_2$ avremo quindi $p$ in cache, più altre $h-1$ pagine differenti.
> Nel restante $\hat{\sigma}_2$ troveremo altre $k$ richieste distinte e diverse da $p$.
> Pericò occorreranno sicuramente almeno altri $k - (h-1)$ page fault.
>  $$\underbrace{k}_{\text{pagine differenti da }p} - \underbrace{(h-1)}_{\text{pagine in memoria differenti da }p}$$
> 
> In conclusione avremo che $$\frac{\text{cost}(LRU, k, \sigma)}{\text{cost}(OPT, h, \sigma)} \leq \frac{k}{k-h+1} \;\; \square$$

-------
# Interpretazione
Quello che viene naturale chiedersi è:

```ad-question
title: Questions
- Cosa garantisce il [[#^b37ec7|precedente teorema]]?
- A cosa serve forzare *artificialmente* l'algoritmo ottimo ad avere prestazioni peggiori?
```

## Interpretazione 1: approccio alla progettazione i sistemi
Una risposta a tale domanda può essere motivata dal fatto che si può sfruttare la [[#Resource Augmentation|resource augmentation]] per avere consigli su quante risorse usare in un contesto reale.

Infatti, per prima cosa possiamo **stimare** le risorse minime necessarie per avere buone performance tramite gli algoritmi ottimi **offline**, per poi **scalare** il valore di $k$ stimato per predire quali saranno le prestazioni di un algoritmo **online**, come LRU.

Per esempio, se ho dei **campioni** di istanza tipici nel mio dominio applicativo, potrei darle in pasto all'algoritmo [[#^fe8b62|FIF]] e vedere per quale valore della size della cache $k$ si hanno buone prestazioni.
Se per esempio stimiamo che $k=10$ è una buona dimensione di cache per FIF, potremmo decidere di adottare nel nostro sistema LRU come politica di paging con cache di grandezza $3k$.

In tal caso possiamo stimare che un upper-bound per al competitive ratio è approssimativamente (perché abbiamo stimato la cache ottima per FIF) $\approx 3/2$.

## Interpretazione 2: Algoritmi online poco competitivi
Mentre la [[#Interpretazione 1 approccio alla progettazione i sistemi|prima interpretazione]] era intuitiva/empirica, questa è più formale e matematica.

C'è un teorema che **informalmente** afferma che

> **Teorema (informale)**
> Per ogni sequenza di richieste $\sigma$, allora LRU è ha "**prestazioni dimostrabilmente eccellenti**" per la sequenza $\sigma$ e per la "**maggior parte**" delle dimensioni della cache $k$.

Un primo *"informalismo"* di questo teorema è il fatto che si può dimostrare che LRU è buono sulla "**maggior parte**" dei valori di $k$, il che è ragionevole assumere che aumentando $k$ le prestazioni migliorino.

Il secondo *"informalismo"* riguarda il concetto di "**prestazioni dimostrabilmente eccellenti**", il che può voler significare due cose:
1. prestazioni eccellenti in confronto all'algoritmo offline ottimo (come nell'[[#Competitive Analysis]]).
2. prestazioni eccellenti nel senso di una piccola percentuale di page fault.

```ad-important
title: Osserva
Non necessariamente il secondo punto implica il primo (perché?)
```

L'idea intuitiva dietro è che le prestazioni dell'LRU rimangono quasi le stesse (al massimo raddoppiano) se si ha disposizione una cache grande $k$, oppure una cache più grande $2k$.

Nel caso in cui si ha disposizione $k$ allora i teoremi [[#^f725b1|lower bound]] e [[#^adc4cd|upper bound]] ci dicono semplicemente che LRU ha un buon competitive ratio.

Mentre nel caso in cui si ha un supplemento di memoria, per esempio $2k$, le prestazioni rapidamente migliorano al crescere del supplemento.
Osserviamo però che da un certo supplemento di memoria in poi, le prestazioni non migliora ulteriormente (su dati reali), perciò LRU può migliorare per un numero fissato di dimensioni della cache.

Più formalmente, sia una sequenza $\sigma$ e un supplemento di pagine extra $b \geq 1$.
Il [[#^b37ec7|teorema]] ci garantisce che
$$\text{cost}(LRU, k+b, \sigma) \leq \frac{k+b}{b+1}\text{cost}(OPT, k, \sigma)$$ ^39d212

Ora possiamo suddividere i valori di $k$ in due categorie.
1. i valori di $k$ **critici**, ovvero quei $k$ tali che aggiungendo un qualsiasi supplemento alla cache (anche una sola pagina, $b=1$) le prestazioni **migliorano drasticamente**, per esempio i page fault almeno dimezzano $$\text{cost}(LRU, k+b, \sigma) < \frac{1}{2}\text{cost}(LRU, k, \sigma)$$ ^c56a00
2. tutti quegli altri $k$ per i quali vale invece l'[[#^39d212|upper bound]], ovvero tali che $$\text{cost}(LRU, k+b, \sigma) \leq \frac{k+b}{b+1}\text{cost}(OPT, k, \sigma) \leq \frac{k+b}{b+1}\text{cost}(LRU, k, \sigma)$$

Prendiamo in analisi il primo caso, quello più drastico.

Supponiamo che il nostro $\sigma$ è tale che esistono dei valori di $k$ per i quali vale il [[#^c56a00|primo caso]].
Consideriamo tutti i valori **critici** per i quali, al fronte di un supplemento di $b$ pagine, le prestazioni "*migliorano drasticamente*" (almeno raddoppiano).

Fissiamo un generico $k$, e supponiamo che tra 1 e $k$ ci sono **almeno** $\ell \leq k$ valori **critici**.
Allora togliendo a $k$ il supplemento $b$, possiamo trovare **almeno** $\ell / b$ altri valori critici della dimensione della cache, per i quali vale quindi la [[#^c56a00|disuguaglianza 1]].
Perciò
$$\text{cost}(LRU, k, \sigma) < \frac{1}{2}\text{cost}(LRU, k-b, \sigma) < \frac{1}{4}\text{cost}(LRU, k-2b, \sigma) < ... < 2^{-\ell/b} \cdot \text{cost}(LRU, 1, \sigma)$$
Perciò, se abbiamo che $$\ell \geq b \cdot \log_2\frac{1}{\varepsilon}$$ allora $$\text{cost}(LRU, k, \sigma) < \varepsilon \cdot \vert \sigma \vert$$
(Osserva che $\text{cost}(LRU, 1, \sigma) = \vert \sigma \vert$).

> In conclusione se $\varepsilon$ è sufficientemente piccolo le prestazioni di LRU sono **eccellenti in senso assoluto**, e possiamo anche dimenticarci del suo rapporto competitivo.

