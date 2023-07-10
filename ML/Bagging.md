Il **bagging** è un [[Ensemble methods|metod di ensamble]] che consiste dell'addestrare un insieme di modelli, per poi fare la predizione tramite una **media pesata** delle singole predizioni, pesandole in base alla qualità dei singoli.

Consideriamo per esempio i [[Decision Trees]].
In base a un dato dataset $\mathcal{T}$ avremo un albero, il quale rinforza la sua precisione man mano che si scende verso le foglie.
Questo decision tree dipende però troppo da $\mathcal{T}$, infatti a fronte di piccole perturbazioni sui dati abbiamo decision tree molto differenti.

## Bootstrap
Il **boostrap** è uno strumento statistico.
Quello che si vuole fare è cercare di stimare una distribuzione $\mathcal{F}$ tramite una **distribuzione empirica** $\hat{\mathcal{F}}$.
Consideriamo il dataset $(\mathbf{X}, \mathbf{t})$ grande $n$, e definiamo la distribuzione empirica $\hat{\mathcal{F}}$ come segue
$$\hat{p}(x, t) := \begin{cases}
\dfrac{1}{n} &\text{if } \exists (x_i, t_i) \in (\mathbf{X}, \mathbf{t}): (x_i, t_i) = (x, t)\\
\\
0 &\text{otherwise}
\end{cases}$$

Un **bootstrap sample** di $m$ elementi da un dataset $(\mathbf{X}, \mathbf{t})$ è composto da $m$ campionamenti $(x^*, t^*)$ da $(\mathbf{X}, \mathbf{t})$ ==con **rimpiazzo**==.
Questo bootstrap sampling equivale all'aver campionato $m$ elementi dalla distribuzione empirica $\hat{\mathcal{F}}$.
Questo può essere utilizzato come strumento per **estendere** la dimensione del dataset, ed è molto utile quando non si ha la possibilità di generare molti dataset.
Quando $m = n$ possiamo interpretarlo come **rigenerazione** del dataset. ^638637

# Bagging
Il bagging consiste nel addestrare uno stesso modello, però su una collezione di [[#^638637|bootstrap samples]] differenti.
Quindi, per $b = 1, ..., B$ facciamo $n$ *bootstrap sample* dal dataset $(\mathbf{X}, \mathbf{t})$.
Indichiamo il $b$-esimo sample come $(x^b_i, t^b_i)$ per $i = 1, ..., n$.

Una volta addestrati $B$ classificatori differenti, per classificare un elemento $x$ possiamo per esempio assegnarli la classe predetta dalla maggior parte dei $B$ classificatori.
Per quanto riguarda la regressione invece possiamo restituire la media dei $B$ valori predetti.

Se invece ogni predittore $b$ restituisce una probabilità $\hat{p}^b_k(x)$ sulla classe $C_k$, possiamo definire la probabilità media $$p_k(x) = \frac{1}{B}\sum_{b=1}^{B}\hat{p}^b_k(x)$$
Come al solito, assegnamo all'input $x$ la classe $C_i$ che *massimizza la probabilità* media $p_i(x)$.

## Bagging Classification
Analizziamo perché il bagging funziona.
Per semplicità consideriamo un problema di **classificazine binaria**.
Supponiamo di aver addestrato $B$ modelli, con un errore rate di $e \in \left[ 0,1 \right]$
Senza perdita di generalità, assumiamo di avere in input $x$ con classe $1$.
Sia $B_0 \leq B$ il numero di classificatori che classificano $x$ male, ovvero gli assegnano la classe $0$.
Dato che i $B$ classificatori sono tutti **indipendenti**, allora avremo che $$B_0 \sim \text{Binom}(B, e)$$
Perciò la probabilità di classificare **male** un input $x$ può essere modellata con l'evento $B_0 \geq B/2$, con probabilità $$p\left(B_0 \geq \frac{B}{2}\right) = \sum_{k=\frac{B}{2}}^{B}\binom{B}{k}e^k(1-e)^{B-k}$$
Per $e < 0$ tale probabilità tende a $0$ al crescere di $B$.

------
# Random Forest
Una **random forest** è un'applicazione del [[Bagging]] su un insieme di (random) [[Decision Trees]].

1. For $b = 1$ to $B$
	1. faccio un [[#Bootstrap|bootstrap sample]] di $n$ elementi
	2. Faccio crescere un decisione tree $T_b$ su questo bootstrap sample, eseguendo le seguenti operazioni su ogni nodo:
		1. seleziono $m$ variabili a caso
		2. faccio lo split del nodo in due figli, utilizzando la megliore di queste $m$ variabili.
2. Ritorno la collezione di alberi $T_1, ..., T_B$.

Per classificare un elemento $x$ basta che ritorno la classe che viene predetta alla maggiore tra i $B$ alberi.