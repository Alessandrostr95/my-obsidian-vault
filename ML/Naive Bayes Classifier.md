I [[ML/Language Models|Language Models]] possono essere utilizzati per **classificare** dei documenti in una o più classi.
Più precisamente, date delle **classi** $C_1, C_2$ e un documento $d = \lbrace t_1, ..., t_{n_d} \rbrace$ vogliamo stimare le seguenti probabilità:
- $P(C_1 \vert d)$: la probabilità che campionato il documento $d$ esso appartenga alla classe $C_1$.
- $P(C_2 \vert d)$: la probabilità che campionato il documento $d$ esso appartenga alla classe $C_2$.

Per stimare tali probabilità, possiamo utilizzare la regola di Bayes $$p(C_k \vert d) \propto p(d \vert C_k) \cdot p(C_k) \;\; \forall k =1,2$$
```ad-tldr
title: Osservazione
La formula di Bayes in realtà è $$p(C_k \vert d) = \frac{p(d \vert C_k) \cdot p(C_k)}{p(d)}$$ Assumiamo però che i documenti $d$ sono campionati **u.a.r.**, perciò $p(d)$ è uniforme.
```

Assumiamo di avere a disposizione una collezione di documenti $D_1$ di documenti appartenenti alla classe $C_1$, e una collezione $D_2$ di documenti appartenenti alla classe $C_2$.

Per calcolare la [[Maximum a Posteriori - Bayesian Approach#^8ea705|probabilità a priori]] $p(C_k)$ possiamo usare uno [[Stimatore di Massima Verosimiglianza]] $$p(C_k) = \frac{\vert D_k \vert}{\vert D_1 \vert + \vert D_2 \vert}$$ con $k = 1,2$.



