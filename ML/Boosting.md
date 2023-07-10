Il boosting consiste nello sfruttare gli errori fatti dai predittori addestrati fino a un certo punto, per creare un nuovo predittore migliore che vada meglio dove gli altri vanno male.

Per fare ciò, si utilizza una versione **pesata** del dataset.
Vengono quindi pesati maggiormente gli elementi per i quali i predittori precedenti sbagliano molto.
In fine per fare une predizione si fa una **media pesata** delle singole predizioni, pesato in base a come si sono comportati i singoli.

Quindi siano $y_1, ..., y_M$ i predittori, con qualità $\alpha_1 ,..., \alpha_M$.
Per esempio, per fare classificazione, possiamo considerare la media pesata $$Y_M(x) = \text{sign}\left( \sum_{m=1}^{M}\alpha_m \cdot y_m(x)\right)$$

![[ML_13_1.png]]

# Adaboost
Siamo nel contesto della **classificazione binaria**.
Abbiamo quindi un dataset $(\mathbf{X}, \mathbf{t})$ con $n$ elementi, e target in codifica $\lbrace -1, 1 \rbrace$.

L'algoritmo di **adaboost**, ad ogni tempo $t$, preserva un vettore di pesi $$w^{(t)}(\mathbf{X}) = (w^{(t)}_1,..., w^{(t)}_n)$$ inizializzati come $$w^{(0)}(\mathbf{X}) = \left( \frac{1}{n}, ..., \frac{1}{n} \right)$$
Ad ogni tempo $t = 1, ..., M$, addestriamo un **weak learner** $y_t(x)$ sul dataset in modo da **minimizzare** la sbagliata classificazione rispetto ai pesi $w^{(t)}(\mathbf{X})$.

Indichiamo con $$\pi^{(t)} = \frac{\sum_{x_i \in \mathcal{E}^{(t)}}w_i^{(t)}}{\sum_{i=1}^{n}w_i^{(t)}}$$ dove $\mathcal{E}^{(t)}$ sono gli elementi **classificati male** dal predittore $y_t$.