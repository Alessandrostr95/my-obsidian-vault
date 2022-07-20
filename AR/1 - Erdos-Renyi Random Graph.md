# Analisi di Reti - definizione

Nel senso più ampio del termine, una **rete** è uno schema di *interconnessioni* tra entità.
A seconda del tipo di entità e di interconnessioni, possono esistere diverse tipologie di reti, come:
-   reti di calcolatori che interagiscono tra di loro per eseguire task di computazione distribuita
-   le reti sociali, come per esempio una rete di conoscenza di un social network
-   le reti di informazioni, ovvero il *web*
-   ...

Lo studio di una rete po\' interessare ed essere affrontato da differenti discipline: nella matematica/informatica studiando i modelli che le rappresentano e gli algoritmi che le coinvolgono, nell\'economia studiando le dinamiche di incentivo/disincentivo che influenzano il comportamento delle entità, o anche nelle scienze sociali per quanto riguarda i fenomeni che influenzano i singoli individui o l'intera popolazione.
Sostanzialmente, il modo in cui ci interesserà osservare una rete è come un ampio insieme di individui che reagisce in base al comportamento dei singoli.
Quindi ci interesserà osservare come il comportamento locale di una entità influenza i fenomeni globali
dell'intera rete.

L'analisi di una rete, nel nostro ambito d\'interesse, verrà fatta considerando numerosi aspetti, come per esempio le **prestazioni**, la **struttura** e il loro **utilizzo**,e verranno anche analizzate le **tecniche** impiegate nell'analisi stessa.
Alcuni aspetti strutturali che sono interessanti di analizzare sono:

## Fenomeno di diffusione

In questo esempio verrà mostrato come i legami influiscono sul comportamento dei singoli individui. Supponiamo di giocare a una variante probabilistica del gioco del telefono in cui siamo disposti tutti in cerchio, e un volta ricevuto il messaggio lo passo solo se lanciando una moneta esce testa. La moneta è la stessa per tutti, però è **non** equa, ovvero esce testa con probabilità $p = 0.6$ e croce con probabilità $1-p = 0.4$. Se il cerchio è molto grande, è davvero molto poco probabile che il messaggio arrivi a un numero elevato di persone.

Se invece per ogni persona nel cerchio associassimo un terzo "vicino" totalmente a caso a cui poter trasmettere il messaggio, succederà che *molto probabilmente* il messaggio arriverà a una grossa porzione di
persone in poco tempo.

Questo esempio può essere paragonato al fenomeno della diffusione di un'infezione, in cui la probabilità di infettare un vicino non infetto (o che non è stato infettato in precedenza) è $p$ (vedi [qui](https://arxiv.org/abs/2103.16398)).

## Fenomeno di stabilità
Un classico esempio in cui invece la presenza di legami influisce sulla struttura della rete è il seguente: consideriamo di avere una rete sociale in cui *Bob* è amico di *Alice* e di *Carol*, ma *Alice* e *Carol* non si sopportano (perché entrambe segretamente innamorate di *Bob*).

Questa rete è detta **instabile**, in quanto per *Bob* sarà difficile uscire con entrambe. Dovendo scegliere per forza una delle due con la quale passare più tempo, necessariamente il legame con l'altra tenderà a rompersi, rendendo così la rete più stabile. (Proprio non vorrei essere nei panni di *Bob*...).

------------------------------------------------------------------------

# Struttura di una Rete
Generalmente si è interessati a studiare reti di dimensioni enormi, perciò diventa estremamente complicato studiare ed analizzare ciò che accade ad ogni singolo individuo. Infatti ciò che si studia sono delle **proprietà globali** della rete.
Esempi di proprietà globali interessanti sono

