# Mechanism Design Basics

## Single item auction
Il **mechanism design** può essere vista come la *scienza del rule-making*.
In questi appunti verrà trattato un noto problema di mechanism desing, ovvero il problema dell'**asta a singolo oggetto**.

Come *setup* consideriamo una situazione in cui vogliamo mettere all'astra un oggetto, e dove partecipano $n$ acquirenti.
Ogni acquirente è intenzionato a vincere l'asta, e ognuno ha una **informazione privata**, ovvero quanto secondo lui vale l'oggetto.
Ci riferiremo a tela informazione privata con il termine **valutazione**.
Indichiamo con $v_i$ la valutazione del partecipante $i$-esimo.

Dalla partecipazione all'asta ogni acquirente ne ricava un guadadno, espresso sotto forma di **utilità**.

Un partecipante $i$ che perde l'asta avrà utilità $u_i = 0$.
Il votante $i$ che vince avrà utilità $u_i = v_i - p$, dove con $p$ si indica il **pagamento** del debito che il vincitore dovrà risarcire.
Tale modello di utilià è noto come **quasilineare**.

L'**obiettivo** del designer dell'asta è quello di assegnare l'oggetto al partecipante con valutazione *più alta*, nonostante tali informazioni sono private.

### Sealed-Bid Auctions
Un primo modello di meccanismo d'asta è il seguente:
1. Ogni player $i$ comunica privatamente al banditore dell'asta un'**offerta**, che indichiamo con $b_i$. ^9c5bb1
2. Il banditore *sceglie* un vincitore. ^8059f6
3. In fine decide a quale prezzo $p$ verrà venduto l'oggetto. ^6127d0

