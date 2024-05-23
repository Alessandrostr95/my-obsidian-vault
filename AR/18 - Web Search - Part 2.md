# Link Analysis and Web Search

## HITS: Hubs and Authorities
Ricapitolando, una volta individuato un sottoinsieme di pagine inerenti alla nostra ricerca, consideriamo maggiormente rilevanti quelle pagine che sono molto puntate (o **indicizzate**) all'interno dell'insieme.

Purtroppo però potrebbe capitare che una pagina venga puntata da tantissime pagine che in realtà sono molto poco rilevanti ai fini della ricerca.
Oppure ancora, un utente malevolo a scopi egoistici potrebbe creare tante pagine fittizie che puntano alle sue pagine per avere più rilevanza.
Perciò è utile porsi il seguente questio:

> *quando e quanto una pagina è **autorevole** nel conferire rilevanza ad un'altra pagina che punta?*

L'algoritmo **Hyperlink-Induced Topic Search** (o **HITS**, noto anche come **Hubs & Authorities**) proposto da [Jon Kleinberg](https://it.wikipedia.org/wiki/Jon_Kleinberg) consente di valutare la rilevanza delle pagine in funzione dei link che la puntano.

Iniziamo con l\'associare ad ogni pagina due **indici**:
- un indice di **autorità** che esprime la **rilevanza** della pagina ai fini della ricerca.
- un indice di **hub** che invece esprime l\'attitudine della pagina a **conferire autorità** alle pagine alle quali punta.

