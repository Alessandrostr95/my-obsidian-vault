# Metodi euristici per partizionare in comunità
Nella precedente [[7 - Communities - Part 1|parte]] sono stati introdotti i concetti
[[7 - Communities - Part 1#Cut-Communities|cut-community]] e [[7 - Communities - Part 1#Web-Communities|web-community]], e dimostrato che il problema del partizionamento di un grafo in web-communities è un problema **difficile**[^1] dal punto di vista computazionale.

In questa parte verranno mostrate due famiglie approcci che dal punto di vista euristico riescono a partizionare un grafo in comunità accettabili, ovvero in gruppi di nodi abbastanza coesi tra di loro.

Più precisamente i due metodi si dividono in:
- **metodi partitivi**, in cui si parte considerando l'intero grafo come unica grande comunità, e la si inizia a disconnettere (rimuovendo archi) finché non si otterranno due comunità distinte e coese. Se si desidera ottenere una **granularità** maggiore basta iterare il metodo sui sottografi ottenuti.
- **metodi agglomerativi**, in cui si parte considerando ogni singolo nodo una comunità, e man mano si aggiungono gli archi del grafo finché non si otterranno un numero di comunità desiderato con un livello di coesione accettabile.

Osservare che entrambi i metodi consentono di ottenere delle **partizioni nidificate** (**nested**), infatti nel metodo partitivo si genera una partizione a partire da una più grande (approccio **top-down**), e nel metodo agglomerativo si genera una partizione come composizione di altre più piccole (approccio **bottom-up**).

![Schema di partizione metodo partitivo.|350](ar-lesson08-img1.png)


![Schema di partizione metodo agglomerativo.|350](ar-lesson08-img2.png)
Entrambi i metodi hanno quindi uno *schema di partizione* ad albero.

Osservare infine che entrambi i metodi richiedono la scelta di un arco da rimuovere o aggiungere ad ogni passo.

-----------------------------
## Edge-Betweenness
Il concetto di **betweennes** di un arco è sfruttato come criterio di rimozione degli archi nei metodi partitivi di partizionamento precedentemente accennati.
Tale concetto a sua volta si basa sulle proprietà di un arco di essere *bridge* o *local bridge*.

Sappiamo che per definizione, rimuovere un arco bridge disconnette due porzioni di una rete, mentre rimuovere un local bridge certamente peggiora la connettività tra due comunità.

Sappiamo anche che gli archi bridge e local brdige sono dei *weak ties*.
Perciò, rimuovendo tutti i weak ties da una rete rimangono solamente gruppi di nodi connessi da relazioni forti (*strong ties*), e tali gruppi rispecchiano il concetto intuitivo di comunità (ovvero gruppi di persone collegati da legami forti).

Purtroppo però esistono casi in cui anche rimuovendo tutti i weak ties non avviene un partizionamento in comunità.

![Controesempio: in blu i weak ties e in rosso gli strong ties.|350](ar-lesson08-img3.png)

Infatti, considerando il precedente controesempio, pur rimuovendo l'arco rosso non avremo un partizionamento.
Invece è evidente che c'è una comunità composta da una clique a destra e una composta da una clique a sinistra.
Per quanto riguarda il nodo centrale esso può essere inserito indistintamente i una delle due comunità.

Perciò come criterio potremmo considerare la quantità di **traffico** (o **flusso**) di informazioni che passa attraverso gli archi.

Possiamo considerare come "nuovi archi ponte" tutti quegli archi attraverso i quali passa una grande quantità di flusso, e che se rimossi rendono più difficile la comunicazione tra due comunità.

Secondo questo criterio possiamo rimuovere gli archi sui quali passa maggior flusso, finché non otterremo un partizionamento accettabile della rete.

Più formalmente, considerando un grafo <u>non diretto</u> $G = (V,E)$, definiamo con $\sigma_{st}(u,v)$ il numero di [shortest-paths](https://en.wikipedia.org/wiki/Shortest_path_problem) $\pi^\star(s,t)$ (o *camminimi minimi*) tra $s$ e $t$ che passano per l'arco $(u,v)$.

Definiamo poi con $b_{st}(u,v)$ la **betweennes dell'arco** $(u,v)$ **rispetto alla coppia** $s,t$ come la **frazione** di shortest path $\pi^\star(s,t)$ che passano per l'arco $(u,v)$ $$
\begin{align*}
\sigma_{st} &= \vert \lbrace \pi^\star(s,t)  \rbrace \vert\\
b_{st}(u,v) &= \frac{\sigma_{st}(u,v)}{\sigma_{st}}
\end{align*}
$$Definiamo infine la **betweenness** $b(u,v)$ di un arco $(u,v)$ come la *semi-somma* di tutte le betweenness relative $b_{st}(u,v)$, per ogni coppia di nodi $s,t$
$$b(u,v) = \frac{1}{2} \sum_{(s,t) \in \binom{V}{2}} b_{st}(u,v)$$
Notare che il fattore $1/2$ è necessario per evitare di contare le ripetizioni, del tipo $b_{st}(u,v)$ e $b_{ts}(u,v)$.

Analogamente alla **edge-betweenness** si può definire una **node-betweenness**, seguendo la stessa definizione.

## Il metodo di Girvan-Newman
Il metodo di *Girvan-Newman* è un metodo partitivo per il partizionamento, basato sul concetto di *edge-betweenness*, abbastanza semplice.
I passaggi sono i seguenti:
1.  Si calcola l'arco $(u,v)$ con betweenness $b(u,v)$ **massima**, e lo si rimuove.
2. Se il grafo residuo risulta partizionato con in una granularità desiderata allora abbiamo concluso.
3. Se così non fosse si ricalcola il nuovo arco con betweenness massima del nuovo grafo e si itera finché non si ottiene il risultato desiderato.

L'algoritmo di suo è abbastanza semplice, l'unico problema è il calcolo delle betweenness degli archi.

Purtroppo non si può applicare un approccio brute force calcolando tutti gli shortest path altrimenti la complessità risulterebbe esponenziale.

<u>Serve quindi un approccio più efficiente.</u>

## Algoritmo per il calcolo della betweenness
Per ogni nodo $s \in V$ l'algoritmo del calcolo della betweenness degli archi si suddivide in tre fasi:
1. Calcolare il sottografo $T(s)$ composto dall'*unione* degli alberi di camminimi minimi radicati in $s$. Anche se il numero di tali alberi può essere esponenziale, in realtà il calcolo di $T(s)$ può essere effettuato in tempo polinomiale con una visita in ampiezza `BFS` con le dovute modifiche. ^8a6088
2. Mediante una visita `top-down` calcolo i valori $\sigma_{sv}$ per ogni $v \in V \setminus \lbrace s \rbrace$, e questo vedremo si può fare in tempo polinomiale. ^853300
3. Infine mediante una visita `bottom-up`, e grazie a quanto calcolato nel punto 2, calcolo per ogni arco $(u,v) \in T(s)$ tutte le betweennes relative a shortest paths che partono da $s$, ovvero il valore $$b_s(u,v) = \sum_{t \in V \setminus \lbrace s \rbrace} b_{st}(u,v) \;\; \forall (u,v) \in T(s)$$ ^f5c61f


Per concludere, una volta eseguiti i punti [[#^8a6088|(1)]], [[#^853300|(2)]] e [[#^f5c61f|(3)]]  per ogni nodo $s \in V$, possiamo ricavare i valori delle betweenness come segue
$$b(u,v) = \frac{1}{2} \sum_{s \in V} b_s(u,v)$$
Osserviamo che nel punto `3` calcoliamo $b_s(u,v)$ solo per gli archi di $T(s)$, in quanto se $(u,v)$ non appartiene al sottografo degli shortest path $T(s)$ allora vuol dire che non appartiene nessun shortest path, e quindi non avrebbe senso calcolarlo (per definizione di betweenness).

Andiamo ora a vedere un esempio pratico che mostra anche come eseguire i tre passi dell\'algoritmo del calcolo della betweenness.

### Fase 1
Per calcolare $T(s)$ basta effettuare una visita in ampiezza `BFS` opportunamente modificata.

Definiamo con $L_h$ l'insieme di tutti i nodi a distanza $h$ dalla sorgente $s$.
Diremo che un nodo $v$ si trova al **livello** $h$ se $v \in L_h$.

Se un nodo $v$ si trova al livello $h$, allora certamente **tutti** i cammini minimi da $s$ a $v$ termineranno con archi che partono da $L_{h-1}$.
Perciò, tutti gli archi $(u,v)$ con $u \in L_{h-1}$ e $v \in L_h$ faranno parte di $T(s)$.

Perciò possiamo calcolare $T(s)$ nella seguente maniera:
1. Poniamo inizialmente $L_0 = \lbrace s \rbrace$ e $T(s) = \emptyset$.
2. Per ogni $h \geq 0$ calcoliamo $L_{h+1}$ come tutti quei nodi **fuori** $L_0 \cup L_1 \cup ... \cup L_h$ tali che hanno almeno un vicino in $v \in L_h$, ovvero come $$ L_{h+1} \equiv \lbrace v \in V \setminus (L_0 \cup L_1 \cup ... \cup L_h) | \exists u \in L_h : (u,v) \in E  \rbrace$$
3. Calcolato $L_{h+1}$ poniamo $T(s)$ come $$T(s) = T(s) \cup \lbrace (u,v) \in E | u \in L_h \land v \in L_{h+1} \rbrace$$
![Costruzione sottografo dei cammini minimi.|400](ar-lesson08-img4.png)

### Fase 2
Calcolato $T(s)$, possiamo sfruttarlo per calcolare, per ogni $v \in V$, la quantità $\sigma_{sv}$, ovvero il numero di cammini minimi che vanno da $s$ a $v$.

Iniziamo osservando che per ogni nodo $v$ nel livello 1, il numero di cammini minimi sarà pari ad 1 (perché $v$ è diretto vicino di $s$).

Invece al livello 2, il numero di cammini minimi di un generico nodo $v \in L_2$ è pari alla somma di tutti i $\sigma_{su}$, tali che esiste $(u,v)$ con $u \in L_1$. Più in generale, per ogni $v \in L_h$, possiamo calcolare $\sigma_{sv}$ con la seguente formula ricorsiva
$$
\begin{align*}
	\sigma_{sv} &= 1  &h = 1\\
	\sigma_{sv} &= \sum_{u \in N(v) \cap L_{h-1}} \sigma_{su}  &h > 1
\end{align*}
$$

![Calcolo numero dei cammini minimi.|550](ar-lesson08-img5.png)

Notare che questa fase viene eseguita facendo una visita `top-down` del sottografo $T(s)$.

### Fase 3
Nella terza fase dobbiamo calcolare per ogni arco $(u,v) \in T(s)$ la quantità di flusso che ci passa sopra rispetto a $s$, ovvero $$b_s(u,v) = \sum_{t \in V \setminus \lbrace s \rbrace} b_{st}(u,v)$$
Per fare ciò in questo caso faremo una visita `bottom-up` di $T(s)$.

Si $d$ il numero di livelli in $T(s)$, e consideriamo un arco $(y,x)$ con $y \in L_{d-1}$ e $x \in L_d$. 

Osserviamo che tutti gli shortest path che partono da $s$ e passano attravero l'arco $(y,x)$ sono tutti shortest path che terminano in $x$.

Perciò il flusso che passa su $(y,x)$ rispetto a $s$ sarà pari al rapporto tra il numero di shortest path che vanno da $s$ a $y$ (ovvero $\sigma_{sy}$) e il numero complessivo di shortest path che vanno da $s$ ad $x$ (ovvero $\sigma_{sx}$).
$$b_s(y,x) = \sum_{t \in V \setminus \lbrace s \rbrace} b_{st}(y,x) = b_{sx}(y,x) = \frac{\sigma_{sx}(y,x)}{\sigma_{sx}} = \frac{\sigma_{sy}}{\sigma_{sx}}$$
Analogamente possiamo applicare questo ragionamento per tutti gli archi che collegano nodi dell'ultimo livello, come mostrato nella seguente [[#^b81567|figura]].

![Calcolo di $b_s(y, x)$.|550](ar-lesson08-img6.png) ^b81567

Adesso, saliamo di un livello, e consideriamo gli archi che entrano nel livello $L_{d-1}$. 

Rifacendoci all'[[#^a89eb9|immagine in esempio]], consideriamo l'arco $(z, y)$, con $z \in L_{d-2}$ e $y \in L_{d-1}$.
![Esempio da considerare.](ar-lesson08-img7.png) ^a89eb9

Osserviamo che tra tutti i cammini minimi che passano per l'arco $(z, y)$, una parte saranno cammini che termineranno in $y$, e una parte saranno cammini minimi che andranno ai nodi di livello inferiore.

Perciò la frazione di shortest path che, partendo da $s$, passano per $(z, y)$ è pari alla somma dei seguenti fattori:
- La frazione degli shortest path da $s$ ad $y$, ovvero $\frac{\sigma_{sz}}{\sigma_{sy}}$.
- Per ogni *diretto discendente*[^2] $x$ di $y$, una frazione $\frac{\sigma_{sz}}{\sigma_{sy}}$ della frazione di shortest path da $s$ ad $x$ che passano per l'arco $(y,x)$, ovvero $\frac{\sigma_{sz}}{\sigma_{sy}} \cdot \frac{\sigma_{sy}}{\sigma_{sx}} = \frac{\sigma_{sz}}{\sigma_{sx}}$.

Quindi, rifacendoci all\'esempio, avremo che $$ b_s(z,y) = \frac{\sigma_{sz}}{\sigma_{sy}} + \frac{\sigma_{sz}}{\sigma_{sy}} \cdot \frac{\sigma_{sy}}{\sigma_{sx}} = \frac{1}{3} + \frac{1}{3}\cdot\frac{3}{5} = \frac{8}{15}$$
Così facendo abbiamo definito un metodo `bottom-up` per il calcolo tutti i valori $b_s(u,v)$.

![Calcolo di tutti i valori $b_s(u, v)$.|550](ar-lesson08-img8.png)

### Fase finale - Calcolo Betweenness
Concludendo, possiamo calcolare la betweennes di tutti gli archi come già descritto in precedenza $$b(u,v) = \frac{1}{2} \sum_{s \in V} b_s(u,v)$$
Notiamo che questa procedura permette di calcolare la betweennes degli archi in tempo *polinomiale* nella grandezza della rete (e non esponenziale).

Infatti la [[#Fase 1|fase 1]] è una semplice visita in ampiezza del grafo (e si può calcolare in tempo $O(|V| + |E|)$), la [[#Fase 2|fase 2]] è un'ulteriore visita in ampiezza di $T(s)$ (e si può calcolare ancora in tempo $O(|V| + |E|)$), e infine la [[#Fase 3|fase 3]] è nuovamente una visita in ampiezza, partendo però dal livello più basso (acnora una volta $O(|V| + |E|)$).

Dato che bisogna iterare questo procedimento per ogni nodo del grafo, e dato che nel caso peggiore abbiamo grafi molto densi con $\Theta(n^2)$ archi, la complessità temporale dell'esecuzione di questo algoritmo per il calcolo delle betweennes sarà $O(nm) \in O(n^3)$.

# Rilassare il modello
La definizione teorica discussa fin ora era basata su assunzioni forti:
un arco poteva essere un *bridge edge* o no, e poteva rappresentare uno *strong* o *weak tie*.
Riportare i concetti di *bridge edges* e *strong/waeak ties* in una rete sociale reale è però un'assunzione <u>troppo forte</u>.
Per esempio in una rete molto grande è molto raro trovare dei *bridge edges*, inoltre le persone non classificano le proprie relazioni in maniera dicotomica `strong/weak`, in genere ci sono moltissime sfumature.

Per rilassare i concetti di *strong* e *waeak ties* possiamo definire un modello in cui le relazioni tra due nodi della rete sono **pesate** in maniera numerica.
Quindi invece di considerare una rete $G = (V, S \cup W)$, potremmo per esempio definire una rete $G = (V,E,w)$ dove $w : E \rightarrow \mathbb{N}$ è una funzione che rappresenta la *forza* delle relazioni, più è alto il valore $w(u,v)$ più la relazione tra i nodi $u$ e $v$ è <u>forte</u>.

Per quanto riguarda il concetto di *bridge edge*, possiamo considerare il concetto di **neighborhood overlap**, ovvero la frazie di amicie in comune che hanno due nodi. $$NO(u,v) = \frac{ \vert N(u) \cap N(v) \vert }{ \vert N(u) \cup N(v) - \lbrace u,v \rbrace \vert }$$
Osserviamo che un *local bridge* $(u,v)$ ha un neighborhood overlap $NO(u,v) = 0$.

Sappiamo che i due concetti *bridge edges* e *strong/waeak ties* sono correllati, ovvero sappiamo che qualora una rete $G$ soddisfa la [[7 - Communities - Part 1#The Strong Triadic Closure Property|STCP]] allora tutti i *local bridge* sono *weak ties*.
Ciò che si può chiedere è se questo tipo di relazione persiste anche nel modello rilassato appena descritto.

Data la generalità del modello rilassato, è più complesso andare a fare un'analisi rigorosa. In aiuto però esistono dei risultati di tipo empirico risultati da un esperimento di *Onnela et al* del 2007.
Vennero presi i registri di una compagnia telefonica e costruito (in maniera anonima ?) sulla basse di essi un grafo, seguendo i seguenti criteri:
1. gli indirizzi telefonici vennero rappresentati dai nodi della rete.
2. esisteva un arco tra due nodi se essi si erano scambiati almeno una telefonata nelle ultime 18 settimane.
3. gli archi vennero persati in base alla durata delle chiamate effettuate, più due nodi parlano al telefono, più è alto il peso dell'arco.

Tale rete rappresentava una rete socialre reale, in quanto i numeri telefonici erano ad uso *comune* (e non *commericale* o *aziendale*), e inoltre la compagnia telefonica che ha fornito i dati copriva una vasta area di una popolazione.
A conferma, erano presenti alcune caratteristiche fondamentali delle reti sociali, come per esempio la presenza di *componenti giganti* (in questo caso grade l'84% della rete).

L'esperimento in questione consistette in due fasi:
1. nella prima vennero rimossi gli archi in ordine *decrescente* di peso, e si osservò che la rete si disconnetteva molto lentamente.
2. nella seconda fase si fece l'opposto, ovvero vennero rimossi gli archi a partire da quelli meno pesati, osservando invece che la rete si disconnetteva molto più velocemente.

Il risultato di questo esperimento suggerisce in maniera empirica che anche nel modello più rilassato c'è una connessione tra neighborhood overlap e peso di un arco.
Dato che la rimozione di archi con peso basso, rendeva più difficile la comunicazione tra gruppi di nodi, è sensato pensare che tanto più il peso di un arco è basso tanto più è bassa la neighborhood overlap di tale arco.

![Esempio di come cresce la neighborhood di un arco $N(u,v)$ al crescere del suo peso $w(u,v)$.|400](ar-lesson08-img9.png)

# Osservazioni finali
Ricapitolando tutto quello detto fin ora, una rete sociale è sostanzialmente un agglomerato di gruppi densamente connessi tra di loro con relazioni forti, i quali gruppi sono a loro volta connessi tramite una serie di connessioni deboli.

Concettualmente sappiamo che in un contesto sociale l'appartenenza ad un gruppo comporta vantaggi di vitale importanza.
In genere più una persona è ben inserita in un gruppo, ovvero più ha fiducia di chi gli sta intorno, più ne trae vantaggio.

Contrariamente, l'esperimento di Granovetter ci suggerisce che avvolte è altrettanto necessario avere contatti con gente appartenente ad altri gruppi, in quanto così facendo avremo accesso a differenti fonti di informazioni potenzialmente molto convenienti.

Queste due osservazioni ci permettono di inutire che i vantaggi dei nodi non dipendono solamente dall'appartenenza a comunità, bensì anche dalla posizione di tali nodi e da quanto sono ben inseriti o meno all'interno dei rispettivi gruppi.
Per aiutarci definiamo il concetto di **embeddedness** di un arco $(u,v)$ come la quantità di vicinato in comune ai due estremi di un arco, ovvero $$\text{Emb}(u,v) = \vert N(u) \cap N(v) \vert$$
Osserviamo che un *local bridge* è un arco con embeddedness pari a $0$.

Più la embeddedness di un arco è alta, più i due suoi vicini sono ben integrati tra di loro.
Analogamente, un nodo che tani archi incidenti con embeddedness alta è un nodo che è ben integrato nel gruppo a cui appartiene.

![|500](ar-lesson08-img10.png)

Nell'esempio in precedenza si può notare che il nodo `A` è ben integrato nella sua comunità, in quanto ricopre una **posizione centrale**.
Infatti tutti i suoi archi incidenti hanno una embeddedness alta.

Contrariamente il nodo `B` ricopre una posizione marginale nella sua comunità, e infatti ha pochi archi incidenti con embeddedness alta.


[^1]: ovvero **NP-completo**.

[^2]: ovvero quei nodi collegati ad $y$ da un arco e che si trovano al successivo livello più in basso.