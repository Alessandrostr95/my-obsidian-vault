Introdurre un modello di variabile latente per mettere in relazione un vettore di osservazione $d$-dimensionale a una corrispondente variabile latente gaussiana $d'$-dimensionale, della forma
$$\mathbf{x} = \mathbf{W}\mathbf{z}^T + \pmb\mu + \pmb\epsilon$$
dove:
- $\mathbf{z}$ è una **variabile latente** $d'$-dimensionale **gaussiana**. Può essere vista come una **proiezione** di $\mathbf{x}$ in un sottospazio $d'$-dimensionale.
- $\mathbf{W}$ è una matrice $d \times d'$ che mette in relazione lo spazio di dimensione $d$ con quello di dimensione $d'$.
- $\pmb\epsilon$ è un **rumore gaussiano** $d$-dimensionale, ovvero con distribuzione $\mathcal{N}(\mathbf{0}, \sigma^2I)$.
- $\pmb\mu$ è il vettore **media**.

Definiamo il seguente **processo generativo**
1. Campioniamo una **variabile latente** da $\mathbb{R}^{d'}$ con distribuzione $$\mathbf{z} \sim \mathcal{N}(\mathbf{0}, I)$$
2. Facciamo la **proiezione lineare** di $\mathbb{R}^{d}$ in $$\mathbf{y} = \mathbf{W}\mathbf{z}^T + \pmb\mu$$
3. Campioniamo il **rumore** $\pmb\epsilon$ da  $\mathcal{N}(\mathbf{0}, \sigma^2I)$
4. Aggiungo il rumore $$\mathbf{x} = \mathbf{y}+\pmb\epsilon$$

Avendo campionato la variabile latente $\mathbf{z}$, allora il nuovo punto $\mathbf{x}$ avrà distribuzione gaussiana $$\mathbf{x} \vert \mathbf{z} \sim \mathcal{N}(\mathbf{Wz}^T + \pmb\mu, \sigma^2I)$$

![[ML/img/ML_15_5.png]]

Osserviamo che la [[Distribuzioni Multivariate#Joint PDF|distribuzione congiunta]] di $\mathbf{x}, \mathbf{z}$ sarà
$$\begin{bmatrix}
\mathbf{z}\\
\mathbf{x}
\end{bmatrix} \sim \mathcal{N}\left(\mu_{\mathbf{zx}}, \Sigma\right)$$
con
$$\mu_{\mathbf{zx}} =
\begin{bmatrix}
\mu_{\mathbf{z}}\\
\mu_{\mathbf{x}}
\end{bmatrix}$$
Osserviamo che dato che $\mathbf{z} \sim \mathcal{N}(\mathbf{0}, I)$ allora $\mu_{\mathbf{z}} = \mathbf{0}$.
Perciò
$$\mu_{\mathbf{x}} = \mathbb{E}\left[ \mathbf{x} \right] = \mathbb{E}\left[ \mathbf{Wz^T} + \pmb\mu + \pmb\epsilon \right] = \pmb\mu$$
e quindi
$$\mu_{\mathbf{zx}} =
\begin{bmatrix}
\mathbf{0}\\
\pmb\mu
\end{bmatrix}$$

Per quanto riguarda la matrice di covarianza abbiamo
$$\Sigma =
\begin{bmatrix}
\Sigma_{\mathbf{zz}} & \Sigma_{\mathbf{zx}}\\
\Sigma_{\mathbf{xz}} & \Sigma_{\mathbf{xx}}
\end{bmatrix}$$
dove
$$\begin{align}
\Sigma_{\mathbf{zz}} &= I\\
\Sigma_{\mathbf{zx}} &= \mathbf{W}^T = \Sigma_{\mathbf{xz}}^T\\
\Sigma_{\mathbf{xx}} &= \mathbf{W}\mathbf{W}^T + \sigma^2I
\end{align}$$