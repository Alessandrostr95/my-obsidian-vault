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

Tali osservazioni quindi escludono l'**analisi competitiva** dal [[1 - Introduction#Goal 1 Performance Prediction|gola di predire/descriver]] le prestazioni empiriche di un algoritmo.

Infatti il [[#^f725b1|bound]] ci dice che le performance degradano (rispetto all'ottimo <u>offline</u>) **linearmente** al crescere della dimensione della cache.
Ciò suggerirebbe che aumentare la memoria non è una strategia buona da adottare, contrariamente ad ogni buon senso!

Però, ripensando alla [[#^2d11f5|definizione di competitive ratio]], non dovremmo sorprenderci di vedere valori molto alti, infatti si basa sul modello **caso peggiore**.

Questo non significa però che è inutile proseguire su questa strada.
Infatti, se $A$ ha un competitive ratio migliore di quello di $B$ (anche se entrambi enormi), rimane sempre verò che $A$ è più competitivo rispetto a $B$.

Perciò possiamo sempre utilizzarlo come [[1 - Introduction#Goal 2 Identify Optimal Algorithms|metodo di paragone]] per la ricerca di algoritmi ottimi.

> **Teorema (Upper Bound for LRU)**
> Il competitive ratio dell'algoritmo di paging online **LRU** è **al più** $k$.

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
> In conclusione $$\max_{k,\sigma}\frac{\text{cost}(LRU,k,\sigma)}{\text{cost}(OPT,k,\sigma)} \geq \frac{kb}{b} = k \;\; \square$$


