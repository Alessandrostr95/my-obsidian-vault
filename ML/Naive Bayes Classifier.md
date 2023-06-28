I [[ML/Language Models|Language Models]] possono essere utilizzati per **classificare** dei documenti in una o più classi.
Più precisamente, date delle **classi** $C_1, C_2$ e un documento $d$ vogliamo stimare le seguenti probabilità:
- $P(C_1 \vert d)$: la probabilità che campionato il documento $d$ esso appartenga alla classe $C_1$.
- $P(C_2 \vert d)$: la probabilità che campionato il documento $d$ esso appartenga alla classe $C_2$.

Per stimare tali probabilità, possiamo utilizzare la regola di Bayes $$p(C_k \vert d) \propto p(d \vert C_k) \cdot p(C_k) \;\; \forall k =1,2$$
```ad-tldr
title: Osservazione
La formula di Bayes in realtà è $$p(C_k \vert d) = \frac{p(d \vert C_k) \cdot p(C_k)}{p(d)}$$ Assumiamo però che i documenti $d$ sono campionati **u.a.r.**, perciò $p(d)$ è uniforme.
```

Assumiamo di avere a disposizione una collezione di documenti $D_1$ di documenti appartenenti alla classe $C_1$, e una collezione $D_2$ di documenti appartenenti alla classe $C_2$.

Per calcolare la [[Maximum a Posteriori - Bayesian Approach#^8ea705|probabilità a priori]] $p(C_k)$ possiamo usare uno [[Stimatore di Massima Verosimiglianza]] $$p(C_k) = \frac{\vert D_k \vert}{\vert D_1 \vert + \vert D_2 \vert}$$ con $k = 1,2$.

Per quanto $p(d \vert C_k)$ osserviamo che, in accordo al [[Bag of words model - Term Frequency tf|modello bag-of-words]] $d$ può essere visto come l'**insieme** dei termini che lo compongono $$d = \lbrace t_1, t_2,..., t_{n_d} \rbrace$$
Applicando la **chain rule** abbiamo che
$$\begin{align}
p(d \vert C_k)
&= p(t_1, ...,t_{n_d} \vert C_k)\\
&= p(t_1, \vert t_2, ...,t_{n_d}, C_k) \cdot p(t_2, ...,t_{n_d} \vert C_k)\\
&= p(t_1, \vert t_2, ...,t_{n_d}, C_k) \cdot p(t_2, \vert t_3, ...,t_{n_d}, C_k) \cdot p(t_3, ...,t_{n_d} \vert C_k)\\
&= \prod_{i=1}^{n_d-1} p(t_i \vert t_{i+1}, ..., t_{n_d},C_k)
\end{align}$$

Calcolare la precedente probabilità risulta essere particolarmente complesso.
Perciò per semplicità è preferibile assumere che i termini sono **indipendenti** $$p(t_i \vert t_j, C_k) = p(t_i \vert C_k)$$

Perciò possiamo stimare $$p(d \vert C_k) \approx \prod_{i=1}^{n_d}p(t_i \vert C_k)$$
```ad-tldr
title: Calcolare $p(t \vert C_k)$
Per calcolare $p(t \vert C_k)$ facciamo riferimento ai [[ML/Language Models|Language Models]].
Possiamo farlo per **massima verosimiglianza** $$p(t \vert C_k) \approx \frac{\text{cf}_t(D_k)}{\vert D_k \vert}$$ dove $\text{cf}_t(D_k)$ è la [[Scoring, term weighting & the vector space model#^433f1d|collection frequency]] del termine $t$ nella collezione $D_k$.
Oppure possiamo stimarlo usando lo [[ML/Language Models#Bayesian learning of Language Models|smoothing di dirichelet]].
```


