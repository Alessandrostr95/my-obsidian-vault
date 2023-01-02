# Alfabeto $\Sigma$
Sia $\Sigma$ un insieme <u>finito</u> di **simboli**, detto **alfabeto**.
Una **stringa** $s$ è composta dalla **concatenazione** di simboli appartenenti a $\Sigma$.

Esempio:
- $\Sigma = \lbrace a,b,c \rbrace$
- $s = abbacb$

# Espressione Regolare
Un'**espressione regolare** è una **formula** che descrive un insieme di stringhe su di un alfabeto $\Sigma$.

Un'espressione regolare, può essere composta da un **simbolo** di $\Sigma$ oppure dalla **composizione** di altre espressioni.

Gli **operatori** utili a combinare espressioni regolari e simboli sono:
- La **concatenazione**: $abc$ che sta ad indicare la stringa $abc$, con $a,b,c \in \Sigma$
- La **disgiunzione**: $a + b$, che indica la stringa composta dal solo simbolo $a$ **oppure** dal solo simbolo $b$.
- La **stella di Kleene**: $(ab)^*$ ovvero una sequenza di $ab$, potenzialmente anche una sequenza **vuota**.
- $(ab)^+$ come stella di Kleene, ma mai vuota.
- $(ab)^n$ una sequenza di $n$ volte $ab$.
- La **stringa vuota** $\varepsilon$.

# Linguaggio Formale
Un linguaggio è un qualsiasi **sottoinsieme di stringhe** $L \subseteq \Sigma^*$.