Per quanto riguarda il punto [[#^9c5bb1|(1)]] e [[#^8059f6|(2)]] una soluzione semplice è quella di scegliere il partecipante che ha fatto l'offerta $b_i$ più alta.

Per quanto riguarda il punto [[#^6127d0|(3)]] invece non è banale scegliere il pagamento $p$, in quanto dipende fortemente dalle offerte $b_1, ..., b_n$.

Per esempio (volendo essere totalmente generosi) poniamo $p = 0$ sempre.
In tal caso ogni partecipante sarebbe incenitvato a dichiarare $\infty$, e in tal caso non sarebbe possibbile identificare un vincitore.

### First-Price Auctions
Un primo approccio naturale sarebbe quello di far pagare al vincitore i suoi debiti, ovvero quanto offerto.
In tal caso avremo che se il vincitore risultante è il giocatore $i$-esimo, allora esso avrà utilità $u_i = v_i - b_i$.

In tal caso a nessun partecipante conviene proporre un'offerta maggiore della propria valutazione, perché se dovessa avere la (s)fortuna di vincere otterrebbe una utilità *negativa*.

Inoltre non si ha alcuna certezza che dichiarando di meno si può massimizzare la probabilità di vittoria (anzi ...).

### Vickrey's Second-Price Auctions
Un meccanismo invece molto ragionevole è il meccanismo di *Vickrey*, noto anche come **second-price auctions**.
In questo sistema il vincitore dell'asta, ovvero colui che offre più di tutti, dovrà pagare un prezzo $p$ pari alla **seconda** maggiore offerta.

Questo meccanismo può sembrare banale, ma in realtà nasconde delle proprietà molto interessanti.

> **Claim 1** Nella second-price auctions ogni partecipante ha una **strategia dominante**: proporre $b_i$ pari alla propria valutazione privata $v_i$. 
> Ovvero la strategia che massimizza l'utilità di ogni player $i$ è quella di dichiarare il vero, **indipendentemente** dalle offerte degli altri partecipanti.

^94869b

Questa proprietà implica che questo meccanismo è *facile* da applicare da parte dei partecipanti (che non dovranno impegnarsi troppo per capire qual è la strategia ottimale).

Quando in un meccanismo è *strategia dominante* dichiarare sempre il vero è detto **truthful**.

> **Proof** Indichiamo con $b = (b_1, ..., b_n)$ il *vettore* di offerte fatte dai partecipanti, e con $b_{-i} = (b_1, ..., b_{i-1}, b_{i+1}, ... b_n)$ il vettore di tutte le offerte fatte, esclusa quella del partecipante $i$-esmo.
> 
> Si vuole dimostrare che per ogni partecipante $i$, indipendente da quanto offrono gli altri in $b_{-i}$, la strategia che massimizza $u_i$ è quella di porre $b_i = v_i$.
> 
> Sia $B$ la massima offerta in $b_{-i}$, ovvero
> $$B = \max_{j \ne i}{b_j}$$
> Se $b_i <B$ certamente $i$ non vince, e quindi avrà utilità $u_i = 0$. Consideriamo quindi il caso in cui $b_i \geq B$. In tal caso $i$ vince e avrà utilità $u_i = v_i - B$. Perciò l'utilità del giocatore $i$ sarà
> $$u_i = \max{\{v_i - B, 0\}}$$
> Consideriamo i due scenari possibili.
> Nel caso $v_i < B$ avremo che la massima utilità che può ottenere $i$ è pari a $0$, e questo può ottenerlo dichiarando il vero $b_i = v_i$. Infatti se $i$ pone $b_i = v_i < B$ allora $i$ non vince. Se invece dichiara $b_i > v_i$ rischia di vencere, e quindi di ottenere *utilità negativa*.
> 
> Nel caso invece $v_i \geq B$ l'utilità massima che può ottenere $i$ è $v_i - B$. Questo valore si ottiene certamente con $b_i = v_i$. Se invece dichiara $b_i < v_i$ rischia di perdere e di ottenere *utilità nulla* $\square$.


Da questa prima proprietà si può derivare una seconda proprietà importante.

> **Claim 2** Nella second-price auctions è garantita una utilità **non negativa** ad ogni offerente *sincero*.
> 
> **Proof:** Dalla proprietà [[#^94869b|(1)]] abbiamo visto che *indipendentemente* dalle altre offerte $b_{-i}$ al partecipante $i$ è garantita una utilità pari a $u_i = \max{\{v_i - B, 0\}} \geq 0$ $\square$. 

^bffa08

> **Theorem 1** *The Vickrey auction is awesome.* Ovvero tale meccanismo gode delle seguenti tre proprietà desiderabili da un meccanismo d'asta:
> 1. **strong incentive guarantees**: Il meccanismo è detto **dominant-strategy incentive-compatible** (in breve DSIC). Questa proprietà è l'unione delle proprietà [[#^94869b|(1)]] e [[#^bffa08|(2)]], ovvero la strategia ottimale per ogni partecipante è quella di dichiarare sempre la propria valutazione privata, e ciò è sufficiente a garantire utilità *non negative*.
> 2. **strong performance guarantees**: Se tutti i partecipanti dichiarano il vero, allora si massimizza il **costo sociale** (o **social surplus**). Ricordando gli obiettivi del problema, per costo sociale si intende la valutazione privata del vincitore. Per motivi che vedremo, indichiamo il costo sociale con la seguente espressione
> 	$$\sum_{i=1}^{n}v_i x_i$$
> Dove $x_i$ vale $1$ sei il giocatore $i$ vince, altrimenti $0$.
> 3. **computational eciency**: l'asta può essere implementata in **tempo polinomiale** (in realtà in tempo **lineare**).

^e83fd1

La prima proprietà è dimostratà dai claim [[#^94869b|(1)]] e [[#^bffa08|(2)]].
La seconda proprietà è banalmente verificabile dal fatto che, se tutti dichiarano il vero, vincerà certamente il partecipante $i$ con valutazione *massima*. Perciò, supponiamo che il partecipante $i$ vince l'asta, avremo che
$$\sum_{i=1}^{n} v_ix_i = v_1 \cdot 0 + ... + v_{i-1} \cdot 0 + v_{i} \cdot 1 + v_{i+1} \cdot 0 + ... + v_{n} \cdot 0 = v_i = \max_{j=1,...,n}{v_j}  $$
L'ultimo punto è ancor più semplice da constatare perché in tempo costante ogni partecipante sa qual è la sua strategia ottimale, mentre al banditore basterà semplicemente scorrere tutte le offerte e trovare le due più alte per ricavare vincitore e pagamento.

## Sponsored Search Auctions
Una ricerca sul web comporta un insieme di **pagine** *"organiche"* (ovvero pagine inerenti alla ricerca restituite da un algoritmo come il [PageRank](https://it.wikipedia.org/wiki/PageRank)) e un insieme di pagine *"sponsorizzate"*, ovvero link pagati per essere posti come pubblicità.

Ogni volta che si effettua una ricerca su un motore di ricerca viene effettuata un'asta in real time per determinare quali pagine sponsorizzare e in quale ordine.  Quest'asta è nota come **sponsored search auction**.

### The Basic Model
Introduciamo un primo modello *semplicistico* ma comunque interessante da analizzare.

Supponiamo di avere solamente $k>1$ *"slot"* disponibili per le sponsorizzazioni, e supponiamo inoltre che le pagine partecipanti all'asta siano *inerenti* alla ricerca.

Un'altra assunzione è che gli slot non abbiano tutti lo stesso valore, in termini di qualità. Quantifichiamo le qualità degli slot tramite il concetto di **click-through-rate** (o CTR). Il CTR $\alpha_j$ rappresenta la probabilità che lo slot $j$-esimo venga cliccato in seguito alla ricerca. Dato che si tratta di una distribuzione, avremo che
$$\sum_{j=1}^{k} \alpha_j = 1$$
Un'assunzione ragionevole che si può fare è che gli slot sono ordinati in senso non crescente di CTR, ovvero $\alpha_1 \geq \alpha_2 \geq ... \geq \alpha_k$. Infatti è ragionevole assumere che una pagina in posizione 1 ha una probabilità più alta di essere visitata, piuttosto che la pagina in posizione 10. ^af7b13

Un'altra assunzione, poco ragionevole ma necessaria per questioni di complessità, è che i CTR sono **indipendenti** dai propri accupanti.

Assumiamo in fine che la valutazione privata di ogni partecipante è misurata tramite al guadagno $v_i$ che ha *per ogni click*.
Perciò se la pagina $i$ viene assegnata allo slot $j$, mediamente risceverà un guadagno di $v_i \cdot \alpha_j$.

Ciò che vogliamo è definire un meccanismo che abbia le stesse proprietà ottenute dal meccanismo di [[#^e83fd1|Vickrey]]  nell'asta a singolo oggetto, ovvero:
1. **DSIC.** Deve essere strategia dominante dichiarare il vero (ovvero la propria valutazione privata), e inoltre questo deve essere sufficiente a garantire utilita *non negativa*. ^a63f67
2. **Social surplus maximization.** Il costo sociale è massimizzato quando tutti dichiarano il vero. In questo il costo sociale è definito sempre come
   $$\sum_{i=1}^{n}v_ix_i$$
   dove per $x_i$ è pari al CTR dello slot $j$ assegnato al partecipante $i$-esiamo ($0$ se la pagina non è tra i $k$ vincitori). ^db3c63
3. **Polynomial running time.** Questo punto è importantissimo, dato che un numero immenso di ricerche vengono effettuate ogni giorno. ^92a5ec

### Our Design Approach
Possiamo dividere il problema in due sottoproblemi: decidere *chi vince cosa* e poi dicidere *quanto pagano*.
Il nostro approccio sarà quindi:
- **Step 1:** Assumiamo, senza giustificazioni, che tutti i partecipanti dichiarino il vero. Cerchiamo un modo di definire i vincitori (e cosa vincono) in modo tale da rispettare le proprietà [[#^db3c63|(2)]] e [[#^92a5ec|(3)]]. ^40f690
- **Step 2:** Dati i vincitori e i premi dallo Step 1, cerchiamo di definire uno *schema di pagamenti* per i vincitori in modo tale da assicurarci che gli convenga sempre dichiarare il vero, ovvero che occora la proprietà [[#^a63f67|(1)]]. ^9a8ea2

Se riusciamo a *"centrare"* entrambi i punti correttamente, avremo definito un meccanismo con le proprietà desiderate.