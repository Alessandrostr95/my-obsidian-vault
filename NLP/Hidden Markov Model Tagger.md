L'**Hidden Markv Model** (o **HMM**) è un modello di pos-tagging **probabilistico**, che dato un testo (una sequenza di parole) che calcola le distribuzioni di probabilità di tutte le possibili sequenze di labeling e restituisce quella che massimizza tale probabilità (come uno [[Stimatore di Massima Verosimiglianza]]).

# Catene di Markov
Una catena di Markov è un modello probabilistico utile a studiare **sequenze** di **stati di un sistema**.
Lo stato del sistema in un dato momento $t$ è sempre univocamente definito da una v.a. $X_t$.
Abbiamo un insieme **finito** di stati possibili $q_1, ..., q_n$.
Per ogni coppia di stati $q_i, q_j$ è definita una probabilità di passare dallo stato $q_i$, allo stato $q_j$ $$P(X_{t+1}= q_j \vert X_t = q_i) = p_{ij}$$
Mettendo insieme tutte queste probabilità possiamo definire una **matrice di transizione** $P$ $n \times n$.

La principale assunzione delle catene di markov è che lo stato del sistema dipende solamente dallo stato al tempo precedente $$P(X_t = q \vert X_0, X_1, ..., X_{t-1}) = P(X_t = q \vert X_{t-1})$$
Questa proprietà è anche detta **mancanza di memoria** delle catene. ^cd70c4

È importante anche avere una **distribuzione iniziale** $\overline{\pi}$ che indica la probabilità che il sistema inizi in un dato stato.

Definiamo la **distribuzione marginale** $\overline{p}(t)$ che indica la probabilità di trovarsi in uno stato al tempo $t$, ovvero
$$\overline{p}(t) = \langle p_1(t), p_2(t), ..., p_n(t) \rangle$$
$$p_i(t) = P(X_t = q_i)$$

Tale distribuzione può essere calcolata grazie alle probabilità totali sfruttando la matrice di transizione $P$
$$\underbrace{P(X_t = q_i)}_{p_i(t)} = \sum_{q_j} \underbrace{P(X_t = q_i \vert X_{t-1} = q_j)}_{p_{ji}} \underbrace{P(X_{t-1} = q_j)}_{p_j(t-1)}$$
$$\implies \overline{p}(t) = \overline{p}(t-1) \cdot P$$
Possiamo quindi estendere questa formula in maniera ricorsiva $$\overline{p}(t) = \overline{\pi} \cdot P^t$$

