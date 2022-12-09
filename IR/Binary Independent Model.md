Abbiamo discusso nel [[Probabilistic Ranking Principle|PRP]] che abbiamo bisogno di **stimare** la probabilità che un documento restitutio sia rilevante.

L'apporccio più semplice è noto come **Bianary Independent Model**.

In questo modello tutti i documenti (incluse le query) vengono rappresentati come dei **vettori binari** in cui la cella inerente al termine $t$ è pari ad 1 se il termine occorre almeno una volta nel documento, 0 altrimenti.
Tale rappresentazione è simile a quella fatta nel [[Vector Space Model]].

Con tale rappresentazione possono facilmente esserci documenti con la stessa rapressentazione (o comunque molto molto simile).
> **Esempio** `Il cane mangia il gatto` e `Il gatto mangia il cane` avranno la stessa identica rappresentazione.

Inoltre non si tiene conto della **frequenza** dei termini all'interno dei documenti (*term frequency*).

Un'assunzione importante del **BIM** è che le occorrenze dei termini in un documento sono **indipendenti** tra loro.
In realtà nel linguaggio naturale non è così, c'è dipendenza tra i termini in una frase, però all'atto pratico assumere l'indipendenza tra i termini ci aiuta moltissimo nei calcoli e in fine come modello funziona abbastanza bene.

