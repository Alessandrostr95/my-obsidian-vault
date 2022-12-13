Abbiamo visto come poter modellizzare il numero di occorrenze di un termine in un documento con una [[Introducing term frequency in BIM#^2ee5f6|binomiale]].
Per valori di $L$ grandi, possiamo approssiamare questa binomiale come una [[Distribuzioni#Poisson|poisson]] di parametro $\lambda = L\tilde{p}_t$
$$\text{Binom}(L, \tilde{p}) \approx \text{Poisson}(\lambda = L\tilde{p}) \implies P(d_i = k) \approx e^{-L\tilde{p_i}}\frac{(L\tilde{p}_i)^k}{k!}$$

Sorge ora il problema:
> come poter stimare il parametro $\lambda = L \tilde{p}_t$?

Come stimatore usiamo la [[Random Sample#Media campionaria|media campionaria]] del numero di occorrenze del termine $t_i$ nella collazione di documenti.
Ovvero $$\gamma_t = \frac{\sum_{d \in c} \text{tf}_{d,t}}{\vert c \vert}$$
- $$P(d_i = n_i) \sim \text{Poisson}(\gamma_i)(n_i) = \frac{e^{-\gamma_i}\gamma_i^{n_i}}{n_i !}$$
- $$P(d_i = 0) \sim \text{Poisson}(\gamma_i)(0) = e^{-\gamma_i}$$

Riducendoci ai soli documenti rilventi, possiamo calcolare la media campionaria dei numero di occorrenze del termine $t_i$ all'interno dei documenti rilevanti.
Ovvero $$\rho_t = \frac{\sum_{d \in R(q)} \text{tf}_{d,t}}{\vert R(q) \vert}$$ dove $R(q)$ è l'insieme dei documenti rilevanti per la query $q$.
- $$P(d_i = n_i \vert R,q) \sim \text{Poisson}(\gamma_i)(n_i) = \frac{e^{-\rho_i}\rho_i^{n_i}}{n_i !}$$
- $$P(d_i = 0 \vert R,q) \sim \text{Poisson}(\rho_i)(0) = e^{-\rho_i}$$

```ad-bug
Come faccio a calcolare $R(q)$ se fino a mo abbiamo detto che non so nulla riguardo a quali sono i documenti rilevanti???!!!
```

Come sonseguenza avremo che
$$\begin{align*}
RSV_d
&= \sum_{t \in q}\log{\frac{P(d_t = n_t \vert R,q) P(d_t = 0)}{P(d_t = n_t)P(d_t = 0 \vert R, q)}}\\
&= \sum_{t \in q} \log{\frac{\dfrac{e^{-\rho_t}\rho_t^{n_t}}{n_t !} \cdot e^{-\gamma_t}}{e^{-\rho_t} \cdot \dfrac{e^{-\gamma_t}\gamma_t^{n_t}}{n_t !}}}\\
&= \sum_{t \in q} \log{\frac{\rho_t^{n_t}}{\gamma_t^{n_t}}}\\
&= \sum_{t \in q} n_t \log{\frac{\rho_t}{\gamma_t}}
\end{align*}$$

