# Instance-Optimal Geometric Algorithms

## The 2D Maxima problem
L'input del **2D Maxima problem** sono $n$ punti su un piano <u>bidimensionale</u>.
Assumiamo per semplicità che i punti hanno tutti coordinate distinte (in realtà non è necessario).

Diremo che il punto $p$ è **dominato** dal punto $q$ se $q$ ha entrambe le coordinate magiori rispetto a quelle del punto $p$ (ovvero $q$ si trova nel quadrante a nord-est rispetto a $p$).

Un punto $q$ è detto **massimale** se nessun altro punto lo domina.
```ad-important
Cerca di visualizzare bene cosa significa che $q$ è **massimale** (nel senso appena descritto).
```

L'obiettivo del problema è, dato un input $Q \subset \mathbb{R}^2$ , trovare il sottoinsieme $S \subseteq Q$ composto da tutti e soli i **punti massimali** di $Q$.

```ad-important
Osservare che l'insieme di **punti massimali** genera una sorta di **regione di dominanza**, nella quale sono presenti **tutti** gli altri punti che non sono massimali.

Tale regione è sempre a forma di *"scala"*.

![|600](BWA_02_1.png)

```

Come [[1 - Introduction#^db5c4b|misura di qualità]] $\text{cost}(A,z)$ il numero di **operazioni di confronto** che necessita l'algoritmo $A$ per computare la soluzione, dato l'input $z$.

Il nostro obiettivo è quello di dimostrare che esiste un algoritmo $A$ che è [[1 - Introduction#^67367e|instance-optimal]] per il *2D Maxima problem*.

-------------
# The Kirkpatrick-Seidel Algorithm
L'algoritmo di *Kirkpatrick & Seidel* (in breve **KS**) risolve **correttamente** il [[#The 2D Maxima problem|2D Maxima Problem]]

> **KS Algorithm**
> - **Input:** $Q \subset \mathbb{R}^2$
> 	1. if $Q \equiv \emptyset$ return; if $|Q| = 1$ return $Q$;
> 	2. Trova il punto **mediano** $m$ rispetto alle coordinate $x$;
> 	3. Dividi $Q$ in due parti $Q_L$ e $Q_R$, prendendo $m$ come *pivot*;
> 	4. Sia $q$ il punto con coordinata $y$ massima in $Q_R$;
> 	5. Aggiungi $q$ nella soluzione $S$;
> 	6. Rimuovi $q$ e tutti i punti che esso domina da $Q_L$ e $Q_R$.
> 	7. Richiama ricorsivamente l'algoritmo su $Q_L$ e $Q_R$, e fondi le sottosoluzioni con $S$.

```julia
struct Point
	x::Real
	y::Real
end

dominate(a::Point, b::Point)::Bool = a.x ≥ b.x && a.y ≥ b.y

function KS(Q::AbstracVector{Point})::AbstractVector{Point}
	length(Q) ≤ 1 && return Q
	
	m = median(Q)
	
	Q₁ = filter(p -> p.x ≤ m.x, Q)
	Q₂ = filter(p -> p.x > m.x, Q)
	
	q = Q₂[findmax(p -> p.y, Q₂)[2]]
	
	S = [q]
	
	Q₁ = filter(p -> !dominate(q, p), Q₁)
	Q₂ = filter(p -> !dominate(q, p), Q₂)
	
	append!(S, KS(Q₁), KW(Q₂))
end
```

## Correttezza
Innanzitutto osserviamo che tutti i punti che vengono rimossi sono **dominati** da un punto che apparterrà alla soluzione $S$, perciò sono giustificati.

In secondo luogo osserviamo che il punto $q$ trovato ad ogni chiamata ricorsiva è **massimale** (ovvero nessun altro punto lo domina).
Infatti tutti i punti in $Q_L$ hanno coordinata $x$ strattamente minore, mentre tutti quelli in $Q_R$ hanno coordinata $y$ strettamente minore.

Nessun punto massimale potrà mai essere rimosso durante la ricorsione, perché se fosse vero vorrebbe dire che alla ricorsione precedente è stato trovato un punto $q$ che lo domina (assurdo).

Non resta quindi che dimostra che nessun punto **non**-massimale venga trasformato in massimale a fronte delle eliminazioni fatte durante le ricorsioni.

Consideriamo una generica chiamata ricorsiva.
Il punto massimale $q$ trovato a tale iterazione è *sopravvissuto* a tutte le chiamate ricorsive precedenti, ciò vuol dire che in precedenza non è stato trovato nessun altro punto che lo dominasse (altrimenti sarebbe stato rimosso) $\square$.

## Running Time Analysis
### Worst-case bound
Trovare il punto mediano $m$ può essere fatto in **tempo lineare** nella grandezza di $Q$ (vedi [[median-point|appendice]]).
Una volta trovato $m$ possiamo paritizionare $Q$ in $Q_L$ e $Q_R$ ancora in **tempo lineare**.
Trovare il punto $q$ con coordinata $y$ massima in $Q_R$ può essere fatto in **tempo lineare** in $\vert Q_R \vert \leq \vert Q \vert$.
In fine, rimuovere tutti i punti *dominati* da $q$ richiede ancora **tempo lineare** in $\vert Q \vert$.

Dato che $Q_R$ e $Q_L$ sono suddivisi dal **punto mediano**, essi avranno entrambi dimensione $\leq \lceil n/2 \rceil$.
Avremo quindi la seguente **equazione di ricorrenza** $$T(n) \leq 2 \cdot T(n/2) + O(n)$$
Ovvero il running time nel caso peggiore sarà $O(n\log{n})$.

### More refined bound
È possibile dimostrare che nel caso peggiore il running time dell'algoritmo KS è $\Omega(n\log{n})$ ([[vedi esercizio]]).
Però è facile convincersi che esistono istanze che risultano essere più *"facili"* da risolvere:
per esempio potrebbe capitare un'istanza in cui esiste <u>un solo</b> massimale il quale elimina tutti gli altri punti.

![|500](BWA_02_2.png) ^f8ce54

Nell'istanza in [[#^f8ce54|esempio]] il running time risulterà essere $O(n) \in o(n\log{n})$.

Ricordiamo che stiamo cercando un upper-bound che è il più stretto possibile al variare di ogni input, mentre $O(n\log{n})$ è troppo generale e va bene solamente nelle istanze più *"sfortunate"* (abbiamo infatti appena visto che nel caso fortunato abbiamo un upper-bound di $O(n)$).

Parametrizzare l'efficienza **solamente** rispetto alla grandezza $n$ dell'istanza non è sufficiente.
L'idea è di parametrizzare anche rispetto al numero $h$ dei punti **massimali** per ogni istanza.

Infatti, mentre l'upper-bound nel worst-case dipende solo dall'algoritmo, parametrizzare rispetto alla dimension $h$ dell'output dipenderà anche dalle singole istanze.
In questo caso si dice che abbiamo un **output-sensitive** time bound.

Dato che ogni chiamata ricorsiva genera altre due chiamate ricorsive, avremo un albero delle ricorsioni che cresce esponenzialmente ad ogni livello.
Dato che ogni chiamata identifica un nouvo punto massimale, allora avremo che al generico livello $j$ verranno identificati $2^j$ nuovi punti massimali.
Perciò avremo un albero con altezza $\log_2{h}$, inducendo quindi in nuovo upper-bound (*output-sensitice*) di $O(n\log{h})$.

Purtroppo tale bound non propriamente corretto, in quanto molte chiamate ricorsive potrebbero creare una sola, o nessuna, altra chiamata ricorsiva a loro volta.
Quindi potremmo avere un albero che non è 2-regole (o quasi), eccedendo (anche di molto) l'altezza di $\log{h}$.

Perciò $O(n\log{h})$ non è ancora un buon bound.

### A Still More Refined Bound
Per provare l'*instace optimality* dell'algoritmo KS, abbiamo bisogno in un bound ancora più stretto (sempre *input-by-input*).

Supponiamo di poter **partizionare** l'input $Q$ nei sottoinsiemi $S_1, ..., S_k$, tali che per ogni $i$:
1. $S_i$ è composto da un singolo punto.
2. esiste una rettangon $B_i$ tale che contiene **tutti** i punti di $S_i$ e tale che è **strettamente sotto** la regione a di dominanza *"a scala"* (vedi [[#^f40f06|figura]])


![|550](BWA_02_3.png) ^f40f06

Ci riferiremo d'ora in avanti a tale partizione come **partizione legale**.

L'idea intuitiva è che ogni insieme $S_i$ del **secondo tipo** rappresenta il cluster di punti che a una certa ricorsione dell'algoritmo vengono eliminati in un **solo colpo**.
Più grande è $S_i$, più *"facile"* risulterà l'istanza.

> **Teorema**
> Per ogni possibile input $Q$, il running time dell'algoritmo KS è $$O\left( \min_{\text{legal } \{S_i\}} \sum_{i=1}^{k} \vert S_i \vert \log{\frac{n}{\vert S_i \vert}} \right)$$

^3112de

Tale teorema mostra un upper-bound con una *grana molot fine* per il running time dell'algoritmo KS, parametrizzato dalla *"facilità"* dell'istanza (ovvero dalla partizione legale $\{S_i\}$).

> **Proof**
> Fissiamo una generica **partizione legale** $S_1, ..., S_k$.
> L'idea è quella di trovare un bound al contributo (in termini di confronti effettuati) su ogni $S_i$ separatamente, per poi sommarli tutti.
> 
> Fissiamo quindi un $S_i$.
> Il contributo dato da $S_i$ al livello $j$-esimo della ricorsione è **lineare** nel numero di elementi in $S_i$ (che non sono già stati rimossi in precedenza).
> 
> Un bound banale al numero di punti presenti in $S_i$ al livello $j$ della ricorsione è $\leq \vert S_i \vert$.
> Purtroppo tale bound è ancora troppo poco preciso per fare un'analisi più fine.
> Un bound più preciso è dato dal seguente klaim
> > **Key claim**
> > Sia $S_i^{(j)}$ l'isneme di punti di $S_i$ che sopravvivono fino al livello $j$ dell'albero della ricorsione.
> > Allora avremo che $$\vert S_i^{(j)} \vert \leq O(n/2^j)$$
> 
> Procediamo con la dimostrazione assumendo vero il **key claim**.
> Osserviamo che, nei primi livelli della ricorsione, un upper-boud migliore a $\vert S_i^{(j)} \vert$ è $\vert S_i \vert$ (ovvero nel caso sfortunato in cui vengono rimossi pochissimi punti nei primi livelli).
> Dopodiché, ci sarà un certo livello $j$ dal quale, di li in poi, vale il bound $O(n/2^j)$ (ovvero $\vert S_i \vert$ si inizia a dimezzare).
> 
> Perciò, avremo che esiste un $j$ tale che per i primi $j$ livelli avremo un buond di $\approx \vert S_i \vert$, e dal livello $j$ in poi avremo il bound $\approx \vert S_i \vert / 2 \leq n/2^j$, poi $\vert S_i \vert / 4 \leq n/2^{j+1}$, poi $\vert S_i \vert / 8 \leq n/2^{j+2}$, e così via.
> 
> Secondo questo bound, avremo che $\vert S_i \vert /2 \approx  n/2^j$ quando $j \approx \log_2{n/\vert S_i \vert}$.
> 
> In conclusione avremo che il contributo (in termini di operazioni di confronto) fatte dal singolo insieme $S_i$ sarà
> $$\begin{align*}
\vert S_i \vert + \vert S_i^{(2)} \vert + \vert S_i^{(3)} \vert + ...
&\leq \underbrace{\vert S_i \vert + \vert S_i \vert + ... + \vert S_i \vert}_{\approx \log{(n/\vert S_i \vert})\text{ times}} + \frac{ \vert S_i \vert}{2} + \frac{ \vert S_i \vert}{4} + \frac{ \vert S_i \vert}{8} +  ...\\
&\leq \vert S_i \vert \log{\frac{n}{\vert S_i \vert}} + \vert S_i \vert\sum_{\ell=1}^{\infty}\frac{1}{2^\ell}\\
&= \vert S_i \vert \log{\frac{n}{\vert S_i \vert}} + 2\vert S_i \vert\\
&\in O\left(\vert S_i \vert \log{\frac{n}{\vert S_i \vert}}\right) \;\; \square
\end{align*}$$

> **Proof of the Key claim**
> Fissiamo un generico livello $j$ della ricorsione, e suddividiamo la dimostrazione in due parti:
> 1. Tutti i punti di $S_i$ rimasti (ovvero $S_i^{(j)}$) sono compresi (rispetto all'asse delle $x$) tra due punti **massimali** $p_L,p_R$ **adiacenti** e **trovati entro il livello** $j$.
> 2. Per ogni coppia di punti **massimali** $p_L,p_R$ **adiacenti** e **trovati entro il livello** $j$, ci sono solamente $O(n/2^j)$ punti sopravvissuti compresi tra $p_L$ e $p_R$ (rispetto alla coordinata $x$).
> 
> Siano $a$ e $b$ le coordinate $x$ dei punti $p_L$ e $p_R$ (rispettivamente).
> Nel caso in cui $p_L$ o $p_R$ non esistano (nei margini) poniamo $a = -\infty$ o $b = \infty$.
> Per definizione, tutti i punti in $S_i^{(j)}$ hanno coordinate $x$ **minori** di $b$ (pensa come funzione l'algoritmo).
> Sicuramente avremo anche che tutti i punti in $S_i$ con coordinate $x$ **minori** di $a$ saranno stati *rimossi* dall'algoritmo quando il punto $q_L$ è stato identificato come massimo.
> Perciò avremo anche che in $S_i^{(j)}$ tutti i punti hanno coordinate $x$ **maggiori** di $a$, provando quindi il punto (1).
> 
> ![|500](BWA_02_5.png)
> 
> Per quanto riguarda il punto (2), ricordiamo che l'algoritmo **dimezza** la grandezza dell'istanza ad ogni iterzaione, in quanto $Q$ viene partizionato rispetto al punto **mediano**.
> Perciò le $\leq 2^j$ chiamate ricorsive al livello $j$ (al più perché alcune chiamate possono essere vuote) sono composte da $\leq 2^j$ sottoinsiemdi di $Q$ (partizionati rispetto all'asse $x$), composti da $\leq n/2^j$ punti ciascuno (al più perché potrebbero essere rimossi molti punti strada facendo).
> 
> ![](BWA_02_4.png)
> 
> Una volta identificati i punti **massimali** per ognuno di questi sottoinsiemi, osserviamo che tra un massimale e un altro possono esserci **al più** $2 \cdot n/2^j \in O(n/2^j)$ punti che sopravvivono.
> Ciò implica che $\vert S_i^{(j)} \vert \in O(n/2^j)$ $\square$.

-------------------
# Instance Optimality of the KS Algorithm
Il [[#^3112de|precedente teorema]] propone un upper-bound parametrizzato e molto raffinato al running time dell'algoritmo KS.
Per dimostrare ora che KS è [[1 - Introduction#^67367e|instance optimal]]  per il problema del *2D Maxima*, equivale al dimostrare che per ogni input $Q \subset \mathbb{R}^2$, e per ogni altro algoritmo (*corretto*) $B$ avremo
$$\text{cost}(B, Q) \in \Omega\left( \min_{\text{legal } \{S_i\}} \sum_{i=1}^{k} \vert S_i \vert \log{\frac{n}{\vert S_i \vert}} \right)$$

Purtroppo però è facile mostrare un esempio in cui tale relazione non è vera.
Un algoritmo può **memorizzare** (in maniera *hard-coded*) le soluzioni per alcune istanze (magari già risolte in precedenza).
Periò un algoritmo che sfrutta tale tecnica può essere:
1. Controlla se la soluzione per l'input $Q$ è **memorizzata** (time $O(n)$);
2. Se si, ritorna la soluzione *hard-coded* $S$ (time $O(1)$);
3. Altrimenti, risolvi l'istanza con l'algoritmo che preferisci (per esempio KS).

Questo algoritmo è corretto, è in certi casi è anche asintoticamente migliore di KS.
Anche se potrebbe scoraggiarci nel continuare verso questa strada, un ragionamente invece incoraggiante è

> *Annoying counterexamples are not a good reason to abandon the quest for an interesting theorem*

In poche parole

> Se hai in mente un teorema, forse il modello o l'affermazione del teorema possono essere leggermente modificati per ottenere un risultato significativo?

Osserviamo che il nostro [[#^3112de|upper-bound]] al running time dell'algoritmo KS dipende **solamente** dall'insieme di punti in input $Q$, ed è **indipendente dall'ordine** in cui questi punti sono elencati (ricordando che per avere queste prestazioni $Q$ deve essere disposto in un **array**).

Pericò, riconsiderando l'algoritmo banale in questi termini, per verificare se $Q$ ha una soluzione gia memorizzata, saranno necessari $O(n\log{n})$ confronti anziché $O(n)$ (perché conviene ordinare $Q$ e la sequenza memorizzata, e poi verificare se sono uguali).

Ciò che quindi ci si aspetta è che le prestazioni di un algoritmo *"naturale"* per il problema sia **incurante dell'ordine**, nel senso appena discusso.
Mentre l'algoritmo banale, per avere efficienza $O(n)$ richiede che $Q$ sia già ordinato, perciò possiamo classificarlo come algoritmo *"non naturale"*.

Formuliamo quindi il seguente teorema

> **Teorema**
> Per ogni possibile input $Q \subset \mathbb{R}^2$ per il problema *2D maxima*, qualsiasi algoritmo *"naturale"* (o *"incurante dell'ordine"*) richiederà un numero di confronti dell'ordine di $$\Omega\left( \min_{\text{legal } \{S_i\}} \sum_{i=1}^{k} \vert S_i \vert \log{\frac{n}{\vert S_i \vert}} \right)$$

