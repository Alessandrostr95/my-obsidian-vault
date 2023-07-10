Un **albero di decision** (o **decision tree**) è un classificatore che **ricorsivamente** partiziona lo spazio delle features.
Ogni nodo del decision tree rappresenta una **regione** dello spazio.
Ogni nodo interno ha quindi due figli, i quali rappresentano la **pratizione** della regione del nodo padre.
Alla fine, le foglie rappresentano una **partizione** di tutto lo spazio, e a ciascuna di essere è associata una classe.
Perciò per fare la classificazione di una qualsiasi input $x$, basta percorrere l'albero dalla radice e vedere a quale foglia appartiene.

## Example
![[ML/img/ML_12_1.png]]

![[ML/img/ML_12_2.png]]

![[ML/img/ML_12_3.png]]

![[ML/img/ML_12_4.png]]

![[ML/img/ML_12_5.png]]

![[ML/img/ML_12_6.png]]

![[ML/img/ML_12_7.png]]

# Construction
Lo spazio delle feature è **partizionato ricorsivamente** costruendo l'albero decisionale dalla radice alle foglie.
Ciò che ci serve sapere ad ogni nodo è:
1. Come eseguire una partizione della regione corrispondente (scelta la feature da partizionare e la funzione)?
2. Quando interrompere il partizionamento? Come assegnare le classi alle foglie?

## Partitioning at each node
Ciò che si vuole fare, ad ogni nodo, è selezionare la **feature/dimensione** sulla quale effettuare la partizione della regione, e la **funzione/soglia** secondo la quale assegnare i punti alle sottoregioni.
È desiderabile scegliere questa coppia **feature-soglia** in modo tale da **minimizzare** una **misura di impurità** delle sottoregioni, o analogamente da **massimizzare una misura di qualità**.

Definiamo come deve essere fatta una **misura di impurità**.
Consideriamo una variabile aleatoria multidimensionale con **dominio discreto** e vettore di probabilità $\vec p = (p_1, ..., p_k)$.
Una **misura di impurtià** è una funzione $\phi: \vec p \to \mathbb{R}$ tale che valgono le seguenti proprietà:
1. $\phi(\vec p) \geq 0$ per ogni possibile $\vec{p}$.
2. $\phi(\vec{p})$ è **minimizzata** quando *esiste* un $p_i \in \vec{p}$ tale che $p_i = 1$.
3. $\phi(\vec{p})$ è **massimizzata** quando *per ogni* $p_i \in \vec{p}$ vale che $p_i = 1/k$.
4. $\phi(\vec{p}) = \phi(\vec{p}^*)$ per ogni **permutazione** $\vec{p}^*$ di $\vec{p}$.
5. $\phi(\vec{p})$ è **differenziabile** ovunque.

Consideriamo il problema della classificazione $K \geq 2$ classi, e prendiamo una qualsiasi **regione** $S$.
Associamo ad $S$ il vettore di probabilità $$p_S = \left(\frac{\vert S_1 \vert}{\vert S \vert}, ..., \frac{\vert S_K \vert}{\vert S \vert}\right)$$ dove $$S_h \equiv \lbrace x \in S \cap \mathcal{T} : x \in C_h \rbrace$$ è l'insieme di elementi dentro la regione $S$ che appartengono alla classe $C_h$.
Definiamo la funzione $$f: S \to \lbrace 1, ..., r \rbrace$$ e definiamo con $$s_i \equiv \lbrace x \in S : f(x) = i \rbrace$$
La funzione $f$ di fatto esegue una **partizione** di $S$ in $s_1, ..., s_r$ regioni.
La **goodness** della partizione $s_1, ..., s_r$ di $S$ (fatta mediante la funzione $f$) è definita come $$\Delta_{\phi}(S,f) := \phi(p_S) - \sum_{i=1}^{r}\frac{\vert s_i \vert}{\vert S \vert} \cdot \phi(p_{s_i})$$ dove $p_{s_i}$ è la probabilità della partizione $s_i$ fatta mediante $f$.
La goodness può anche essere vista come la differenza tra l'**impurità** di $S$ e la media pesata delle impurità dei sottoinsiemi $s_1, ..., s_r$ generati mediante $f$.

> [!tldr]
> Più precisamente, data una sottoregione $s_i$ avremo il vettore di probabilità 
> $$p_{s_i} = \left(\frac{\vert \lbrace x \in s_i : x \in C_1 \rbrace \vert}{\vert s_i \vert}, ..., \frac{\vert \lbrace x \in s_i : x \in C_K \rbrace \vert}{\vert s_i \vert}\right)$$

Indichiamo con $H_S$ l'**entropia** della classe $S$ come $$H_S = - \sum_{i=1}^{K}\frac{\vert S_i \vert}{\vert S \vert}\log_2\frac{\vert S_i \vert}{\vert S \vert}$$
Osservare che l'entropia è **minima** (ovvero pari a 0), quando tutti gli elementi appartengono ad una stessa classe, ed è **massima** (ovvero pari a $\log_2K$) quando invece sono equamente distribuiti tra le $K$ classi.

Se usiamo l'entropia come **misura di inpurità** avremo come **goodness** della partizione $s_1, ..., s_r$ una quantità nota come **information gain** (**guadagno informativo**)
$$IG(S, f) = H_S - \sum_{i=1}^{r} \frac{\vert s_i \vert}{\vert S \vert}H_{s_i}$$

Un'altra misura di inpurità usata è il **gini index**, definito come $$G_S = 1- \sum_{i=1}^{K} \left( \frac{\vert S_i \vert}{\vert S \vert}\right)^2$$
In tal caso la goodness è nota come **gini gain**
$$GG(S,f) = G_S - \sum_{i=1}^{r} \frac{\vert s_i \vert}{\vert S \vert} G_{s_i}$$

Perciò, fissata una misura di goodnes si partiziona $S$ scegliendo al feature e la funzione $f$ che massimizza $\Delta_{\phi}(S,f)$.

## Leavs
Ovviamente se non diamo un limite al numero di partizioni, la partizione ottima è quella che, per ogni elemento del dataset, abbiamo una regione differente (ovvero si va in **overfittinig**).
Alcune politiche per limitare l'overfitting sono:
1. limitare la **profondità** dell'albero.
2. limitare il **numero di foglie**.
3. definire un **numero minimo** di elementi per regione.

Una volta identificato quando una regione è una foglia, possiamo associargli la classe di maggioranza presente al suo interno.

Osservare che:
- fermare troppo presto la creazione dell'albero tende all'**underfitting**.
- fermare troppo tardi invece fa tendere all'**overfitting**.

Perciò, un'ulteriore tecnica per ridurre l'overfitting è quella di applicare tecniche di **pruning**.