Anche sui linguaggio possiamo definire gli stessi operatori delle [[#Espressione Regolare|espressioni regolari]].

- **Concatenazione** $L_1 \circ L_2 \equiv \lbrace s_1s_2 \in \Sigma^* \vert s_1 \in L_1 \land s_2 \in L_2 \rbrace$ 
- **Intersezione** $L_1 \cap L_2 \equiv \lbrace s \in \Sigma^* \vert s \in L_1 \land s \in L_2 \rbrace$
- **Unione** $L_1 \cup L_2 \equiv \lbrace s \in \Sigma^* \vert s \in L_1 \lor s \in L_2 \rbrace$
- **Differenza** $L_1 \setminus L_2 \equiv \lbrace s \in \Sigma^* \vert s \in L_1 \land s \not\in L_2 \rbrace$
- $L^n \equiv L \circ L \circ \dots \circ L$ per $n$ volte.
- **Stella di Kleene** $L^* \equiv \bigcup_{n \in \mathbb{N}} L^n$ dove $L^0 = \lbrace \varepsilon \rbrace$
- $L^+ \equiv L^* \setminus L^0$
- **Complemento** $\overline{L} \equiv \Sigma^* \setminus L$


# Grammatica Formale
Una **grammatica formale** è una struttura matematica che serve per descrivere un [[#Linguaggio Formale]].

Una grammatica $\mathcal{G}$ è formalmente una **quadrupla** $$\mathcal{G} = \langle N, \Sigma, P, S \in N \rangle$$ tale che:
- $N$ è un insieme di simboli detti **non terminali**, il quale é **disgiunto** da $\Sigma$.
- Un alfabeto $\Sigma$, detto anche **simboli terminali**
- Un **relazione** $P$ che definisce delle **regole di produzione** che trasformano delle sequenzi di simboli terminali e non in altre sequenze. $$P: (N \cup \Sigma)^* \circ N \circ (N \cup \Sigma)^* \to (N \cup \Sigma)^*$$ Osservare che nelle regole di produzione ci deve essere **almeno** un simbolo **non terminale**. Una produzione è spesso indicata come $\alpha \to \beta$.
- $S \in N$ è un assioma dal quale partono tutto le sequenze di produzioni.

Un linguaggio $L$ è detto **generato** da una grammatica $\mathcal{G}$ se per ogni stringa $s \in L$ esiste una sequenza di **produzioni** nella grammatica che dall'assioma $S$ porta alla stringa $s$.

### Esempio: palindrome
Sia l'alfabeto $\Sigma = \lbrace a,b \rbrace$.
Si vuole definire la grammatica $\mathcal{G}$ che **generi** il linguaggio di tutte le stringhe **palindrome** su $\Sigma$.
$$\begin{align*}
S \to a \;\vert\; b \;\vert\; aSa \;\vert\; bSb \;\vert\; \varepsilon
\end{align*}$$

La stringa $aabbabbaa$ è **generata** dalla seguente sequenza di produzioni
$$S \to aSa \to aaSaa \to aabSbaa \to aabbSbbaa \to aabbabbaa$$

# La gerarchia di Chomsky
[Noam Chomsky](https://it.wikipedia.org/wiki/Noam_Chomsky) definì una **gerarchia** di grammatiche formali, separate in 4 gruppi.

## Tipo 3 - Grammatiche Regolari
Una **grammatica regolare** è divisa in dua tipologie:
- **Lineare Sinistra** con produzioni del tipo $$A \to \beta$$ dove $A \in N$ e $\beta \in (N \circ \Sigma) \cup \Sigma$.
- **Lineare Destra** con produzioni del tipo $$A \to \beta$$ dove $A \in N$ e $\beta \in (\Sigma \circ N) \cup \Sigma$.

Osservare che a sinistra della produzione ci sta sempre un solo non terminale, mentre a destra ci sono al più 2 dimboli, di cui 1 terminale e uno no.

I linguaggio regolari sono equivalenti ai linguaggi riconosciuti dagli [automi a stati finiti](https://it.wikipedia.org/wiki/Automa_a_stati_finiti "Automa a stati finiti") ed ai linguaggi rappresentati da [espressioni regolari](https://it.wikipedia.org/wiki/Espressione_regolare "Espressione regolare").

### Esempio grammatica regolare
Sia l'alfabeto $\Sigma = \lbrace a,b,c \rbrace$, si vuole rappresentare con una grammatica regolare il linguaggio $L = \lbrace a^nbc^m \vert n,m \geq 0 \rbrace$.

Sia $N = \lbrace X,S \rbrace$.
$$\begin{align*}
S &\to aS \; \vert \; bX \; \vert \; b\\
X &\to cX \; \vert \; c
\end{align*}$$

Questa grammatica è equivalente all'espressione regolare $a^*bc^*$.

## Tipo 2 - Grammatiche Context-Free
Le grammatiche **context-free** sono una generalizzazione di quelle regolari, dove a partire da <u>un solo</u> simbolo terminale viene generata una sequenza di simboli terminali o non.
Più formalmente la relazione di produzione è della forma $$N \to (N \cup \Sigma)^*$$

Mentre per le grammatiche regolari bastava una **automa a stati finiti** per essere accettate, per le grammitche context free non è più sufficiente.
È necessario l'utilizzo degli **[automi a pila](https://it.wikipedia.org/wiki/Automa_a_pila)** (o **PDA**, *PushDown Automa*).

### Esempio 1 grammatica context-free
Si vuole realizzare una grammatica che genera il linguaggio delle **espressioni parentetiche ben formate**.
Avremo quindi
- $\Sigma = \lbrace (,) \rbrace$
- $N = \lbrace S \rbrace$
- $$S \to SS \;\vert\; (S) \;\vert\; ()$$


### Esempio 2 grammatica context-free
Si vuole realizzare una grammatica che generi il linguaggio $$L \equiv \lbrace b^na^mb^{2n} \vert n,m \geq 0 \rbrace$$
- $\Sigma = \lbrace a,b \rbrace$
- $N = \lbrace S,A \rbrace$
$$\begin{align*}
S &\to bSbb \; \vert \; bAbb \; \vert \; A\\
A &\to aA \;\vert\; \varepsilon
\end{align*}$$

## Tipo 1 - Grammatiche Context-Sensitive
Un'ulteriore superclassi di grammatiche sono quelle dette **context-sensitive**.
Tali grammatiche sono appunto **dipendenti dal contesto**, il quale viene letteralmente *"portato appresso"* durante una produzione.
Infatti le regole di produzione sono della forma $$\alpha A\beta \to \alpha \gamma \beta$$ dove
- $A \in N$
- $\alpha, \beta \in (N \cup \Sigma)^*$
- $\gamma \in (N \cup \Sigma)^+$

Per riconoscere un linguaggio context-sensitive si necessita di un [automa lineare](https://it.wikipedia.org/wiki/Automa_lineare_limitato), una speciale [macchina di Turing non deterministica](https://it.wikipedia.org/wiki/Macchina_di_Turing_non_deterministica) dove però la lunghezza del nastro in input è una [funzione lineare](https://it.wikipedia.org/wiki/Funzione_lineare "Funzione lineare") della dimensione dell'input.

## Tipo 0 - Grammatiche Libere
Infine le grammatiche più espressive di tutte sono le **grammatiche libere**.
In pratica queste grammatiche non hanno vincoli sulla forma delle produzioni, purché la stringa che sta a sinistra della produzione non sia nulla.
In pratica $P$ è della forma $$(N \cup \Sigma)^+ \to (N \cup \Sigma)^*$$

Per decidere un linguaggio generato da una grammatica libera si necessita di una [Macchina di Turing](https://it.wikipedia.org/wiki/Macchina_di_Turing).

## Gerarchia di Chomsky
![](gerarchia_di_chomsky.jpg)

## Modi alternatici di classificare le grammatiche
Sia $\alpha \to \beta$ una regola di produzione.
Allora possiamo classificare le grammatiche secondo le seguenti caratteristiche:
- **Regolari**: $|\alpha|=1, 1 \leq |\beta| \leq 2$
- **Context-Free**: $|\alpha|=1, |\beta| \geq 1$
- **Context-Sensitive**: $1 \leq |\alpha| \leq |\beta|$
- **Free**: $|\alpha| \geq 1$

## Grammatiche CNF - Chomsky Normal Form
Una [[#Tipo 2 - Grammatiche Context-Free|context-free]] è detta in forma normale di Chomsky se le produzioni sono tutte della forma
- $A \to BC$
- $A \to a$

Dove $A,B,C \in N$ e $a \in \Sigma \cup \lbrace \varepsilon \rbrace$.
Le produzioni del secondo tipo $A \to a$ sono anche dette **pre-terminali**. ^c1b4a0

È possibile dimostrare che qualsiasi grammatica context-free può essere espressa in forma normale di Chomsky.

