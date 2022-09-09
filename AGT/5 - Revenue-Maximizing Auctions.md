# Revenue-Maximizing Auctions
## The Challenge of Revenue Maximization
Fin ora ci siamo interessati alla progettazione di meccanismi che massimizzasserro (*esattamente* o *approsimativamente*) una sorta di **benessere** sociale, ovvero
$$\text{social-surplus} = \sum_{i=1}^{n} v_i x_i$$
dove $(x_1,...,x_n) \in X$ è una [[3 - Myerson’s Lemma#^43b362|soluzione ammissibile]].

Un'asta è detta **welfare-maximizing** se ha come obiettivo quelli di massimizzare il social-surplus (come visto fin ora).

Ci siamo interessati a *welfare-maximizing auctions* per due semplici motivi:
1. è un obiettivo che spesso si incontra in scenari reali
2. in ogni ambiente a [[3 - Myerson’s Lemma#Single-Parameter Environments|singolo parametro]] esiste sempre un meccanismo DSIC che massimizza il *"benessere"* (esattamente come se il designer conoscesse in anticipo tutte le valutazioni private $v_i$).

In tali aste vengono generati dei **pagamenti**, però col solo scopo di incentivare i partecipanti a dichiarare il vero (ovvero per ottenere un meccanismo [[2 - Mechanism Design Basics#^a63f67|DSIC]]).

Cosa accade se ci prefissiamo l'ulteriore obiettivo di **massimizzare il ricavato** dell'asta il più possibile?

## One Bidder and One Item
Consideriamo il seguente esempio, ovvero una semplificazione dell'asta a singolo oggetto.

Supponiamo che oltre a un solo oggetto abbiamo un solo partecipante, con valutazione $v$.

Non essendoci altri concorrenti avversari, sarebbe troppo facile per il partecipante accaparrarsi l'oggetto facendo qualsiasi offerta.
Perciò assumiamo che l'oggetto all'asta abbia un prezzo $r$, fissato dal designer.

In tal caso lo spazio dei meccanismi si riduce al semplice meccanismo del **"prendere o lasciare"**.

Se il partecipante deciderà di *prende* l'oggetto pagherà il prezzo $r$.
Perciò, per non avere utilità negativa l'unica strategia (*dominante*) è quella di prendere se $v \geq r$, e lasciare se $v < r$.
Così facendo non solo si garantisce utilità non negatica, ma la si massimizza anche
$$
u = \max{\{0, v-r\}}
$$
Visto in termini di **rivcavi** (*revenue*) dell'asta, avremo che
$$
\text{revenue} = \begin{cases}
r &v \geq r\\
0 &v < r
\end{cases}
$$

Il social-surplus in questo problema sarà semplicemente pari a $v \cdot x$, dove $x =1$ se il partecipante accetta (ovvero $v \geq r$) o $x=0$ se rifiuta ($v < r$).

È molto semplice massimizzare il surplus (il *benessere*), basta porre $r=0$.
Così facendo si da sempre l'oggetto *"gratis"*, ottenendo sempre utilità massima pari a $v$.
Notare che $r$ è **indipendente** dal valore di $v$.

Così facendo (massimizzando il benessere del partecipante) però stiamo minimizzando i **ricavi** (*revenue*) dell'asta, il quale avrà sempre valore pari a $0$.

Se volessimo invece massimizzare i ricavi dell'asta basterebbe porre $r = v$.
Così, il partecipante comprerebbe sempre l'oggeto, al prezzo massimo il quale è disposto a pagare.
Ma non conoscendo $v$ come potremmo fare a massimizzare i guadagni?

Il problema fondamentale è che differenti meccanismi funzionano meglio o peggio in base a differenti input.
Per esempio, il meccanismo che consiste nel porre $r=20$ funziona bene quando $v$ è uguale o poco maggiore di $20$.
Ha invece pessime performance quando $v < 20$, con surplus e revenue pari a $0$. ^5f39d8

## Bayesian Analysis
Come abbiamo visto [[#^5f39d8|prima]], nel problema della massimizzazione dei guadagni le performance di un meccanismo dipendono dai diversi input (perché non conosciamo le valutazioni private).

Introduciamo e studiamo quindi il modello noto come **avarage-case** o **Bayesiano**, nel quale si assume che le valutazioni dei partecipanti rispettano certe **distribuzioni**, aprendo quindi la possibilità di studiare i **guadagni medi**.

Più formalmente, gli ingredianti di un ambiente Bayesiano sono:
- Un *enviroment* a [[3 - Myerson’s Lemma#Single-Parameter Environments|singolo parametro]].
- La valutazione privata $v_i$ del partecipante $i$ segue una *distribuzione* con funzione di ripartizione $F_i$ e funzione di densità $f_i$, con **supporto** $\left[0, v_{\max} \right]$. Assumiamo inoltre che le distribuzioni $F_1,...,F_n$ siano **indipendenti**.
- Le distribuzioni $F_1,...,F_n$ sono note *a priori* dal designer. Mentre le reali valutazioni $v_1,...,v_n$ sono *private* (come al solito).

Osservare che, dato che siamo interessati a meccanismi [[2 - Mechanism Design Basics#^a63f67|DSIC]] per i quali (<u>per definizione</u>) esiste sempre una strategia dominante, i partecipanti non hanno bisogno di conoscere le relative distribuzioni $F_1,...,F_n$.

In questo ambiente **Bayesiano** è semplice definire un'asta *"ottima"* che massimizza il guadagno (revenue).
Il meccanismo ottimo è quello che tra tutti i meccanismi DSIC ha il più alto **valore atteso** rispetto alla [[Distribuzioni Multivariate#Joint PDF|distribuzione congiunta]] $F_1 \times F_2 \times ... \times F_n$ rispetto alle valutazioni $\mathbf{v}$.

Per esempio, rispetto all'asta [[5 - Revenue-Maximizing Auctions#One Bidder and One Item|one-bidder one-item]] il guadagno atteso rispetto al prezzo $r$ sarà
$$ r \cdot (1 - F(r)) $$
dove $F(r)$ è la probabilità che $v \leq r$.

Modellando in questa maniera è semplice identificare il valore ottimo (in media) del prezzo $r$.
Chiamiamo il prezzo ottimo $r$ con il nome **monopoly price** della distribuzione $F$.

Per esempio, se $v \sim U\left[0,1\right]$ è uniforme (ovvero quando $F(x) = x$) è facile constatare che il *monopoly price* dell'asta [[5 - Revenue-Maximizing Auctions#One Bidder and One Item|one-bidder one-item]] è $r = 1/2$, involvendo in un guadagno medio di $1/4$.

Lo stesso ragionamento si può applicare anche con due partecipanti, dove lo spazio dei meccanismi DSIC si espande.
Per esempio, consideriamo la solita asta a singolo oggetto con due partecipanti, le quali valutazioni sono **i.i.d.** dall'uniforme in $\left[0, 1\right]$.
In tal caso si potrebbe applicare la solita [[2 - Mechanism Design Basics#Vickrey's Second-Price Auctions|Vickrey's second-price auctions]], è constatare che il *guadagno medio* è esattamente il valore atteso dell'offerta più bassa, ovvero $1/3$ (vedi **[esercizi]**).

Un altro esempio potrebbe essere quello di integrare nell'asta di Vickrey un **prezzo soglia**, come accade nelle aste su eBay.
Più precisamente, vince il partecipante che offre di più, a meno che tutte le offerte non siano minori del *prezzo soglia* $r$, e in tal caso non vince nessuno.
Il pagamento del vincitore sarà pari al massimo tra $r$ è la seconda miglior offerta.
In questo sistema ci sono sia dei vantaggi che svantaggi dal punto di vista dei guadagni, ovvero:
- se tutti offrono meno di $r$ nessuno vince e il guadagno sarà $0$.
- se un solo partecipante offre più di $r$ allora aumenta il guadagno, anziché far pagare l'offerta del secondo miglior offerente.
Si può verificare (**[vedi esercizio]**) che il guadagno medio in questa asta è di $5/12$ (meglio di $1/3$ senza la presenza del prezzo soglia).

Ciò che ci chiedimo è quindi:
si può fare meglio, per esempio cambiando il prezzo soglia $r$ oppure con totalmente un altro approccio ?

----------------------
# Expected Revenue Equals Expected Virtual Welfare
Cerchiamo ora di *caratterizzare* come è fatta un'asta DSIC *ottima* (ovvero che massimizza in valore atteso i guadagni) per ogni ambiente a singolo parametro e per ogni distribuzione $F_1,...,F_n$.

### Step 0
Per il [[4 -  Knapsack Auctions#^ec0935|revelation principle]] sappiamo che ogni asta per la quale esiste una *strategia dominante* esiste un'asta equivalente e DSIC.
Perciò d'ora in poi consideriamo solo meccanismi *truthful*, ovvero dove $\mathbf{b} = \mathbf{v}$.

Il valore atteso dei guadagni di tali aste $(\mathbf{x}, \mathbf{p})$ è
$$\mathbb{E}_{\mathbf{v}} \left[ \sum_{i=1}^{n} p_i(\mathbf{v}) \right]$$
dove tale media è calcolata rispetto a $\mathbf{v} \sim F_1 \times ... \times F_n$ (ovvero le offerte dei partecipanti).

### Step 1
Fissiamo un $i$ e un $\mathbf{v}_{-i}$.
Ricordiamo che $\mathbf{v}_{-i}$ è una v.a. e quindi integreremo su di essa in seguito.
Per la [[3 - Myerson’s Lemma#^539c5a|formula dei pagamenti]] possiamo calcolare il pagamento atteso del partecipante $i$ dati le altre offerte $\mathbf{v}_{-i}$
$$\mathbb{E}_{v_i \sim F_i} \left[ p_i(\mathbf{v}) \right] = \int_0^{v_{\max}}p_i(\mathbf{v})f_i(v_i) \,dv_i = \int_0^{v_{\max}} \left[ \int_{0}^{v_i} z \cdot x'_i(z, \mathbf{v}_{-i}) \,dz \right] f_i(v_i) \,dv_i$$
Notare che nella prima uguaglianza stiamo sfruttando l'**indipendenza** delle offerte dei partecipanti, ovvero $\mathbf{v}_{-i}$ non ha alcuna influenza sulla distribuzione $F_i$.

Cerchiamo ora di riscrivere la media dei pagamenti in funzione della sola regola di allocazione $x_i$.

### Step 2
Invertendo l'ordine del doppo integrale otterremo che
$$\begin{align}
\int_0^{v_{\max}} \left[ \int_{0}^{v_i} z \cdot x'_i(z, \mathbf{v}_{-i}) \,dz \right] f_i(v_i) \,dv_i
&= \int_0^{v_{\max}} \overbrace{\left[ \int_{z}^{v_{\max}} f_i(v_i) \,dv_i \right]}^{P(v_i \geq z)} z \cdot x'_i(z, \mathbf{v}_{-i}) \,dz \\
&= \int_0^{v_{\max}} (1 - F_i(z)) \cdot z \cdot x'_i(z, \mathbf{v}_{-i}) \,dz 
\end{align}$$

### Step 3
Integrando per parti
$$\begin{align}
&\int_0^{v_{\max}} \underbrace{(1 - F_i(z)) \cdot z}_{f} \cdot \underbrace{x'_i(z, \mathbf{v}_{-i})}_{g'} \,dz =\\
=& (1 - F_i(z)) \cdot z \cdot x_i(z, \mathbf{v}_{-i}) \Big\vert_0^{v_{\max}} - \int_0^{v_{\max}} (-zf_i(z) + 1 - F_i(z)) \cdot x_i(z, \mathbf{v}_{-i}) \,dz\\
=& (1 - \underbrace{F_i(v_{\max})}_1) \cdot z \cdot x_i(v_{\max}, \mathbf{v}_{-i}) - (1 - F_i(0)) \cdot z \cdot \underbrace{x_i(0, \mathbf{v}_{-i})}_0 - \int_0^{v_{\max}} \frac{f_i(z)}{f_i(z)}\left(1 - F_i(z)-z\right) \cdot x_i(z, \mathbf{v}_{-i}) \,dz\\
=& -\int_0^{v_{\max}} \left(\frac{1 - F_i(z)}{f_i(z)}-z\right) \cdot x_i(z, \mathbf{v}_{-i}) \cdot f_i(z) \,dz\\
=& \int_0^{v_{\max}} \left(z-\frac{1 - F_i(z)}{f_i(z)}\right) \cdot x_i(z, \mathbf{v}_{-i}) \cdot f_i(z)\,dz
\end{align}$$
Per comodità indichiamo la funzione
$$\varphi_i(z) := z - \frac{1-F_i(z)}{f_i(z)}$$
perchiò
$$\mathbb{E}_{v_i \sim F_i} \left[ p_i(\mathbf{v}) \right] = \int_0^{v_{\max}} \varphi_i(z) \cdot x_i(z,\mathbf{b}_{-i}) \cdot f_i(z) \, dz$$

### Step 4
Definiamo la **valutazione virtuale** (**virtual valuation**) $\varphi_i(v_i)$ del player $i$ con valutazione $v_i \sim F_i$
$$\varphi_i(v_i) := v_i - \frac{1-F_i(v_i)}{f_i(v_i)}$$
Notare che tale *valutazione virtuale* dipende **solamente** dalla valutazione privata del relativo player, e non da quelle degli altri. ^9edaa0

Osservare che
$$\mathbb{E}_{v_i \sim F_i} \left[ \varphi_i(v_i) \right] = \int_0^{v_{\max}} \varphi_i(z) \cdot f_i(z)\, dz $$
perciò possiamo semplificare il tutto e dire che per ogni $i$ e per ogni $\mathbf{v}_{-i}$ **fissato** avremo che il pagamento medio di $i$ campionando la sua valutazione privata $v_i \sim F_i$ sarà

$$\mathbb{E}_{v_i \sim F_i} \left[ p_i(\mathbf{v}) \right] = \int_0^{v_{\max}} \varphi_i(z) \cdot x_i(z,\mathbf{b}_{-i}) \cdot f_i(z)\, dz = \mathbb{E}_{v_i \sim F_i} \left[ \varphi_i(v_i) \cdot x_i(\mathbf{v}) \right]$$ ^2b6eca

#### Esempio
Supponiamo che $v_i \sim U\left[ 0,1 \right]$.
Avremo quindi che $F_i(z) = z$ e $f_i(z) = F'_i(z) = 1$.
Perciò $$\varphi_i(z) = z - \frac{1-z}{1} = 2z - 1$$ per ogni $z \in \left[ 0, v_{\max} \right]$.
Osserviamo inoltre che $\varphi_i(z) \in \left[ -1, 1 \right]$.

#### Interpretazione della valutazione virtuale
La *valutazione virtuale* gioca un ruolo importante nella progettazione di aste bayesiane ottimali.
È utile chiedersi se si può dare un qualche significato *intuitivo* a $\varphi_i(v_i)$.

Un modo per interpretare i due addendi che compongono $\varphi_i(v_i)$ è considerando $v_i$ il **massimo guadagno** che è possibile ottenere da $i$ (di più $i$ non può pagare altrimenti avrebbe utilità negativa) mentre il secondo elemento $(1-F_i(v_i))/f_i(v_i)$ la **perdita** di guadagno causata dalla **mancata conoscenza a priori** di $v_i$ (aka *"information rent"*).
$$\varphi_i(v_i) = \underbrace{v_i}_{\text{what you'd like to charge }i} - \underbrace{\frac{1-F_i(v_i)}{f_i(v_i)}}_{\text{"information rent" earned by bidder } i}$$

Un secondo modo più accurato è quello di pensare a $\varphi_i(v_i)$ come la **pendenza** di una *"curva dei ricavi"*, dove tale curva traccia il *guadagno atteso* dal player $i$ campionando $v_i \sim F_i$. 

### Step 5
Applichiamo al [[#^2b6eca|formula]] ad ogni elemento di $\mathbf{v}$, ottenendo
$$\begin{align}
\mathbb{E}_{\mathbf{v} \sim F_1 \times ... \times F_n} \left[ p_i(\mathbf{v}) \right]
&= \idotsint_{\left[0,v_{\max} \right]^n} p_i(z_1, ..., z_n) 
 \cdot f_1(z_1) f_2(z_2) \dots f_n(z_n) \, dz_1 \dots dz_n\\
&= \idotsint_{\left[0,v_{\max} \right]^n} \varphi_i(z_i)x_i(z_1, ..., z_n) \cdot f_1(z_1) f_2(z_2) \dots f_n(z_n)\, dz_1 \dots dz_n\\
&= \mathbb{E}_{\mathbf{v} \sim F_1 \times ... \times F_n} \left[ 
 \varphi_i(v_i)\cdot x_i(\mathbf{v})\right]
\end{align}$$
o in maniera più compatta
$$\mathbb{E}_{\mathbf{v}} \left[ p_i(\mathbf{v}) \right] = \mathbb{E}_{\mathbf{v}} \left[ \varphi_i(v_i) \cdot x_i(\mathbf{v}) \right]$$

### Step 6
Applicando la **linearità** del valore atteso
$$\mathbb{E}_{\mathbf{v}} \left[ \sum_{i=1}^{n} p_i(\mathbf{v}) \right] = \sum_{i=1}^{n} \mathbb{E}_{\mathbf{v}} \left[ p_i(\mathbf{v}) \right] = \sum_{i=1}^{n}\mathbb{E}_{\mathbf{v}} \left[ \varphi_i(v_i) \cdot x_i(\mathbf{v}) \right] = \mathbb{E}_{\mathbf{v}} \left[ \sum_{i=1}^{n}\varphi_i(v_i) \cdot x_i(\mathbf{v}) \right]$$

Abbiamo quindi riscritto il pagamento medio in funzione della sola funzione di allocazione $\mathbf{x}$  , e delle probabilità $F_1,...,F_n$.

$$\mathbb{E}_{\mathbf{v}} \left[ \mathbf{p}(\mathbf{v}) \right] = \mathbb{E}_{\mathbf{v}} \left[ \Phi(\mathbf{v}) \cdot \mathbf{x}(\mathbf{v}) \right] $$
Osserviamo che nella formula appena trovata, se rimovessimo la funzione $\Phi$ ci ritroveremmo con il **social-surplus atteso**, ovvero
$$\text{EXPECTED SURPLUS} = \mathbb{E}_{\mathbf{v}}\left[\sum_{i=1}^{n} v_i \cdot x_i(\mathbf{v})\right] $$
perciò (analogamente a prima) ci riferiremo alla quantità $\sum_{i=1}^{n} \varphi_i(v_i) \cdot x_i(\mathbf{v})$ con il termine **virtual surplus**

$$\text{VIRTUAL SURPLUS} = \sum_{i=1}^{n} \varphi_i(v_i) \cdot x_i(\mathbf{v})$$ ^7b8481

In conclusione possiamo dire che massimizzare i guadagni attesi (<u>nello spazio dei meccanismi DSIC</u>) equivale a massimizzare il virtual surplus atteso

> **EXPECTED REVENUE = EXPECTED VIRTUAL WELFARE**

^c86ff2

--------------------
# Bayesian Optimal Auctions
Il [[#^c86ff2|risultato]] appena trovato è molto comodo, in quanto ci consente di trascurare i pagamenti e concentrarci solamente su un problema di ottimizzazione che riguarda solamente la regola di allocazione.

## Maximizing Expected Virtual Welfare
Prima di cercare di caratterizzare l'asta *"ottima"* (in termini di ricompense) per un <u>qualsiasi</u> problema a singolo parametro, partiamo da casi più semplici e restrittivi.

Innanzitutto partiamo col considerare un'asta a singolo oggetto.
Dopodiché, assumiamo che le valutazioni private siano **i.i.d**, ovvero $F_1 = F_2 = ... F_n = F$.

Così facendo avremo anche che $f_1 = f_2 = ... = f_n = f$ e quindi $\varphi_1 = \varphi_2 = ... = \varphi_n = \varphi$.

Indichiamo con $\mathbf{F}$ la distribuzione congiunta $F_1 \times ... \times F_n \equiv F^n$.

In questo contesto e con queste assunzioni, come possiamo scegliere un $\mathbf{x}$ (DSIC) che massimizzi <u>in media</u> il [[#^7b8481|virtual surplus]]?

$$\mathbf{x} = \arg \max_{(x_1,...,x_n) \in X} \; \mathbb{E}_{\mathbf{v} \sim \mathbf{F}} \left[ \sum_{i=1}^{n} \varphi(v_i) x_i(\mathbf{v}) \right]$$ ^8ee749

Purtroppo non abbiamo controllo sulla distribuzione $\mathbf{F}$, perciò su $\mathbf{v}$ e $\varphi$.

Il modo più semplice è quello di massimizzare il *virtual surplus* separatamente per ogni input $\mathbf{v}$ (*pointwise*).

Nell'asta a singolo oggetto abbiamo che $\mathbf{x}(\mathbf{v}) \in \{0,1\}^n$ con la costrizione che *al più* una sola entry deve essere $1$.
Perciò la regola di allocazione $\mathbf{x}$ che massimizza il *virtual surplus* equivale semplicemente alla regola che elegge come vincitore il player $i$ con **[[#^9edaa0|valutazione virtuale]]** $\varphi(v_i)$, massima, ovvero
$$\text{winner} = \arg\max_{i=1,...,n} \varphi(v_i)$$

> Ricordiamo che la valutazione virtuale può anche assumere valori **negativi** ([[#Esempio|vedi esempio]]).
> Perciò, nel caso in cui tutti i $\varphi(v_i)$ siano $< 0$, la regola $\mathbf{x}$ che massimizza il *virtual surplus* è quella che non dichiara nessun vincitore.

Trovare un $\mathbf{x}$ che massimizza il *[[#^7b8481|virtual surplus]]* separatamente per ogni $\mathbf{v}$ definisce una regola di allocazione che lo massimizza anche in [[#^8ee749|media]] tra tutte le possibili regole di allocazione possibili in $X$ (monotone o no).

Noi però vorremmo anche che tale regola di allocazione $\mathbf{x}$ che massimizza il *virtual surplus* abbia una *strategia dominante*.
Se così fosse, per il [[4 -  Knapsack Auctions#^ec0935|principio di rivelazione]] sappiamo che possiamo estendere $\mathbf{x}$ ad essere [[2 - Mechanism Design Basics#^a63f67|DSIC]], e per il [[3 - Myerson’s Lemma#Theorem Mayerson's Lemma|Mayerson's Lemma]] ciò equivale a dire che $\mathbf{x}$ è [[3 - Myerson’s Lemma#^3c29c4|monotona]].
In fine sappiamo che se $\mathbf{x}$ è monotona allora tale meccanismo che massimizza il *virtual surplus* (in media) massimizzerebbe anche i *guadagni medi* (vedi [[#Step 6]]).

La risposta a tale domanda chiave dipende dalla distribuzione $F$.
Infatti si può dimostrare che se $F$ è tale che la [[#^9edaa0|valutazione virtuale]] $\varphi$ è **strettamente crescente**, allora la regola di allocazione $\mathbf{x}$ che abbiamo trovato che massimizza il *virtual surplus* allora è **monotona** (e quindi massimizza anche il *guadagno atteso*).

> **Def.** Una distribuzione $F$ è **regolare** se la corrispondente [[#^9edaa0|valutazione virtuale]] $v - \frac{1-F(v)}{f(v)}$ è **strettamente crescente**.

Per esempio la distribuzione uniformo vista nell'[[#Esempio]] è *regolre*.
Anche altre distribuzioni lo sono, come per esempio quella [esponenziale](https://en.wikipedia.org/wiki/Exponential_distribution) o quella [log-normale](https://en.wikipedia.org/wiki/Log-normal_distribution).

Perciò possiamo dire che nell'asta a singolo oggetto con distribuzioni i.i.d. e con l'assunzione di *regolarità* delle distribuzioni, la regola di allocazione $\mathbf{x}$ che elegge vincitore il partecipante con massima (<u>non negativa</u>) *valutazione virtuale* $\varphi(v_i)$ (se esiste), è *monotona* e quindi induce in un'asta *"ottima"* che **massimizza il social-surplus (esattamente) e il guadagno atteso**.

Osservare inoltre che dato che tutti i partecipanti condividono la stesso funzione di *valutazione virtuale* $\varphi$, e dato che stiamo assumendo che $F$ è regolare, allora il player con $\varphi(v_i)$ più alto equivale al player con valutazione privata $v_i$ più alta.

Notare che l'unica differenza tra la [[2 - Mechanism Design Basics#Vickrey's Second-Price Auctions|Vickrey's second-price auctions]] che elegge il partecipante con offerta (o valutazione dato che è DSIC) maggiore e la regola $\mathbf{x}$ appena descritta che elegge il partecipante con *valutazione virtuale* $\varphi$ massima, è che nella seconda possono capitare casi in cui non viene eletto nessun vincitore (ovvero quando $\max \varphi(v_i) < 0$).

In poche parole la regola $\mathbf{x}$ appena trovata equivale sostanzialmente alla [[2 - Mechanism Design Basics#Vickrey's Second-Price Auctions|Vickrey's second-price auctions]] con **prezzo minimo** $r = \varphi^{-1}(0)$.
Infatti se la valutazione massima $v^* \geq r = \varphi^{-1}(0)$ allora avremo una *valutazione virtuale*
$$\varphi(v^*) \geq \varphi(r) = \varphi(\varphi^{-1}(0)) = 0 \implies \varphi(v^*) \geq 0$$
e quindi esiste un vincitore.

In maniera pià generale vedremo che in un qualsiasi ambiente a singolo parametro con distribuzioni $F_1,...,F_n$, se tali distribuzioni sono **tutte regolari** allora la regola di allocazione $\mathbf{x}$ che massimizza (dipendentemente da $\mathbf{v}$) il [[#^7b8481|surplus virtuale]] è **monotona**, perciò avremo trovato un meccanismo DSIC e che massimizza in valore atteso i guadagni.

In questo senso possiamo dire di aver risolto le aste Bayesiane per ogni ambiente a singolo parametro a patto di avere distribuzioni regolari.