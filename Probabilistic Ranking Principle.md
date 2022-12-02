Definiamo una prima nozione di **rilevanza probabilistica**.

Dato un documento $d$ e una query $q$ definiamo la **variabile aleatoria binaria**
$$R_{d,q}\begin{cases}
1 &\text{se } d \text{ è rilevante rispetto a } q\\
0 &\text{altrimenti}
\end{cases}$$

Possiamo quindi *iterpretare* la probabilità $P(R_{d,q} = 1) = P(R = 1 \vert d,q)$ come la probabilità che un qualsiasi utente **a caso** giudichi $d$ **rilevante** rispetto alla query $q$.
Più formalmente sia $U$ lo spazio degli utenti, definiamo quindi
$$P(R_{d,q} = 1) = \sum_{u \in U} P(R_{d,q} = 1 \vert u) P(u)$$
dove $P(R_{d,q} = 1 \vert u)$ è la probabilità che l'utente $u$ dentifichi il documento $d$ come rilevante ripsetto alla query $q$.

Una buona idea è quella di ritornare un documento $d$ come risultato di una query $q$ se la probabilità che sia inerente è **maggiore** di $1/2$, ovvero se $P(R_{d,q} = 1) > 0.5$.
In termini di [[Appendice Probabilità#Odds|odds]] se $$O(\lbrace R_{d,q} = 1 \rbrace) = \frac{P(R_{d,q} = 1)}{P(R_{d,q} = 0)} > 1$$
```ad-info
Per semplicità, indichiamo con $R \vert d,q$ l'evento $\lbrace R_{d,q} = 1 \rbrace$.
Analogamente indichiamo con $\overline{R} \vert d,q$ l'evento $\lbrace R_{d,q} = 0 \rbrace$.
```

Il **Probabilistic Ranking Principle** (in breve **PRP**) consiste nell'effettuare il ranking in base alle rispettive probabilità che i documenti ritornati siano rilevanti rispetto alla query: ordiniamo i documenti rispetto a $P(R \vert d,q)$.

Usando la [[Appendice Probabilità#Bayes' Rule|regola di Bayes]] abbiamo quindi che
$$P(R \vert d,q) = \frac{P(d \vert R, q) P(R \vert q)}{P(d \vert q)} = \frac{P(d \vert R, q) P(R \vert q)}{P(d)}$$
dove
- $P(d \vert R, q)$ è la probabilità di campionare un documento $d$  dal sottoinsieme dei soli documenti **rilevanti** rispetto a $q$.
- $P(R \vert q)$ è la probabilità di campionare a caso un qualsiasi documento rilevante rispetto a $q$.
- $p(d)$ è la probabilità che il documento $d$ sia campionato dalla collezione.

Una prima assunzione che stiamo facendo è che $d$ è campionato in maniera **indipendente** rispetto alla query $q$.

[DA FINIRE]
