Nel [[Probabilistic Ranking Principle]] sfruttavamo la regola di [[Appendice Probabilità#Bayes' Rule|regola di Bayes]] per esprimere la probabilità che un documento sia rilevante rispetto a una query in funzione della rappresentazione del documento.

> $$P(R \vert d,q) = \frac{P(d \vert R, q) P(R \vert q)}{P(d \vert q)} = \frac{P(d \vert R, q) P(R \vert q)}{P(d)}$$
> dove
> - $P(d \vert R, q)$ è la probabilità di campionare un documento $d$  dal sottoinsieme dei **soli documenti rilevanti** rispetto a $q$.
> - $P(R \vert q)$ è la probabilità di campionare a caso un qualsiasi documento rilevante rispetto a $q$.
> - $P(d)$ è la probabilità che il documento $d$ sia campionato dalla collezione. Osservare che $P(d \vert q) = P(d)$ perché a priori un campionamento di un documento è indipendente dalla query $q$.

Allo stesso modo possiamo definire un **modello generatico** che esprime la stessa probabilità in funzione della query $q$.

$$P(R \vert d, q) = \frac{P(q \vert R,d) P(R \vert d)}{P(q \vert d)} = \frac{P(q \vert R,d) P(R \vert d)}{P(q)}$$
dove:
- $P(q \vert R,d)$ è la probabilità che dato un documento $d$ risaputo essere **rilevante** per l'information need, qual è la probabilità che $q$ sia la query fatta che ha **generato** $d$.
- $P(R \vert d)$ è la probabilità che il documento $d$ sia rilevante.
- $P(q)$ è la probabilità che venga fatta la query $q$ (dato come contesto un information need).

## Language Model
Assumiamo di voler **generare** termine per termine un documento $d$.

Un **Language Model** $M_d$ per un documento $d$ definisce le regole da applicare per **generare** step by step ogni termine del documento $d$.

Possiamo pensare ad $M_d$ come una sorta di **automa a stati finiti**, doce le transizione da stato a stato sono date dai termini.

![](./img/IR_language_model_1.png)

Osserviamo che se stiamo in uno stato in cui abbiamo appena letto un verbo, ci sono vaire probabilità di leggere un `oggetto` anziché un `articolo` anziché un `aggettivo`, tutte dipendenti dal verbo (**stato**) appena letto.

Perciò è buono avere una **distribuzione** su ogni trasnizione che parte da un dato stato.

![](./img/IR_language_model_2.png) ^85bf9e

Osserviamo che in questo caso $M_d$ è una **catena di Markov** in cui la probabilità di generare il documento $d$ è data dalla **Random Walk** (che parte dallo stato $0$) tale che il passo $i$-esimo della passeggiata aleatoria corrisponde al termine $i$-esimo di $d$.

> **Esempio**
> Consideriamo il [[#^85bf9e|precedente]] language model $M_d$ e cerchiamo di generare il documento $$d = \text{"I am Frodo you saw Sam.} \texttt{[STOP]} \text{"}$$
> Partendo dallo stato $X = 0$ avremo la seguente probabilità
> $$\begin{align*}
P(d \vert M_d) &=\\
&= P(\text{"I"}|X = 0) \cdot P(\text{"am"}|X = 1) \cdot P(\text{"Frodo"}|X = 2) \cdot P(\text{"you"}|X = 0) \cdot P(\text{"saw"}|X = 3) \cdot P(\text{"Sam"}|X = 2) \cdot P(\texttt{[STOP]}|X = 0)\\
&= 0.4 \cdot 0.5 \cdot 0.8 \cdot 0.4 \cdot 1 \cdot 0.2 \cdot 0.2\\
&= 0.00256
\end{align*}$$

Un modello parecchio più semplificato è quello in cui ho un **unico stato** e definisco quindi una distribuzione sull'occorrenza dei termini (in generale, indipendentemente da ciò che ho letto prima).

![](./img/IR_language_model_3.png)

