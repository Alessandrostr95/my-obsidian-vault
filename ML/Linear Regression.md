Supponiamo di avere un dataset $\mathcal{T} = (X,t) = \lbrace (x_1,t_1), ..., (x_n, t_n) \rbrace \subseteq \mathbb{R^d} \times \mathbb{R}$.
Ovvero ad ogni vettore $x_i \in \mathbb{R}^d$ è associato un valore reale (il **target**) $t_i \in \mathbb{R}$.

Supponiamo che i valori target seguano una **funzione** (*sconosciuta*), per esempio $\sin(2\pi x)$, più un **rumore** distribuito come $\varepsilon \sim \mathcal{N}(0, \sigma^2)$. 
Quindi $$t_i = \sin(2\pi x_i) + \varepsilon_i$$
![[ML/img/ML_04_2.png]] ^db2e08

L'obiettivo della **regressione lineare** è quella di approssimare al meglio possibile la **relazione deterministica** $t = \sin(2\pi x)$, in base all'analisi dei dati presenti nel traning set $\mathcal{T}$.

L'approccio principale è quello di approssimare la relazione deterministica mediante un **polinomio** di grado arbitrale $m > 0$, del tipo $$y(\mathbf{x},\mathbf{w}) = w_0 + w_1 \mathbf{x} + w_2 \mathbf{x}^2 + ... + w_m \mathbf{x}^m = \sum_{j=0}^{m}w_j\mathbf{x}^j$$ dove $\mathbf{w} = (w_0, w_1, ..., w_m)$ è il vettore di **coefficienti** che si desidera ottenere, mentre $\mathbf{x} = (x_1, ..., x_d) \in \mathbb{R}^d$ è un elemento del dominio $\mathcal{X}$ del quale si desidera ottenere il valore target.

In maniera più generale possiamo riscriverlo come $$y(\mathbf{x},\mathbf{w}) = \sum_{j=0}^{m}w_j \phi_j(\mathbf{x})$$ dove $\phi_j(\mathbf{x}) = \mathbf{x}^j$.

In pratica stiamo mappando gli elementi del dominio $\mathcal{X} \subseteq \mathbb{R}^d$ in punti dello spazio $\mathbb{R}^{m+1}$, tramite l'applicazione di $m+1$ funzioni $\Phi = (\phi_0, \phi_1, ..., \phi_m)$ dette **base functions** (**funzioni base**).
$$\Phi: \mathbb{R}^d \to \mathbb{R}^{m+1}$$
$$\phi_j : \mathbb{R}^d \to \mathbb{R} \;\; \forall i=0,1,...,m$$ ^ba76a0

$$\mathbf{X} =
\begin{pmatrix}
\mathbf{x_1}\\
\mathbf{x_2}\\
\vdots\\
\mathbf{x_n}
\end{pmatrix} =
\begin{pmatrix}
x_{1,1} &x_{1,2} &... &x_{1,d}\\
x_{2,1} &x_{2,2} &... &x_{2,d}\\
\vdots &\vdots &\ddots &\vdots\\
x_{n,1} &x_{n,2} &... &x_{n,d}
\end{pmatrix}$$

$$\Phi(\mathbf{X}) =
\begin{pmatrix}
\Phi(\mathbf{x_1})\\
\Phi(\mathbf{x_2})\\
\vdots\\
\Phi(\mathbf{x_n})
\end{pmatrix} =
\begin{pmatrix}
\phi_0(\mathbf{x_1}) &\phi_1(\mathbf{x_1}) &... &\phi_m(\mathbf{x_1})\\
\phi_0(\mathbf{x_2}) &\phi_1(\mathbf{x_2}) &... &\phi_m(\mathbf{x_2})\\
\vdots &\vdots &\ddots &\vdots\\
\phi_0(\mathbf{x_n}) &\phi_1(\mathbf{x_n}) &... &\phi_m(\mathbf{x_n})\\
\end{pmatrix}$$




Abbiamo bisogno di proiettare le dimensioni da $d$ ad $m+1$ (generalmente più grande) per il semplice motivo che la dimensione $d$ potrebbe risultare essere troppo o troppo poco espressiva.
Infatti se consideriamo infatti i polinomi di dimensione solo $d$ potrebbero accadere due cose:
1. $d$ è una dimensione troppo grande, e il polinomio risultante rischia di passare attraverso tutti i punti del dataset, ottenendo [[From Learning to Optimization#Problemi con $ mathcal{H}$ grande|overfitting]]. ![[ML/img/ML_04_3.png]]
2. $d$ è una dimensione troppo piccola, e il polinomio risulta avere brutte prestazioni rispetto al dataset.

```ad-warning
Osserviamo che $\phi_j$ può essere una qualsiasi funzione, anche **non lineare**. Perciò anche se la combinazione è **non lineare** in $\mathbf{x}$ è però **lineare in** $\mathbf{w}$. 
```

# Regression Loss
Ciò che si vule fare è sempre trovare o [[From Learning to Optimization|apprendere]] un valore dei coefficienti $\mathbf{w}$ che **minimizzi** una qualche funzione di [[Prediction Risk|Loss o Error]].

Una delle funzioni di loss più comuni per la regressione lineare è la [[Some Loss Functions#Quadratic Loss|quadratic loss]], ovvero lo **scarto quadratico**.

$$E(\mathbf{w}) = \frac{1}{2}\sum_{i=1}^{n}\left(t_i - y(\mathbf{x}_i, \mathbf{w})\right)^2 = \frac{1}{2}\sum_{i=1}^{n}\left(t_i - \sum_{j=0}^{m}w_j\phi_j(\mathbf{x})\right)^2 = \frac{1}{2}\sum_{i=1}^{n}\left(t_i - \mathbf{w}^T\mathbf{x}\right)^2$$

![[ML/img/ML_04_4.png]]

Per **minimizzare** l'errore $E(\mathbf{w})$ possiamo il classico **approccio analitico**, trovando il valore di $\mathbf{w}$ che rende la **deriviata** di $E(\mathbf{w})$ **nulla**, tramite tecniche di [[Gradient Descent]].

Essendo la [[Some Loss Functions#Quadratic Loss|quadratic loss]] una funzione [[Convessità|strettamente convessa]], abbiamo un **unico** punto di minimo globale, perciò è sufficiente annullare la derivata.

Ricordiamo che la [[Gradient Descent#^4c1ed4|derivata della quadratic loss]] è $$\dfrac{\partial}{\partial w_k}E(\mathbf{w}) = 2 \sum_{i=1}^n(t_i - \mathbf{w}^T\Phi(\mathbf{x}_i)) \cdot \phi_k(\mathbf{x}_i) = 2 \sum_{i=1}^n\left(t_i - \sum_{j=0}^{m}w_j\phi_j(\mathbf{x}_i)\right) \cdot \phi_k(\mathbf{x}_i)$$
```ad-important
title: Closed Form Solution
È possibile trovare il $\mathbf{w}^*$ che annulla il gradiente $\nabla_{\mathbf{w}}E(\mathbf{w}) = \mathbf{0}$ mediante una **singola forma chiusa** $$\mathbf{w}^* = (\mathbf{\Phi}^T\mathbf{\Phi})^{-1}\mathbf{\Phi}^T\mathbf{t}$$
con $$\mathbf{\Phi} = \mathbf{\Phi}(\mathbf{X})$$
```

^6eecde


## [[Model Selection]]
### [[Esempio utilizzo Regolarizzazione]]