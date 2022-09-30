# Clustering in Pertubation-Stable Instances
Come in [[6 - Clustering in Approximation-Stable Instances]], introduciamo un nuovo concetto di **perturbazione** per le istanze di $k$-median clustering.

Non in tutti i casi i punti appartengono ad uno **spazio geometrico**.
Infatti quasi sempre si vogliono classificare punti che rappresentano oggetti, proteine, documenti, ecc... e quindi si definiscono funzioni di distanza **euristiche**, che non possono essere prese alla lettera.

Dato che quindi si possono definire differenti funzioni di distanza, ci aspettiamo che quelle "*buone*" non differiscano tra di loro di molto (diciamo al più di un fattore **costante moltiplicativo**).

Se tali funzioni di distanza sono "*buone*", inoltre, "*più o meno*" identificheranno lo stesso clustering come ottimo.

Innanzitutto, assumiamo che il **clustering target** $C^*_1, ..., C^*_k$ è l'unica istanza che **ottimizza la [[6 - Clustering in Approximation-Stable Instances#^cc98b0|funzione obiettivo]]**. ^85e465

> **Def. ($\gamma$-perturbation)**
> Una $\gamma$**-perturbazione** di una istanza $(X, d)$ del $k$*-median clustering problem* è una nuova istanza $(X, \tilde{d})$ dove le distanze tra ogni coppia di punti $x,y \in X$ sono scalate $$\tilde{d}(x,y) = \sigma_{xy} \cdot d(x,y)$$ di un fattore **arbitrario** $\sigma_{x,y} \in \left[1, \gamma\right]$.

^490cba

Riuscire a risolvere il problema, ovvero identificare il clustering ottimo $C^*_1, ..., C^*_k$, sotto una funzionde distanza $d$ **empirica** "*buona*", suggerisce che la soluzione è **insensibile** a piccole permutazioni di $d$.

In altre parole, la sensibilità dell'output ai dettagli della funzione distanza $d$ suggerisce che è stata posta la domanda sbagliata (ricordiamo che la soluzione è [[#^85e465|unica]]).

> **Def. ($\gamma$-stable instace)**
> Per ogni $\gamma \geq 1$, diremo che una istanza $(X,d)$ è $\gamma$**-stable** se a fronte di una qualsiasi $\gamma$[[#^490cba|-perturbazione]] $(X, \tilde{d})$, la soluzione **ottima** $C^*_1, ..., C^*_k$ rimane invariata.

^a9d2c7

Il nostro obiettivo è quello di dimostrare che per ogni $\gamma$ <u>sufficientemente grande</u> esiste un **algoritmo polinomiale** che ricava la soluzione ottima per ogni istanza $\gamma$[[#^a9d2c7|-stable]].

```ad-warning
Ricorda che tale problema è **NP-Hard** nelle istanze peggiori.
```


```ad-summary
- L'[[6 - Clustering in Approximation-Stable Instances#^d2b2c2|Approximation stability]] formalizza l'idea che un clustering con valore **quasi ottimo** rispetto alla funzione obiettivo, deve essere **strutturalmente simile** alla soluzione ottima.
- La [[#^a9d2c7|Pertubation Stability]] invece formalizza l'intuizione che la soluzione ottima deve essere **robusta** a fronte di pertubazioni della funzione di distanza.
```

---------