Grazie a questa assunzione possiamo ora **stiamare** la probabilità che un documento $d$ sia rilevante data una query $q$, come discusso nel [[Probabilistic Ranking Principle#^87f37c|PRP]].
Ovvero, rappresentando un documento $d$ come un vettore di features $(f_1,...,f_n)$, dove $n$ è il numero di termini e tale che $f_i = 1$ se il termine $t_i \in d$ ($0$ altrimenti), avremo che
$$\begin{align*}
P(R \vert d,q)
&= \frac{P(d \vert R, q) P(R \vert q)}{P(d)}\\
&= \frac{P(f_1,...,f_n \vert R, q) P(R \vert q)}{f(f_1,...,f_n)}\\
\small{(\text{removing constant factors})}&\propto P(f_1,...,f_n \vert R, q)\\
&= \prod_{i=1}^{n} P(f_i \vert R, q)
\end{align*}$$
Dove $P(f_i \vert R, q)$ è la probabilità che il termine $f_i$ è presente in un documento rilevante per la query $q$.
In alternativa $P(f_i \vert R, q)$ è la probabilità che dato un documento rilevante per la query $q$ esso abbiamo il termine $f_i$.

```ad-note
title: Ricapitolando
Sia $d$ un documento con rappresentazione **vettoriale** $v_d = (x_1,...,x_n) \in \lbrace 0,1 \rbrace^n$, tale che $x_i = 1$ se e solo se il termine $t_i$ è presente nel documento $d$.

Allora la probabilità $P(R \vert d,q) = P(R \vert v_d, v_q) \propto P(v_d \vert R, v_q)$ è la probabilità che campionato un documendo $d$ rilevante per la query $q$ esso abbia rappresentazione vettoriale $v_d$.

Per la **Naive Bayes conditional independence assumption** abbiamo che $$P(v_d \vert R, v_q) = \prod_{i=1}^{n} P(x_i \vert R, v_q)$$
```

Per stimare la probabilità $P(t_i \vert R, q)$ che il termine $t_i$ appaia in un documento rilevante avremmo bisogno di **annotatori** che indichino quali documenti sono rilevanti rispetto ad una query $q$, e fare questo per ogni possibile query risulta irragionevole.
Abbiamo quindi bisogno di un modo che in qualche modo aprrossimi $P(t_i \vert R, q)$.

--------
## Approssimazione - Retrieval Status Value
Partiamo col considerare l'[[Appendice Probabilità#Odds|odds]] dell'evento $R \vert d,q$ anziché la sua probabilità.
Tanto ordinare per la probabilità che un documento sia rilevante oppure per il suo odds è **equivalente**.
$$O(R \vert d,q) = \frac{P(R \vert d,q )}{P(\overline{R} \vert d,q )} \propto \frac{P(d \vert R,q)}{P(d \vert \overline{R}, q)} = \frac{P(v_d \vert R,v_q)}{P(v_d \vert \overline{R}, v_q)}$$
Riapplicando la **Naive Bayes conditional independence assumption** $$\frac{P(v_d \vert R,v_q)}{P(v_d \vert \overline{R}, v_q)} = \prod_{i=1}^{n}\frac{P(x_i \; \vert R,v_q)}{P(x_i \; \vert \overline{R}, v_q)}$$
**Spazziamo** poi la produttoria poi tra i termini che appartengono e che non appartengono al documento $d$
$$\prod_{i=1}^{n}\frac{P(x_i \; \vert R,v_q)}{P(x_i \; \vert \overline{R}, v_q)} = \prod_{i \,:\, x_i = 1}\frac{P(x_i \; \vert R,v_q)}{P(x_i \; \vert \overline{R}, v_q)} \cdot \prod_{i \,:\, x_i = 0}\frac{P(x_i \; \vert R,v_q)}{P(x_i \; \vert \overline{R}, v_q)}$$

Aggiungiamo ora una ulteriore semplificazione: assumiamo che per tutti quei termini $t \notin q$ che non appaiono nella query è **equiprobabile** che essi occorrano o meno in un documento rilevante, ovvero $$\forall t \notin q \left[ P(t \vert R,q) = P(t \vert \overline{R},q) \right] \implies \forall t \notin q \left[ \frac{P(t \vert R,q)}{P(t \vert \overline{R},q)} = 1 \right]$$
Data questa semplificazione, nella produttoria "*sopravviveranno*" i soli termini che appartengono alla query
$$\prod_{i=1}^{n}\frac{P(x_i \; \vert R,v_q)}{P(x_i \; \vert \overline{R}, v_q)} = \prod_{t_i \in q \,:\, x_i = 1}\frac{P(x_i \; \vert R,v_q)}{P(x_i \; \vert \overline{R}, v_q)} \cdot \prod_{t_i \in q \,:\, x_i = 0}\frac{P(x_i \; \vert R,v_q)}{P(x_i \; \vert \overline{R}, v_q)}$$


Per snellire la sintassi indichiamo con:
- $p_t = P(t \vert R, q)$ la probabilità che il termine $t$ appaia in un documento rilevante per la query $q$
- $u_t = P(t \vert \overline{R},q)$ la probabilità che un termine $t$ appaia in un documento **non** rilevante per la query $q$

\ | relevant | non relevant
--|--|--
term present | $p_t$ | $u_t$
term not present | $1-p_t$ | $1-u_t$

Ricapitolando avremo $$O(R \vert d,q) = \prod_{t_i \in q \,:\, x_i = 1}\frac{p_i}{u_i} \cdot \prod_{t_i \in q \,:\, x_i = 0}\frac{1-p_i}{1-u_i}$$

Per ogni termine rilevante della query moltiplichiamo e dividiamo il tutto per $$(1-u_i)/(1-p_i)$$ ovvero
$$\begin{align*}
&\prod_{t_i \in q \,:\, x_i = 1}\frac{p_i}{u_i} \cdot \prod_{t_i \in q \,:\, x_i = 0}\frac{1-p_i}{1-u_i} \cdot \left [ \prod_{t_i \in q \,:\, x_i = 1}\frac{1-u_i}{1-p_i} \cdot \prod_{t_i \in q \,:\, x_i = 1}\frac{1-p_i}{1-u_i}  \right]\\
\\
=& \prod_{t_i \in q \,:\, x_i = 1}\frac{p_i(1-u_i)}{u_i(1-p_i)} \cdot \prod_{t_i \in q}\frac{1-p_i}{1-u_i}
\end{align*}$$

Osserviamo quindi che la prima produttoria continua a riferirsi a tutti quei *query-terms* presenti nel documento.
La seconda produtorria riguarda i soli termini della query, perciò **costante** per ogni documento.
Possiamo quindi **ignorare** la seconda produttoria ottenendo
$$O(R \vert d,q) \propto \prod_{t \in q \cap d} \frac{p_t(1-u_t)}{u_t(1-p_t)}$$ ^3cba3b

Per via della monotonicità del logaritmo, possiamo pensare di effetuare il ranking per il **logaritmo** della [[#^3cba3b|formula]] ottenuta.
Tale valore è anche noto con il nome **Retrieval Status Value** (o **RSV**)
$$RSV_d = \log{\prod_{t \in q \cap d} \frac{p_t(1-u_t)}{u_t(1-p_t)}} = \sum_{t \in q \cap d} \log{\frac{p_t(1-u_t)}{u_t(1-p_t)}} = \sum_{t \in q \cap d} c_t$$
dove
$$c_t = \log{\frac{p_t(1-u_t)}{u_t(1-p_t)}} = \log{\frac{p_t}{1-p_t}} - \log{\frac{u_t}{1-u_t}}$$ ^ab2996

```ad-note
title: Osservazione 1
Osservare che:
- $p_t/(1-p_t)$ è l'**odds ratio** dell'evento $t \vert R,q$ ($t$ apparte in un documento rilevante per la query $q$).
- $u_t/(1-u_t)$ è l'**odds ratio** dell'evento $t \vert \overline{R},q$ ($t$ apparte in un documento non rilevante per la query $q$).

Perciò $c_t$ può essere visto come la differenza dei logaritmi dei due precedenti odd ratio.
```

^29c28c

```ad-note
title: Osservazione 2
In base alla [[#^29c28c|osservazione 1]] abbiamo che:
- se $c_t = 1$ allora c'è uguale probabilità che $t$ appaia in un documento rilevante anziché uno non rilevante.
- se $c_t > 1$ è più probabile che il termine $t$ appaia in un documento rilevante anziché uno non rilevante.
- se $c_t < 1$ è più probabile che $t$ appaia in un documento non rilevante anziché uno rilevante.
```

### Stimare $p_t$ e $u_t$
Che informazione possiamo usare per stimare la porbabilità che un documento $t$ appaia in un documento rilevante o uno non rilevante?

Ci sono due possibili scenari:
1. abbiamo un training set che (dato da degli annotatori o da un sistema di feedback).
2. non abbiamo alcuna informazione a disposizione riguardo la rilavanza dei documenti.

#### Caso 1
Nel caso in cui abbiamo a disposizione un training set possiamo applicare un approccio **frequentista** per stimare l'[[#^ab2996|odds ratio]] $c_t$.

Siano:
- $N$ il numero **totale** di documenti nella collezione.
- $R$ il numero di documenti **rilevanti** per la nostra query.
- $r_i = \vert \lbrace d \in R : t_i \in d \rbrace \vert$ il numero di documenti <u>rilevanti</u> tali che contengono il termine $t_i$.
- $\text{df}_{t_i}$ (*document frequency*) il numero di documenti che contengono il termine $t_i$.

Dato che $p_i$ e $u_i$ sono **bernoulliane**, possiamo usare lo [[Stimatore di Massima Verosimiglianza]]
- $$\hat{p}_i = \frac{r_i}{R}$$
- $$1-\hat{p}_i = \frac{R - r_i}{R}$$
- $$\hat{u}_i = \frac{\text{df}_{t_i} - r_i}{N - R}$$
- $$1 - \hat{u}_i = \frac{(N - R) - (\text{df}_{t_i} - r_i)}{N - R}$$
- $$\hat{c}_i = \log{\frac{\hat{p}_i(1-\hat{u}_i)}{\hat{u}_i(1-\hat{p}_i)}} = \log{\frac{\frac{r_i}{R - r_i}}{\frac{\text{df}_{t_i} - r_i}{(N - R) - (\text{df}_{t_i} - r_i)}}}$$



------
## Problemi
BIM è estremamente semplice da applicare per via del suo approccio **bag-of-words**.
Tale approccio però è anche svantaggioso perché non tiene conto della **frequenza** con cui appaiono le parole all'interno dei documenti.
Un'altro svantaggio è che non prende in considerazione nemmeno la lunghezza dei documenti.