Per ogni pagina $i$ della rete indichiamo con $a_i$ il suo valore di **autorità**, come il numero di pagine (nell\'insieme di pagine attinenti alla ricerca) che puntano ad $i$.
$$a_i = \vert \{ j : j \mbox{ è attinente alla ricerca} \land j \rightarrow i \} \vert$$
Con $j \rightarrow i$ stiamo ad indicare che la pagina $j$ punta alla pagina $i$.

Assumiamo che a fronte di una ricerca otteniamo un sottoinsieme di $n$ pagine, indichiamo con $M$ la **matrice di adiacenza** del sottografo indotto dalle $n$ pagine inerenti alla ricerca.
$$M\left[i,j \right] = \begin{cases}
1 &\mbox{se } i \rightarrow j\\
0 &\mbox{altrimenti}
\end{cases}\;\;
\forall 1 \leq i < j \leq n$$
Definendo in questa maniera $M$ avremo che $$a_i = \sum_{j = 1}^{n} M\left[j,i \right]$$

Indichiamo invece con $h_i$ l'indice di hub di una pagina $i$.
Tale indice potremmo definirlo come il numero di pagine (sempre all'interno del sottoinsieme di nodi inerenti alla ricerca) alle quali $i$ punta.
Ovvero

$$\begin{align*}
h_i &= \vert \{ j : j \mbox{ è attinente alla ricerca} \land j \leftarrow i \} \vert\\
h_i &= \sum_{j = 1}^{n} M\left[i,j \right]
\end{align*}$$

Partiamo con l'osservare che possiamo *raffinare* i due indici appena definiti tramite le seguenti idee:
- il valore di autorità di una pagina $i$ dovrebbe essere tanto più eleveto quanto più sono **autorevoli** le pagine $j$ che la puntano.
- simmetricamente, il valore di hub di una pagina $i$ dovrebbe essere tanto più elevato quanto più è elevata la **rilevanza** delle pagine $j$ a cui punta.

Se in qualche modo ci venisse suggerito un valore di hub iniziale $h^{(0)}_i$ potremmo raffinare il valore $a_i$ visto in precedenza nella seguente maniera $$a^{(1)}_i = \sum_{j = 1}^{n} M\left[j,i \right] \cdot h^{(0)}_j$$
Ovvero l'autorità $a^{(1)}_i$ è influenzata dall\'autorevolezza delle $j$ che la puntano.

Dato che quindi l'indice di autorità è modificato, possiamo raffinare anche $h^{(0)}_i$ alla stessa maniera $$h^{(1)}_i = \sum_{j = 1}^{n} M\left[i,j \right] \cdot a^{(1)}_j$$

Possiamo quindi applicare questo **metodo iterativo** per raffinare al meglio gli indici di autorità e di hub

$$\begin{align*}
a^{(k+1)}_i &= \sum_{j = 1}^{n} M\left[j,i \right] \cdot h^{(k)}_j\\
h^{(k+1)}_i &= \sum_{j = 1}^{n} M\left[i,j \right] \cdot a^{(k+1)}_j
\end{align*}$$

oppure in forma compatta

$$\begin{align*}
a^{(k+1)}_i &= M^T h^{(k)}\\
h^{(k+1)}_i &= M a^{(k+1)}
\end{align*}$$

e ponendo (in maniera del tutto convenzionale) $h^{(0)} = \underline{1}$.

Ciò che vogliamo è ottenere ad ongi iterazione che i valori di autorità e di hub descrivano **meglio rispetto all'iterazione precedente** la rilevanza di una pagina nella ricerca.
Ciò che cerchiamo è quindi una sorta di "convergenza" ai valori reali di autorità e di hub.

Innanzitutto non possiamo parlare di convergenza vera e propria in quanto ogni volta sommiamo valori non negativi.
Perciò considereremo una convergenza in presenza di una **opportuna normalizzazione**.

Poi, per ottenere convergenza, è necessario che ad ogni iterazione si ottiene un risultato sempre migliore rispetto a quella precedente.
Perciò non vogliamo che accada una situazione del tipo $$\exists i,j : a^{(k)}_i > a^{(k)}_j, \;\; a^{(k+1)}_i < a^{(k+1)}_j, \;\; a^{(k+2)}_i > a^{(k+2)}_j, \;\; ...$$

### Teorema convergenza
Comunque si scelga un vettore iniziale $h^{(0)}$ con valori **positivi**, esiste un valore $c \in \mathbb{R}^+$ e un vettore $z \in \mathbb{R}^n$ **non nullo** tali che $$\lim_{k \rightarrow \infty} \frac{h^{(k)}}{c^k} = z$$ Analogamente per $a^{(k)}$.

#### Proof
Per definizione di $a^{(1)}$ abbiamo che $$h^{(1)} = M a^{(1)} = MM^T h^{(0)}$$
Perciò *"srotolando"* la formula che definisce $h^{(k)}$ avremo che

$$\begin{align*}
h^{(2)} &= M a^{(2)} = MM^T h^{(1)} = (MM^T)(MM^T) h^{(0)} = (MM^{T})^2 h^{(0)}\\
h^{(3)} &= M a^{(3)} = MM^T h^{(2)} = (MM^T)(MM^T)^2 h^{(0)} = (MM^{T})^3 h^{(0)}\\
&\vdots\\
h^{(k)} &= M a^{(k)} = MM^T h^{(k-1)} = (MM^{T})^k h^{(0)}\end{align*}$$

Dato che nella matrice di adiacenza ogni elemento appartiene a $\{ 0,1 \}$, allora per il teorema `C` avremo che la matrice $MM^T$ è una matrice **simmetrica**, **semidefinita positiva**.
Inoltre dato che i suoi valori sono tutti non negativi, allora $MM^T$ è **non nulla**.
Dato che siamo nelle ipotesi del teorema `A`, avremo che la matrice $MM^T$ ha $n$ autovettori reali $z_1, ..., z_n \in \mathbb{R}^n$ distinti, i quali formano una **base ortonormale** per $\mathbb{R}^n$.
Per il teorema `B` sappiamo che ogni autovettore $z_i$ di $MM^T$ ha un relativo autovalore **reale non negativo** $c_i \in \mathbb{R}$.
Senza perdita di generalità assumiamo che gli autovettori siano ordinati in modo tale che i relativi autovalori rispettino il seguente oridine $$c_1 \geq c_2 \geq ... \geq c_n$$
Sempre per il teorema `B` sappiamo che esiste almeno un autovalore <u>strettamente positivo</u>, perciò certamente avremo che $c_1 > 0$.

Dato che gli autovettori $z_1, ..., z_n$ formano una base di $\mathbb{R}^n$ allora possiamo scrivere $h^{(0)}$ come una **combinazione lineare** di questi ultimi, del tipo $$h^{(0)} = \sum_{i=1}^{n} q_i z_i$$
e dunque
$$\begin{align*}
h^{(k)} &= (MM^{T})^k h^{(0)} = (MM^{T})^k \sum_{i=1}^{n} q_i z_i\\
&= (MM^{T})^{k-1} \sum_{i=1}^{n} q_i (MM^{T}) z_i\\
(\textit{per def. di autovettore})&= (MM^{T})^{k-1} \sum_{i=1}^{n} q_i (c_i z_i)\\
&= (MM^{T})^{k-2} \sum_{i=1}^{n} q_i c_i(MM^{T}) z_i = (MM^{T})^{k-2} \sum_{i=1}^{n} q_i c_i^2 z_i\\
&\vdots\\
&= \sum_{i=1}^{n} q_i c_i^k z_i
\end{align*}$$

Ovvero abbiamo ottenuto che $$h^{(k)} = \sum_{i=1}^{n} q_i c_i^k z_i$$
Ora <u>normaliziamo</u> dividendo ambi i membri per $c_1^k$, ovvero per l'autovalore massimo.

$$\frac{h^{(k)}}{c_1^k} = \sum_{i=1}^{n} q_i \left( \frac{c_i}{c_1} \right)^k z_i$$

Individuiamo ora il primo indice $\ell \leq n$ tale che $c_{\ell} > c_{\ell + 1}$, ovvero tale che $$c_1 = c_2 = ... = c_{\ell} > c_{\ell + 1} \geq ... \geq c_n$$
Allora per ogni $i \leq \ell$ avremo che $\left( \frac{c_i}{c_1} \right)^k = 1$, perciò

$$\begin{align*}
\lim_{k \rightarrow \infty} \frac{h^{(k)}}{c_1^k} &= \lim_{k \rightarrow \infty} \sum_{i=1}^{n} q_i \left( \frac{c_i}{c_1} \right)^k z_i\\
&= \lim_{k \rightarrow \infty} (q_1z_1 + q_2z_2 + ... + q_{\ell}z_{\ell} + q_{\ell + 1} \left( \frac{c_{\ell + 1}}{c_1} \right)^k z_{\ell + 1} + ... +  q_n \left( \frac{c_n}{c_1} \right)^k z_n)\\
&= q_1z_1 + q_2z_2 + ... + q_{\ell}z_{\ell}
\end{align*}$$

Dimostriamo il teorema <u>solamente</u> nel caso in cui $\ell = 1$, ovvero quando $$\lim_{k \rightarrow \infty} \frac{h^{(k)}}{c_1^k} = \lim_{k \rightarrow \infty} \frac{(MM^T)^k h^{(0)}}{c_1^k} = q_1z_1$$
Per concludere la dimostrazione del teorema basti dimostrare che, comunque si scelga $h^{(0)}$ a coordinate **positivie**, avremo che $q_1 \neq 0$.

Infatti, sappiamo che $z_1$ è un vettore **reale** e **non nullo** in quanto è un elemento di una [[#Base Ortonormale|base ortonormale]] di $\mathbb{R}^n$.

Se fosse nullo, avremo che $z_1$ non sarebbe normale, ovvero $$z_1 = \underline{0} \implies z_1 \cdot z_1 = \underline{0} \cdot \underline{0} = 0 \neq 1$$

Osserviamo inoltre che la base è ortonormale allora $q_1 = h^{(0)}z_1$, infatti

$$\begin{align*}
h^{(0)}z_1 &= \left(\sum_{i = 1}^{n} q_i z_i \right) \cdot z_1\\
&= \sum_{i = 1}^{n} q_i (z_i \cdot z_1)\\
&= q_1 \cdot \underbrace{(z_1 \cdot z_1)}_{1} + q_2 \cdot \underbrace{(z_2 \cdot z_1)}_{0} + ... + q_n \cdot \underbrace{(z_n \cdot z_1)}_{0} = q_1
\end{align*}$$

Perciò $q_1 \neq 0$ se e solo se $h^{(0)}z_1 \neq 0$.
Dimostriamo quindi che comunque scegliamo $h^{(0)}$ a coordinate positive $h^{(0)}z_1 \neq 0$.

#### Parte 1
Innanzitutto dimostriamo che esiste un vettore $y \in \mathbb{R}^n$ a coordinate positive tale che $y z_1 \neq 0$.
Supponiamo per assurdo che **ogni** vettore $x \in \mathbb{R}^n$ a coordinate positive vale che $x z_1 = 0$.
Scegliamo ora un qualsiasi vettore $y$ a coordinate positive $y_i > 0$ per ogni $i = 1,...,n$.
Per ogni $\ell = 1,...,n$ indichiamo il vettore $y^{\left[ \ell \right]}$ nel segiente modo
$$y^{\left[ \ell \right]} = \begin{cases}
y_i &\mbox{se } i \neq \ell\\
y_i + 1 &\mbox{se } i = \ell\\
\end{cases}$$
Per esempio $$y^{\left[ 3 \right]} = (y_1, y_2, y_3+1, y_4, ..., y_n)$$
Dato che stiamo assumendo che per ogni vettore $x$ a coordinate positive $x z_1 = 0$, allora il seguente sistema è soddisfatto

$$\begin{cases}
y \cdot z_1 &= 0\\
y^{\left[ \ell \right]} \cdot z_1 &= 0
\end{cases}$$
per ogni $\ell = 1, ..., n$.

Andando a risolvere il sistema avremo che
$$
\begin{cases}
y_1 \cdot z_{11} +  y_2 \cdot z_{12} + ... +  y_{\ell} \cdot z_{1\ell} + ... +  y_n \cdot z_{1n} &= 0\\
(y_1 + 1) \cdot z_{11} +  y_2 \cdot z_{12} + ... +  y_{\ell} \cdot z_{1\ell} + ... +  y_n \cdot z_{1n} &= 0\\
y_1 \cdot z_{11} +  (y_2 + 1) \cdot z_{12} + ... +  y_{\ell} \cdot z_{1\ell} + ... +  y_n \cdot z_{1n} &= 0\\
\vdots\\
y_1 \cdot z_{11} +  y_2 \cdot z_{12} + ... +  (y_{\ell} + 1) \cdot z_{1\ell} + ... +  y_n \cdot z_{1n} &= 0\\
\vdots\\
y_1 \cdot z_{11} +  y_2 \cdot z_{12} + ... +  y_{\ell} \cdot z_{1\ell} + ... +  (y_n + 1) \cdot z_{1n} &= 0
\end{cases}$$
$$\begin{cases} &y_1 \cdot z_{11} +  &y_2 \cdot z_{12} + ... +  &y_{\ell} \cdot z_{1\ell} + ... +  &y_n \cdot z_{1n} &= 0\\
&z_{11} &&&&= 0\\
&&z_{12} &&&= 0\\
&⋮&&&&\\
&&&z_{1\ell} &&= 0\\
&⋮&&&&\\
&&&&z_{1n} &= 0
\end{cases}$$

 Ovvero in poche parole, per ogni vettore $x$ a cooridnate positive $x z_1 = 0$ se e soltanto se $z_1 = \underline{0}$.
 Ma da prima sappiamo che $z_1 \neq 0$, perciò deve necessariamente essere vero che esiste un vettore $y$ a coordinate positive tale che $y z_1 \neq 0$.

#### Parte 2
In conclusione si vuole dimostrare che per ogni vettore $y$ a coordinate positive vale che $y z_1 \neq 0$, quindi questo vale anche per $h^{(0)}z_1$ dato che $h^{(0)}$ è un vettore a coordinate positive.

Dalla parte precedente sappiamo che esiste almeno un vettore $y$ a coordinate positive tale che $y z_1 \neq 0$.
Scriviamo $y$ come combinazione lineare della base ortonormale composta dagli autovettori di $MM^T$.
$$y = p_1z_1 + p_2z_2 + ... + p_nz_n$$

Come già visto prima avremo che l'espressione $\frac{(MM^T)^k y}{c_1^k}$ **converge** a $p_1z_1$.

Inoltre dato che $\frac{(MM^T)^k y}{c_1^k}$ contiene solo coordinate **non negative** avremo che anche $p_1z_1$ avrà coordinate **non negative**.

Poiché sappiamo che $y z_1 \neq 0$ e che $p_1 = y z_1$, allora $p_1 z_1$ avrà almeno una coordinata **strettamente positiva**.
Ovvero $z_1$ ha tutte coordinate non negative ($\geq 0$) e almeno un positiva ($> 0$).

Consideriamo ora un qualsiasi altro vettore $x \neq y$ a coordinate positive.
Abbiamo che $$x \cdot z_1 = x_1z_{11} + x_2z_{12} + ... + x_nz_{1n} \neq 0$$
L'ultima uguaglianza è vera perché $x$ ha tutte coordinate positive mentre abbiamo dimostrato prima che $z_1$ ne **almeno** una positiva $\square$.

## Considerazioni
Nel teorema precedente abbiamo dimostrato che la quantità $\frac{h^{(k)}}{c_1^k}$ tende ad un vettore $z$ non nullo.
L'abbiamo dimostrato solamente nel caso $c_1 > c_2$, però con tecniche analoghe si può dimostrare in qualsiasi caso generale.

In poche parole, il precedente teorema ci dice che, nel caso in cui $c_1 > c_2$, qualunque sia il vettore di hub iniziale $h^{(0)}$ purché a coordinate **positive**, avremo che $\frac{h^{(k)}}{c_1^k}$ convergerà al vettore $q_1 z_1$, dove $q_1 = h^{(0)}z_1$.
Ciò significa che comunque scegliamo un vettore di hub iniziale, esso convergerà ad un altro vettore **parallelo** all'autovettore $z_1$.
Perciò la convergenza del vettore di hub dipende <u>solamente</u> dalla matrice $M$.

Ricapitolando, l'algoritmo `HITS` valuta la rilevanza di una pagina presente nel sottoinsieme di pagine inrenti alla ricerca rispetto a due **ruoli**:

- rispetto al ruolo di **autorità** che indica la rilevanza della pagina rispetto alla ricerca, e tale quantità è determinata mediante lo studio degli link **entranti** nella pagina.
- rispetto al ruolo di **hub** che indica quanta rilevanza da tale pagina alle pagine che essa punta, e tale quantità è invece derminata mediante lo studio dei suo link **uscenti**.

Ciò significa che si può incorrere in situazioni in cui una pagina poco rilevante ai fini di una ricerca conferisca invece rilevanza ad altre pagine.

In poche parole l'intuizione alla base di *Hubs & Authorities* si basa sull'idea che le pagine svolgano molteplici ruoli nella rete e, in particolare, che le pagine possano svolgere un potente ruolo di **approvazione** senza che esse siano fortemente sostenute.
Perciò l'algotimo `HITS` se presta bene a modellare tutte quelle situazioni in cui le pagine sono naturalmente partizionate in due sottoinsiemi **semanticamente** distinti.
Per esempio quando ricerchiamo degli oggetti da comprare, le pagine dei rivenditori non si puntano a vincenda in quanto sono concorrenti: se si puntano allora si stanno dando rilevanza al concorrente.
Viceversa, alcuni siti come eBay puntano ad altri rivenditori.

------------------------------------------------------------------------

# Appendice

## Autovalori e Autovettori
Data una matrice $A \in \mathbb{C}^{n \times n}$, un vettore $x \in \mathbb{C}^n$ tale che $x \neq \overline{0}$ e un valore $\lambda \in \mathbb{C}$, diremo che essi sono rispettivamente un **autovettore** e relativo **autovalore** della matrice $A$ se 

$$Ax = \lambda x$$ ovvero se $$(A - \lambda I)x = 0$$

## Base
Una base per uno spazio vettoriale $\mathbb{R}^n$ è un inseme di $n$ vettori $z_1, ..., z_n \in \mathbb{R}^n$ tali che ogni altro vettore $x \in \mathbb{R}^n$ può essere espresso come una **combinazione lineare** della base, ovvero $$\forall x \in \mathbb{R}^n \exists p \in \mathbb{R}^n : x = \sum_{i = 1}^{n} p_i \cdot z_i$$

## Vettore Normale
Dato un vettore $z \in \mathbb{R}^n$ esso si dice **normale** se $$z \cdot z = z_1^2 + z_2^2 + ... + z_n^2 = 1$$

## Vettore Ortogonale
Dati due vettori $z_i,z_j \in \mathbb{R}^n$ essi si dice **ortogonali** se $$z_i \cdot z_j = z_{i1}z_{j1} +  z_{i2}z_{j2} + ... +  z_{in}z_{jn} = 0$$

## Base Ortonormale
Data una base $z = (z_1, ..., z_n) \in \mathbb{R}^{n \times n}$ essa è detta **ortonormale** se tutti i vettori sono normali e reciprocamente ortogonali, ovvero se

$$\begin{align*}
\forall i = 1, ..., n &: z_i \cdot z_i = z_{i1}^2 + z_{i2}^2 + ... + z_{in}^2 = 1\\
\forall i \neq j &: z_i \cdot z_j = z_{i1}z_{j1} +  z_{i2}z_{j2} + ... +  z_{in}z_{jn} = 0
\end{align*}$$

## Matrice Semidefinita Positiva
Una matrice $A$ $n \times n$ è detta **semidefinita positiva** se $$\forall x \in \mathbb{R}^n : x^T A x \geq 0$$

## Teorema A
Se $A$ è una matrice a valori **tutti reali** e **simmetrica**, ovvero $A = A^T$, allora $A$ avrà $n$ autovalori ed $n$ autovettori reali e distinti, e inoltre tali autovalori formano una **base ortonormale** per $\mathbb{R}^n$.

## Teorema B
Se $A$ è una matrice **semidefinita positiva** allora:

1. gli autovalori di $A$ sono **non negativi**.
2. se $A$ è non nulla, ovvero $A \neq \underline{0}$, allora $A$ avrà almeno un autovalore $\lambda > 0$ **strettamente positivo**.

## Teorema C
Se $A$ ha solamente elementi **reali**, allora $AA^T$ è un matrice **simmetrica** e **semidefinita positiva**.


-------------------------------------------------------------