1.  esistenza di **componenti giganti**, ovvero se in un rete esiste una [componente fortemente connessa](https://en.wikipedia.org/wiki/Strongly_connected_component) molto grande (ovvero paragonabile alla grandezza della rete stessa).
    Un esperimento famoso a riguardo fu quello del 2006 condotto da *Leskovec & Horvitz* in cui presero i dati (in maniera anonima?) delle comunicazioni fatte in un mese da 240 milioni di utenti di Instant Messenger. Su questi dati costruirono un grafo in cui i nodi erano i 240M di utenti, e inserirono un arco tra due nodi se i rispettivi utenti si scambiarono almeno un messaggio in quel mese. Risultò fuori l\'esistenza di una componente fortemente connessa di circa 200 milioni di utenti.
2.  presenza di componenti (o porzioni di esse) **densamente connesse** (ovvero con un elevato numero di connessioni)
3.  verificare se la struttura della rete è di tipo **centro/periferica**
4.  verificare se la rete ha un **diametro** piccolo nonostante sia poco denso.

Riguardo quest'ultimo punto è di dovere citare il famoso esperimento dello psicologo Stanley Milgram sui *"6 gradi di separazione"*.
In pratica supponiamo di avere una lettera di volerla recapitare al presidente degli stati uniti. Sappiamo esattamente a chi farla arrivare, ma non come.
Supponiamo di dare la lettera in mano al nostro conoscente che secondo noi è più probabile che la faccia arrivare al presidente, e diciamogli di fare lo stesso.
Mediamente, tramite solo 5 intermediari, la lettera arriverà al presidente Joe Biden.

Lo studio di queste proprietà globali sarà fatto sia a *livello di popolazione*, ovvero considerando mediamente ciò che succede agli individui, sia a *livello di singolo individui*, andando a considerare la topologia della rete (relazione per relazione).

------------------------------------------------------------------------

# Rappresentazione di una rete
Come si è potuto facilmente intuire in precedenza, il modo in cui verrà raffigurata una rete sarà tramite il modello matematico di *Grafo*.
Si assume che i concetti e le definizioni base della teoria dei grafi siano ben note al lettore (se così non fosse, questi appunti non fanno per te... e nemmeno questo corso!).

Supponiamo quindi di riuscire ad avere le informazioni di una rete reale (cosa che all\'atto pratico non accade molto spesso), si potrebbe modellare semplicemente associando un nodo ad ogni entità della rete, e un arco per ogni interconnessione (facile no?).

Fatto ciò si può "tranquillamente" procedere con l'analisi, alla ricerca di tutte le proprietà viste nella [[#Struttura di una Rete|precedente sezione]].

Però, ammettendo pure di avere a disposizione questi dati, è difficile essere interessati a studiare le proprietà di una singola rete.
Generalmente si è interessati a sapere proprietà **generali**, come per esempio qual è il diametro medio di una rete espresso in funzione del numero delle sue entità, oppure dato il grado medio dei nodi quanto sono grandi (mediamente) le componenti connesse.

Per fare ciò bisognerebbe osservare un gran numero di reti!
E in un contesto reale non si hanno così tante informazioni; un po' perché generalmente chi è in possesso di questi dati così preziosi difficilmente li condivide col mondo, un po' perché di reti reali non ne esistono abbastanza da poter consentire di fare un\'analisi statistica.

Perciò quello che si fa generalmente è *inventare* nuovi modelli di grafi che rispecchino il più possibile le proprietà che una rete reale possiede.
Ovviamente non è possibile creare un modo che abbia **tutte** le proprietà di una rete reale, per via del grado di complessità delle ultime.
Però in base al fenomeno che si desidera studiare, esistono dei modelli che presentano alcune proprietà abbastanza utili ai fini analitici.

Perciò, una volta creato un modello, si possono **campionare** (o **istanziare**) quanti grafi si necessita.

------------------------------------

# Il modello Erdős-Rényi
Uno dei modelli più semplici e classici della letteratura è il cosiddetto *modello **Erdős-Rényi***, noto anche come *Erdős-Rényi random graph*.
Tale modello è definito come segue:
siano $n \in \mathbb{N}$ e $p \in \left[ 0, 1 \right]$, $G_{n,p}$ è un grafo tale che:
-   $V = \left[ n \right] \equiv \{1,  2, ..., n \}$
-   $E \subseteq \binom{ V }{ 2 } : \forall u,v \in V \; \left[ \mathcal{P}( \{u, v\} \in E) = p \right]$

Come si può notare tale modello è un **modello aleatorio**, da cui il nome *random graph*.
Se ci si sofferma, l\'insieme di archi $E$ segue una [distribuzione binomiale](https://it.wikipedia.org/wiki/Distribuzione_binomiale), perciò è corretto parlare anche di **distribuzione Erdős-Rényi**.

## Componenti Giganti
Una definizione più precisa di **componente gigante** equivale a una **componente connessa** grande una **frazione costante** dei nodi del grafo.

Per frazione costante, si intende che al variare del numero di nodi del grafo, il rapporto tra $n$ e la grandezza della componente gigante rimane invariato.
Si dice quindi che una componente gigante ha grandezza $\Theta(n)$, dove (ribadendo) $n$ è il numero di nodi del grafo.

Per quanto riguarda la distribuzione *distribuzione Erdős-Rényi* ci si potrebbe chiedere se esiste un valore di $p$ per cui esiste (con buona probabilità) una componente gigante in $G_{n,p}$, e se sì, per quale valore.

Un risultato già noto dice che se $p > \frac{ \ln{64} }{ n }$ allora con **alta probabilità** (in breve **w.h.p.**) esisterà una componente connessa con **almeno** la metà dei nodi.

Il termine *"alta probabilità"* non è usato in maniera impropria, bensì cela un significato rigoroso e preciso. Per alta probabilità si intende con probabilità dell\'ordine di <u>almeno</u> $\left( 1 - \frac{ b }{ n^c } \right)$, per qualche coppia di costanti
$b,c$ strettamente positive.

Da cui quindi il seguente teorema:

> **THM** sia $G_{n,p}$ un Erdős--Rényi random graph di parametri $n,p$, e sia $X$ la *variabile aleatori* che indica il numero di nodi della più grande componente connessa di $G_{n,p}$.
> $$p > \frac{ \ln{64} }{ n } \implies \mathcal{P} \left( X \geq \frac{n}{2} \right) \geq 1 - 2^{-n/8}$$

Per dimostrare il teorema è prima necessario introdurre il seguente lemma

> **Lemma** Se $X < \frac{n}{2}$ allora esiste un insieme $A \subset \left[ n \right]$, detto ***insieme buono***, tale che
> $$ \frac{n}{4} \leq |A| <    \frac{3}{4}n $$ e
> $$ \nexists \{ u,v \} \in E : u \in A \land v \in [n] \setminus A $$
> **Proof (Lemma)** Siano $C_1, C_2, ..., C_k$ tutte le componenti connesse di $G_{n,p}$ ordinate in senso non decrescente di grandezza, ovvero $$ |C_1| \leq |C_2| \leq ... \leq |C_k| := X < \frac{n}{2} $$
> Si scelga un indice $h$ tale che
> $$ |C_1| + |C_2| + ... |C_{h-1}| < \frac{n}{4} $$ e
> $$ |C_1| + |C_2| + ... |C_{h-1}| + |C_h| \geq \frac{n}{4} $$Si osservi che necessariamente $h < k$.
> Infatti poiché $|C_k| < \frac{n}{2}$, se fosse $h=k$ risulterebbe
> $$ |C_1| + |C_2| + ... |C_{k-1} | + |C_k | < \frac{n}{4} + \frac{n}{2} < n $$
> e questo è assurdo in quanto $C_1, C_2, ..., C_k$ sono una **partizione** di $V$, perciò la dimensione della loro unione è esattamente $n$.
> 
> Scelto un $h$ adeguato, poniamo $A = \bigcup_{i=1}^{h} C_h$.
> Necessariamente $A \neq \emptyset$ in quanto è composto da almeno $C_1$ (il quale ha almeno un nodo per definizione di componente), e $\left[ n \right] \setminus A \neq \emptyset$ in quanto al più $h$ può valere $k-1$ (lasciando fuori $C_k$ dall'insieme $A$).
> 
> Seguono quindi le condizioni del lemma
> -  $|A| \geq \frac{n}{4}$ per costruzione
> -   $|A| = \underbrace{|C_1| + |C_2| + ... + |C_{k-1}|}_{ < n/4 } + \underbrace{|C_k|}_{< n/2} < \frac{n}{4} + \frac{n}{2} = \frac{3}{4}n$
> -   Non ci possono essere archi nel taglio $(A,\left[ n \right] \setminus A)$, in quanto i due insiemi sono composti da componenti connesse.
> 	Infatti se esistesse un arco nel taglio, i due estremi apparterrebbero a due componenti connesse $C_i, C_j$ distinte, ma ciò significherebbe che $C_i \cup C_j$ è a sua volta una componente connessa (**assurdo!**). $\square$

Una diretta conseguenza del lemma è che la probabilità che la più grande componente connessa di $G_{n,p}$ sia più piccola di $n/2$ è **minore o uguale** alla probabilità sia presente un *insieme buono*.
In fatti, in probabilità, dati due eventi $\mathcal{E_1},\mathcal{E_2}$, se $\mathcal{E_1} \implies \mathcal{E_2}$ allora $\mathcal{P}(\mathcal{E_1}) \leq \mathcal{P}(\mathcal{E_2})$.

In questo caso gli eventi corrispondono a
- $\mathcal{E_1}$ = "$X < n/2$".
- $\mathcal{E_2}$ = "esiste un insieme buono $A$".

> **Proof (THM)** Si desidera calcolare $\mathcal{P}\left( X < \frac{n}{2} \right)$, ovvero il complementare della probabilità che si desidera dimostrare.
> 
> Dal **Lemma** avremo che
> 
$$\begin{align*}
	\mathcal{P}\left( X < \frac{n}{2} \right)
   &\leq \mathcal{P}( \exists A \subset \left[ n \right] : A \mbox{ è buono})\\
   \mbox{(per def. di insieme buono)} &= \mathcal{P}\left( \bigcup_{A \subset \left[ n \right] \land \frac{n}{4} \leq |A| < \frac{3}{4}n }  A : \mbox{ non ci sono archi nel taglio } (A, \overline{A}) \right)\\
   \mbox{(union bound)} &\leq \sum_{A \subset \left[ n \right] \land \frac{n}{4} \leq |A| < \frac{3}{4}n } \mathcal{P}\left(\mbox{ non ci sono archi nel taglio } (A, \overline{A}) \right)\\
   (1) &= \sum_{A \subset \left[ n \right] \land \frac{n}{4} \leq |A| < \frac{3}{4}n } (1-p)^{|A| \cdot |\overline{A}|}\\
   (2) &\leq \sum_{A \subset \left[ n \right] \land \frac{n}{4} \leq |A| < \frac{3}{4}n } (1-p)^{\frac{3}{16}n^2} \\
   (3) &\leq 2^{n} (1-p)^{\frac{3}{16}n^2}\\
   (4) &< 2^{n} \left(1 - \frac{ \ln{64} }{n} \right)^{\frac{3}{16}n^2}\\
   &= 2^{n} \left[ \left(1- \frac{ \ln{64} }{n} \right)^n \right]^{\frac{3}{16}n}\\
   &= 2^{n} \left[ \left(1- \frac{ \ln{64} }{n} \right)^{n \cdot \left( \frac{ -\ln{64} }{ -\ln{64} }  \right)  } \right]^{\frac{3}{16}n}\\
   (5) &\approx 2^{n} \left[ e^{-\ln{64}}  \right]^{\frac{3}{16}n}\\
   &= 2^{n} \left[ 64^{-1}  \right]^{\frac{3}{16}n}\\
   &= 2^{n} \left[ 2^{-6}  \right]^{\frac{3}{16}n} = 2^{n} 2^{ -\frac{18}{16}n } = 2^{-\frac{n}{8}} \;\;\; \square\end{align*}$$
> Di seguito spiegate le disuguaglianze:
>
> -   **(1)** è data dal fatto che tale probabilità è data dalla probabilità che **non** esista un arco nel taglio, ovvero $(1-p)$, moltiplicato tante volte quanti i archi possono esistere tra $A$ e $\overline{A}$, ovvero $|A| \cdot |\overline{A}|$.
> -   **(2)** è invece data dal fatto chef il punto di *minimo* della funzione $|A| \cdot |\overline{A}|$ è per valori di $|A| = \frac{1}{4}, \frac{3}{4}$. Ci interessano i punti di minimo in quanto essendo $(1-p) < 1$, $(1-p)^{x}$ assume valori massimi per $x$ minimo.
> -   **(3)** è dato dal fatto che il numero *massimo* di sottoinsiemi $A \subseteq \left[ n \right]$ è $2^n$.
> -   **(4)** per ipotesi del teorema
> -   **(5)** perché $\lim_{n \rightarrow \infty} \left( 1 + \frac{1}{n} \right) = e^{n}$

Una versione più generale del precedente teorema è la seguente

> **Teorema generale** Sia $G_{n,p}$ un grafo aleatorio campionato dalla *distribuzione Erdős-Rényi*, allora:
>
> 1.  se $p(n-1) < 1$ allora *quasi sicuramente* (*asymptotically almost surely*, o in breve *a.a.s.*) tutte le componenti connesse di $G_{n,p}$ avranno $O( \log{n} )$ nodi (ovvero saranno molto piccole).
> 2.  se $p(n-1) = 1$ allora *a.a.s.* $G_{n,p}$ avrà una componente connessa grande $\approx n^{ \frac{2}{3} }$.
> 3.  se $p(n-1) > 1$ allora *a.a.s.* $G_{n,p}$ avrà una componente connessa grande $\Omega (n)$ nodi e tutte le altre grandi $O(\log{n})$.

Ciò che suggerisce il teorema più generale è che la presenza di componenti connesse dipende in qualche modo dal prodotto $p(n-1)$, ma cosa rappresenta questa quantità?
Questo verrà mostrato nella seguente sezione.

## Grado dei nodi
Dato un grafo *Erdős-Rényi* $G_{n,p}$, definiamo col simbolo $\delta_i$ la *variabile aleatoria* che esprime il **grado** del nodo $i$.
Sia inoltre la v.a. binaria tale che $\forall j \in \left[ n \right] \setminus \lbrace i \rbrace$ $$
    X_{i,j} = \begin{cases}
    1 &\mbox{se esiste l'arco }(j,i) \in E\\
    0 &\mbox{altrimenti}
    \end{cases}
    $$ Perciò il [valore atteso](https://it.wikipedia.org/wiki/Valore_atteso) di $\delta_i$ sarà

$$\begin{align*}
      \mathbb{E} \left[ \delta_i  \right]
      &= \mathbb{E} \left[ \sum_{j \in \left[ n \right] \setminus \lbrace i \rbrace } X_{j,i} \right]\\
      \mbox{(per linearità)} &= \sum_{j \in \left[ n \right] \setminus \lbrace i \rbrace }  \mathbb{E} \left[  X_{j,i} \right]\\
      &= \sum_{j \in \left[ n \right] \setminus \lbrace i \rbrace } 1 \cdot p + 0 \cdot (1-p) = (n-1)p
\end{align*}$$

Perciò la quantità $p(n-1)$ vista nella precedente sezione, altro non è che il *grado medio* di un nodo.

Ciò significa che, fissato un $p$ **costante**, ogni individuo nella rete sarà in contatto con una porzione constante della reta stessa, ovvero il grado medio cresce in maniera *lineare nel numero di nodi*!

Ovviamente è inverosimile che in una rete di un social network composta da centinaia di milioni (se non miliardi) di nodi, io abbia un numero di amicizie proporzionale al numero di utenti (non penso sarà mai così famoso).

Perciò, per modellare una rete che sia più verosimile, si potrebbe volere che $p$ sia una funzione di $n$, e che magari *decresca* in maniera inversamente proporzionale, del tipo
$$ p = p(n) = \frac{ \lambda }{n} $$Scegliendo $p$ in questa maniera, avremo che il grado medio di un nodo sarà pari alla **costante** $\lambda$, indipendentemente dalla dimensione della rete.

Un'altra domanda che è utile chiedersi è: fissato un $k \leq n$, con quale probabilità un nodo $i \in \left[ n \right]$ ha **esattamente** grado $k$?

Intuitivamente, questa è pari alla probabilità che esistano esattamente $k$ archi incidenti a $i$.
Tale probabilità è esattamente definita dalla [legge binomiale](https://it.wikipedia.org/wiki/Distribuzione_binomiale) $\mathcal{B}(n-1,p)$, ovvero $$
    \mathcal{P}\left( \delta_i = k \right) = \binom{n-1}{k} p^k \left( 1-p \right)^{(n-1) - k}
    $$ Questo perché:
1.  $\binom{n-1}{k}$ è il numero di tutte le possibili $k$-uple di nodi in $\left[ n \right] \setminus \lbrace i \rbrace$.
2.  $p^k$ è la probabilità che esistano gli archi tra $i$ e i nodi di una $k$ -upla.
3.  $\left( 1-p \right)^{(n-1) - k}$ è la probabilità che non esista nessun altro arco (ne vogliamo solo $k$).

Scegliendo $p = \frac{ \lambda }{n}$ in funzione di $n$ come detto in precedenza, avremo che la probabilità che un nodo abbia grado medio esattamente $k$ è

$$
\begin{align*}
      \mathcal{P}\left( \delta_i = k \right)
      &= \binom{n-1}{k} p^k \left( 1-p \right)^{(n-1) - k}\\
      &= \binom{n-1}{k} \left(  \frac{ \lambda }{n} \right)^k \left( 1-  \frac{ \lambda }{n} \right)^{(n-1) - k}\\
      &= \frac{(n-1) \cdot (n-2) \cdot ... \cdot (n-k)}{k!} \frac{ \lambda^k }{ n^k } \left( 1-  \frac{ \lambda }{n} \right)^{(n-1) - k}\\
      (1) &< \frac{(n-1) \cdot (n-2) \cdot ... \cdot (n-k)}{k!} \frac{ \lambda^k }{ n^k } e^{ \frac{-\lambda (n - 1 - k)}{n}} \\
      &< \frac{n^k}{k!} \frac{ \lambda^k }{ n^k } e^{ \frac{-\lambda (n - 1 - k)}{n}}\\
      &\approx \frac{ \lambda^k }{ k! } e^{ -\lambda } \;\;\; \mbox{per } n \mbox{ sufficientemente grande}
\end{align*}
$$

Più semplicemente
$$ \mathcal{P}\left( \delta_i = k \right) < \frac{ \lambda^k }{ k! } e^{ -\lambda } $$
La disuguaglianza **(1)** è data dal fatto che $\forall x < 1$ vale sempre che $1-x < e^{-x}$.

Applicando l\'[approssimazione di Stirling](https://it.wikipedia.org/wiki/Approssimazione_di_Stirling) secondo la quale $k! \thicksim \sqrt{2 \pi k} \left( \frac{k}{e}  \right)^k$, possiamo semplificare il tutto con $$
    \mathcal{P} \left( \delta_i = k \right) < \frac{ (\lambda e)^k }{ \sqrt{2 \pi k} k^k }e^{ -\lambda }
    = \frac{ e^{ -\lambda } }{ \sqrt{2 \pi k} } \left( \frac{ \lambda e }{k}  \right)^k
    $$
Quest'ultima disuguaglianza ci suggerisce che la probabilità che un generico nodo abbia grado $k$ *decresce* molto velocemente al crescere di $k$, più precisamente decresce *esponenzialmente* in $k$ (come $k^{-k}$).

Una volta stabilità questa probabilità, si può calcolare il numero atteso di nodi che hanno grado esattamente $k$.
Per prima cosa definiamo la variabile aleatoria binaria $\delta_{i,k}$ tale che $$
    \delta_{i,k} = \begin{cases}
    1 &\mbox{se } \delta_i = k\\
    0 &\mbox{altrimenti}
    \end{cases}
    $$
Definiamo in oltre la v.a. $F_k$ che indica la *frazione* di nodi della rete che hanno grado $k$
$$ F_k = \frac{ \sum_{i \in \left[ n \right] } \delta_{i,k}  }{n} $$
con valore atteso
$$
\begin{align*}
      \mathbb{E} \left[ F_k \right]
      &= \mathbb{E} \left[ \frac{ \sum_{i \in \left[ n \right] } \delta_{i,k}  }{n} \right]\\
      &= \frac{1}{n} \sum_{i \in \left[ n \right] } \mathbb{E} \left[ \delta_{i,k} \right]\\
      &= \frac{1}{n} \sum_{i \in \left[ n \right] } \left( 1 \cdot \mathcal{p}(\delta_{i,k} = 1) + 0 \cdot \mathcal{p}(\delta_{i,k} = 0) \right)\\
      &= \frac{1}{n} \sum_{i \in \left[ n \right] } \left( 1 \cdot \mathcal{p}(\delta_i = k) + 0 \cdot \mathcal{p}(\delta_i \neq k) \right)\\
      &= \frac{1}{n} \sum_{i \in \left[ n \right] } \mathcal{p}(\delta_i = k)\\
      &< \frac{1}{n} \sum_{i \in \left[ n \right] } \frac{ e^{ -\lambda } }{ \sqrt{2 \pi k} } \left( \frac{ \lambda e }{k}  \right)^k\\
      &= \frac{ e^{ -\lambda } }{ \sqrt{2 \pi k} } \left( \frac{ \lambda e }{k}  \right)^k
\end{align*}
$$

Perciò, in un grafo *Erdős--Rényi* $G_{n,p}$ con grado medio constante, ovvero con parametro $p = \frac{\lambda}{n}$, il numero di nodi di grado esattamente $k$ decresce esponenzialmente in $k$ (come $k^{-k}$).