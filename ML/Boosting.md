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

```ad-attention
Se capita che $\pi^{(t)} > 1/2$, prendiamo in considerazione il **classificatore inverso**, quello che associa il contrario delle predizioni.
Così facendo avremo che $\pi^{(t)} < 1/2$.
```


```ad-tldr
Il valore $\pi^{(t)}$ può essere interpretato la probabilità che che un qualsiasi elemento $x$ del dataset sia **classificato male**, assumendo che $x$ sia campionato con distribuzione $$\frac{w_i^{(t)}}{\sum_{j=1}^{n}w_j^{(t)}}$$
```

Calcoliamo la **confidenza** del $t$-esimo classificatore, come la quantità $$\alpha_t = \frac{1}{2}\log\frac{1 - \pi^{(t)}}{\pi^{(t)}} > 0$$

Per ogni elemento $\mathbf{x}_i$ del dataset aggiorniamo quindi il suo peso nel seguente modo
$$w_i^{(t+1)} = w_i^{(t)} \cdot \exp\left( -\alpha_t t_i y_t(\mathbf{x}_i)\right)$$
Ciò equivale a 
$$w_i^{(t+1)} = \begin{cases}
w_i^{(t)} \cdot e^{\alpha_t} > w^{(t)}_i &\text{if } \mathbf{x}_i \in \mathcal{E}^{(t)}\\
w_i^{(t)} \cdot e^{-\alpha_t} < w^{(t)}_i &\text{if }
\end{cases}$$
Perciò, se l'elemento $\mathbf{x}_i$ è classificato bene, allora il suo peso diminuisce, altrimenti aumenta.

Infine ==**normalizziamo**== l'insieme dei pesi dividendoli tutti per $\sum_{i=1}^{n}w_i^{(t+1)}$, in modo tale da ottenere una **distribuzione**.

In fine, la predizione secondo **Adabust**, viene fatta facendo una media pesata delle predizioni in base alle singole confidenze
$$y(\mathbf{x}) = \text{sign}\left( \sum_{t=1}^{M}\alpha_t \cdot y_t(\mathbf{x})\right)$$

![[ML/img/ML_13_2.png]]

![[ML/img/ML_13_3.png]]

