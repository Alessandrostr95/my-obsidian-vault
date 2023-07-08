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

Per calcolare la [[Maximum a Posteriori - Bayesian Approach#^8ea705|probabilità a priori]] $p(C_k)$ possiamo usare uno [[Stimatore di Massima Verosimiglianza]] $$p(C_k) = \frac{\vert D_k \vert}{\vert D_1 \vert + \vert D_2 \vert}$$ con $k = 1,2$. ^bf4983

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

## Deriving Posterior distribution $p(C_k \vert d)$
Consideriamo il caso generale di punti $\mathbf{x}$ in uno **spazio delle features**, anziché di documenti.
Infatti nel modello **bag of words** infatti i documenti $d$ sono visti come **punti** all'interno dello spazio dei termini.

Applicando la regola di bayes e quella delle **probabilità totali** calcoliamo
$$p(C_1 \vert \mathbf{x}) = \frac{p(\mathbf{x} \vert C_1)p(C_1)}{p(\mathbf{x})} = \frac{p(\mathbf{x} \vert C_1)p(C_1)}{p(\mathbf{x} \vert C_1)p(C_1) + p(\mathbf{x} \vert C_2)p(C_2)} = \frac{1}{1+\frac{p(\mathbf{x} \vert C_2)p(C_2)}{p(\mathbf{x} \vert C_1)p(C_1)}}$$

Per comodità indichiamo con $$a = \log\frac{p(\mathbf{x} \vert C_1)p(C_1)}{p(\mathbf{x} \vert C_2)p(C_2)} = \log\frac{p(\mathbf{x} \vert C_1)}{p(\mathbf{x} \vert C_2)}$$

Possiamo quindi riscrivere le probabilità a posteriori come 
$$p(C_1 \vert \mathbf{x}) = \frac{1}{1+e^{-a}}$$
$$p(C_2 \vert \mathbf{x}) = 1 - \frac{1}{1+e^{-a}} = \frac{1}{1 + e^a}$$

Tale funzione è anche nota come **logistic funciton** o **sigmoide** $$\sigma(x) = \frac{1}{1+e^{-x}}$$ ^8e0f70

![[ML_07_1.png]]

```ad-tldr
Alcune proprietà molto utili della **funzione logistica** sono:
- $\sigma(-x) = 1 - \sigma(x)$
- $\frac{d}{dx}\sigma(x) = \sigma(x)(1 - \sigma(x)) = \sigma(x)\cdot\sigma(-x)$
```

^0f34d2

# Caso Multiclasse - Softmax Regression
Generalizzando per $K > 2$ abbiamo la seguente formula per la probabilità a posteriori
$$p(C_k \vert \mathbf{x}) = \frac{p(\mathbf{x} \vert C_k)p(C_k)}{\sum_{i=1}^{K} p(\mathbf{x} \vert C_i)p(C_i)}$$ ^b61e37

Definiamo per ogni $k = 1,...,K$ la quantità $$a_k(\mathbf{x}) = \log{(p(\mathbf{x} \vert C_k)p(C_k))} = \log{p(\mathbf{x} \vert C_k)} + \log p(C_k)$$

Possiamo quindi riscrivere la [[#^b61e37|precedente probabilità]] come segue $$p(C_k \vert\mathbf{x}) = \frac{e^{a_k(\mathbf{x})}}{\sum_{i=1}^{K} e^{a_i(\mathbf{x})}} = s(a_{k}(\mathbf{x}))$$
Questa probabilità è anche nota come **softmax function** (o **funzione esponenziale normalizzata**), definita come
$$s_i: \mathbb{R}^n \to \mathbb{R}$$
$$s_i(\mathbf{x}) = \frac{e^{x_i}}{\sum_{j=1}^{n}e^{x_j}}$$ ^3e232f

Questa funzione può essere vista come una **generalizzazione** della [[#^0f34d2|funzione sigmoide]], oppure come una versione **smoothed** della funzione $\max$.