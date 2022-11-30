---
date: 2022-11-30
draft: true
---
- $s = w_1 ... w_n$ una frase
- $t$ un albero della interpretazione di $s$

Voglio trovare
$$\begin{align*}
\hat{t}
&= \arg \max_{t} P(t \vert s)\\
&= \arg \max_{t} \frac{P(s \vert t) \cdot P(t)}{P(s)}\\
&\approx \arg \max_{t} \underbrace{P(s \vert t)}_{1} \cdot P(t)\\
&= \arg \max_{t} P(t)
\end{align*}$$


Vogliamo quindi calcolare la probabilità dell'**albero** $t$ (inteso come **albero di produzione**).
Siccome ogni sottoalbero non è **indipendente**, possiamo pensare di assumere l'indipendenza.
Inoltre, possiamo trovare una stessa produzione a **profondità differenti** di $t$, perciò bisognerebbe tenere conto anche di questo.
Per semplicità però, assumiamo che siano equivalenti ed indipendenti.

Stiamo quindi
$$P(t) \approx \prod_{\alpha \to \beta \in t} P(\alpha \to \beta \vert \alpha) \stackrel{MLE}{=} \prod_{\alpha \to \beta \in t} \frac{\# \lbrace \alpha \to \beta \in t \rbrace}{\# \lbrace \alpha \to \beta \in \mathcal{G} \rbrace}$$
