Abbiamo discusso nel [[Probabilistic Ranking Principle|PRP]] che abbiamo bisogno di **stimare** la probabilità che un documento restitutio sia rilevante.

L'apporccio più semplice è noto come **Bianary Independent Model**.

In questo modello tutti i documenti (incluse le query) vengono rappresentati come dei **vettori binari** in cui la cella inerente al termine $t$ è pari ad 1 se il termine occorre almeno una volta nel documento, 0 altrimenti.

Con tale rappresentazione possono facilmente esserci documenti con la stessa rapressentazione (o comunque molto molto simile).
Inoltre non si tiene conto della **frequenza** dei termini all'interno dei documenti (*term frequency*).

Un'assunzione importante del **BIM** è che le occorrenze dei termini in un documento sono **indipendenti** tra loro.
In realtà nel linguaggio naturale non è così, c'è dipendenza tra i termini in una frase, però all'atto pratico assumere l'indipendenza tra i termini ci aiuta moltissimo nei calcoli e in fine come modello funziona abbastanza bene.

Grazie a questa assunzione possiamo ora **stiamare** la probabilità che un documento $d$ sia rilevante data una query $q$, come discusso nel [[Probabilistic Ranking Principle#^87f37c|PRP]].
Ovvero, rappresentando un documento $d$ come un vettore di features $(f_1,...,f_n)$ avremo che
$$\begin{align*}
P(R \vert d,q)
&= \frac{P(d \vert R, q) P(R \vert q)}{P(d)}\\
&= \frac{P(f_1,...,f_n \vert R, q) P(R \vert q)}{f(f_1,...,f_n)}\\
\small{(\text{removing constant factors})}&\propto P(f_1,...,f_n \vert R, q)\\
&= \prod_{i=1}^{n} P(f_i \vert R, q)
\end{align*}$$
Dove $P(f_i \vert R, q)$ è la probabilità che il termine $f_i$ è presente nel documento $d$ **sapendo** che $d$ è rilevante rispetto alla query $q$.
In alternativa $P(f_i \vert R, q)$ è la probabilità che dato un documento rilevante per la query $q$ esso abbiamo il termine $f_i$.

```ad-note
Sia $d$ un documento con rappresentazione **vettoriale** $v_d \in \lbrace 0,1 \rbrace^n$, tale che $v_d\left[i\right] = 1$ se e solo se il termine $t_i$ è presente nel documento $d$.

Allora la probabilità $P(R \vert d,q) = P(R \vert v_d, v_q) \propto P(v_d \vert R, v_q)$ è la probabilità che campionato un documento $d$ rilevante per la query $q$ esso abbia rappresentazione vettoriale $v_d$.

Per la **Naive Bayes conditional independence assumption** abbiamo che $$P(v_d \vert R, v_q) = \prod_{i=1}^{n} P(v_d \left[ i \right] \vert R, v_q)$$
```
