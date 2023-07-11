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


# Why dose Adaboost work? #todo
La tecnica di boosting **Adaboost** in realtà **minimizza** una [[From Learning to Optimization|funzione loss]].
Supponiamo di avere un semplice classificatore del tipo $y(x) = \text{sign}f(x)$.

Noi sappiamo che la [[Some Loss Functions#0/1 Loss|0/1 loss]] 
$$L(y, t) := \begin{cases}
0 &\text{if } y \cdot t > 0\\
1 &\text{otherwise}
\end{cases}$$
ha la problematica che **non** è [[Convessità|convessa]] e ha **sempre** gradiente 0.

# Additive Model