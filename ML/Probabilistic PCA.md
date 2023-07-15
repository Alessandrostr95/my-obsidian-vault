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