Per maggiori approfondimenti vedi [qui](http://95.179.136.254:2122/CP2/markov_chains.html)

# The Hidden Markov Model
Una [[#Catene di Markov|catena di markov]] è utile quando vogliamo calcolare la probabilità di una sequenza di **eventi osservabili**.
In alcuni casi però gli eventi non sono **direttamente osservaibili**, ma sono *nascosti* (**hidden**).
Questo è il caso dei pos-tag: quando noi leggiamo un testo possiamo osservare le parole che occorrono, ma le [[Part of Speach|part-of-speach]] sono intrinseche nel testo, e noi possiamo solamente **inferirle**.

Un **Hidden Markov Model** (**HMM**) ci consente di trattare una serie di **eventi osservabili** (le parole del testo in input) ed **eventi hidden** (i pos).
Un **HMM** è definito dalle seguenti componenti:
- una insieme **finito di stati** $Q = q_1, ..., q_N$.
- una **matrice di transizione** $A$.
- una **distribuzione iniziale** $\overline{\pi}$.
- una sequenza di $T$ **osservazioni** $O = o_1 ... o_T$, ognua delle quali campionata da un **vocabolario** $V = \lbrace v_1, ..., v_V \rbrace$.
- una sequeza di **stimatori** $B_i(o_t)$ che indica la probabilità che $o_t$ sia stato generato dallo stato $q_i$. Definiamo quindi una **matrice di probabilità** $B$ $N \times T$, dove $B_{i,j} = B_i(o_j)$. 

L'assunzione di **mancanza di memoria** della catena persiste anche nel HMM $$P(X_t = q_i \vert X_1, ..., X_{t-1}) = P(X_t = q_i \vert X_{t-1})$$
Un'altra assuzione è che la probabilit che occorra un'osservazione $q_i$ dipende solamente dallo stato che lo ha generato, e non dagli stati e della osservazioni $$P(o_i \vert X_1, ..., X_i, ..., X_T, o_1,..., o_i, ..., o_T) = P(o_i \vert X_i)$$
Questa proprietà è anche nota come **Output Independence**. ^e3a7c1


In questo modello quindi gli **stati** $Q$ sono le variabili nascoste da dover inferire, sfruttando le osservazioni fatte $O$.
Un HMM può quindi essere definito dalla coppia di matrici $(A,B)$ e

# The Hidden Markov Model Tagger
Vediamo ora come poter modellizzare il problema del pos tagging con un HMM.
Dato un HMM $\lambda = (A,B)$ e una sequenza di **parole osservate** $w_1, ..., w_n$ (le osservazioni $O$) trovare la sequenza di pos-tag **più probabile** $t_1, ..., t_n$ (gli stati hidden $Q$).

Ovvero vogliamo trovare lo [[Stimatore di Massima Verosimiglianza]] $$\hat{t}_1^n = \arg \max_{t_1, ..., t_n}{P(t_1, ..., t_n \vert w_1, ..., w_n)}$$
Usando la **regola di Bayes** possiamo riscriverlo come $$\hat{t}_1^n = \arg \max_{t_1, ..., t_n}{\frac{P(w_1, ..., w_n \vert t_1, ..., t_n)P(t_1, ..., t_n)}{P(w_1, ..., w_n)}}$$
Osserviamo che la probabilità a denominatore $P(w_1, ..., w_n)$ è una **costante** per ogni sequenza $t_1, ..., t_n$, perciò avremo che $$\hat{t}_1^n = \arg \max_{t_1, ..., t_n}{P(w_1, ..., w_n \vert t_1, ..., t_n)P(t_1, ..., t_n)}$$

Date che stiamo su una catena di Markov, la sequenza $t_1, ..., t_n$ equivale a un **random walk**, con probabilità
$$\begin{align*}
P(\langle X_1 = t_1, ..., X_n = t_n \rangle)
&= P(t_n \vert t_1, ..., t_{n-1}) \cdot P(t_1, ..., t_{n-1})\\
&= P(t_n \vert t_1, ..., t_{n-1}) \cdot P(t_{n-1} \vert t_1, ..., t_{n-2}) \cdot P(t_1, ..., t_{n-2})\\
&\vdots\\
&= \prod_{i=1}^{n} P(t_i \vert t_1, ..., t_{i-1})
\end{align*}$$
Applicando la proprietà di **[[#^cd70c4|mancanza di memoria]]** di una catena di Markvo avremo che $$P(t_1,...,t_n) = \prod_{i=1}^{n}P(t_i \vert t_{i-1})$$
Questa è anche nota come **bigram assumption**, in cui si assume che il tag di una parola dipende solamente dal tag della parola precedente.

Sfruttando invece la **[[#^e3a7c1|Output Independence]]** delle HMM, e assumendo **indipendenza** tra le parole del testo, avremo che
$$\begin{align*}
P(w_1, ..., w_n \vert t_1, ..., t_n)&=\\
\\
(\text{Words Independence})&= \prod_{i=1}^{n} P(w_i \vert t_1, ..., t_n)
\\
(\text{Output Independence})&= \prod_{i=1}^{n} P(w_i \vert t_i)
\end{align*}$$
In poche parole stiamo assumendo che la probabilità che una parola appaia dipenda solamente dal proprio tag, e non dalle parole ad essa vicine.

Combinando queste due semplificazioni avremo

$$\hat{t}_1^n = \arg \max_{t_1, ..., t_n}{P(t_1, ..., t_n \vert w_1, ..., w_n)} \approx \arg \max_{t_1, ..., t_n} \prod_{i=1}^{n} \underbrace{P(w_i \vert t_i)}_{\text{emission}} \; \underbrace{P(t_i \vert t_{i-1})}_{\text{transition}}$$
dove:
- $P(w_i \vert t_i)$ è la probabilità di **emissione** (occorrenza) della parola $w_i$ dato il tago $t_i$. Questa probabilità è definità matrice $B$ dell'HMM.
- $P(t_i \vert t_{i-1})$ è la probabilità di avere il tag $t_i$ sapendo che quello precedente è il tag $t_{i-1}$. Questa probabilità è invece definita nella matrice $A$.

## The components of an HMM tagger
Modellando $P(t_i \vert t_{i-1})$ e $P(w_i \vert t_i)$ come delle **bernoulliane** possiamo trovare il loro [[Stimatore di Massima Verosimiglianza|MLE]] con la [[Random Sample#Media campionaria|media campionaria]].

Nel caso $P(t_i \vert t_j)$ basta fare il rapporto tra tutte le volte che osserviamo il tag $t_i$ dopo tag $t_j$ nel nostro corpus annotato, fratto il numero totale di volte in cui incontriamo $t_j$, ovvero $$P(t_i \vert t_j) \approx \frac{\#(\langle t_j, t_{i} \rangle)}{\#(t_j)}$$

Per esempio se il tag MD appare 13124 volte in un corpus annotato, di cui 10471 è seguito dal tag VB avremo che $$P(VB \vert MD) \approx \frac{10471}{13124} \approx 80 \%$$

Stessa cosa per $P(w_i \vert t_i)$, possiamo stimarlo come il numero di volte in cui alla parola $w_i$ è associato il tag $t_i$ fratto il numero di osservazioni del tag $t_i$.
Per esempio, se il tag MD appare 13124 volte di cui 4046 associato alla parola *"will"* avremo che $$P(\text{will} \vert MD) \approx \frac{4046}{13124} \approx 31 \%$$
