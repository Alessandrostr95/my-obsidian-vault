La **Principal Component Analysis** (**PCA**) è un metodo di [[Dimensionality Reduction#^2abcdd|feature extraction]].
Supponiamo che il nostro dominio sia $d$-dimensionale.
Nella PCA si cerca un sottospazio di dimensione $d' < d$ che rappresenti le caratteristiche dei dati nel modo più **fedele** possibile.
Per rappresentazione "*fedele*" si intende che le **distanze** tra gli elementi e le loro proiezioni sono piccole, possibilmente minime.
![[ML/img/ML_15_1.png]]


# PCA - $d' = 0$
Supponiamo di voler rappresentare il nostro dataset $d$-dimensionale $\mathbf{x}_0, ..., \mathbf{x}_n$ mediante **un unico punto** punto $\mathbf{x}_0$
Vogliamo che le distanze $$J(\mathbf{x}_0) = \sum_{i=1}^{n} \Vert \mathbf{x}_0 - \mathbf{x}_1 \Vert^2$$ sia **minimizzato**.
È facile mostrare che $\mathbf{x}_0$ è il **centro di massa** $$\mathbf{x}_0 = \mathbf{m} = \frac{1}{2} \sum_{i=1}^{n}\mathbf{x}_i$$
# PCA - $d' = 1$
Un solo punto potrebbe essere una rappresentazione troppo approssimata di un intero dataset.
Una rappresentazione migliore potrebbe essere una **proiezione su un piano**.
![[ML/img/ML_15_2.png]]

Sia $\mathbf{u}_1$ un qualsiasi **vettore unitario** che segue la **direzione** del piano di proiezione che ci interessa.
L'equazione del piano di proiezione è quindi $$\mathbf{x} = \alpha \mathbf{u}_1 + \mathbf{m}$$ dove $\alpha$ è la **distanza** tra la proiezione di $\mathbf{x}$ e quella di $\mathbf{m}$ sulla linea.

Indichiamo con $$\tilde{\mathbf{x}}_i = \alpha_i \mathbf{u}_1 + \mathbf{m}$$ la proiezione di $\mathbf{x}_i$ del dataset.

Vorremmo quindi trovare la retta che minimizzi
$$\begin{align}
J(\alpha_1, ..., \alpha_n, \mathbf{u}_1)
&= \sum_{i=1}^{n} \Vert \tilde{\mathbf{x}}_i - \mathbf{x}_i \Vert^2\\
&= \sum_{i=1}^{n} \Vert (\alpha_i \mathbf{u}_1 + \mathbf{m}) - \mathbf{x}_i \Vert^2\\
&= \sum_{i=1}^{n} \Vert \alpha_i \mathbf{u}_1 - (\mathbf{x}_i - \mathbf{m})\Vert^2\\
&= \sum_{i=1}^{n} \alpha_i^2 \Vert\mathbf{u}_1\Vert^2 + \sum_{i=1}^{n} \Vert\mathbf{x}_i - \mathbf{m}\Vert^2 - 2\sum_{i=1}^{n}\alpha_i\mathbf{u}_1^T(\mathbf{x_i} - \mathbf{m})\\
&= \sum_{i=1}^{n} \alpha_i^2 + \sum_{i=1}^{n} \Vert\mathbf{x}_i - \mathbf{m}\Vert^2 - 2\sum_{i=1}^{n}\alpha_i\mathbf{u}_1^T(\mathbf{x_i} - \mathbf{m})
\end{align}$$

Derivando rispetto ad $\alpha_k$ avremo
$$\frac{\partial}{\partial \alpha_k}J(\alpha_1, ..., \alpha_n, \mathbf{u}_1) = 2\alpha_k - 2 \mathbf{u}_1^T(\mathbf{x}_k - \mathbf{m})$$ il quale è **minimizzato** per $$\alpha_k = \mathbf{u}_1 \cdot (\mathbf{x}_k - \mathbf{m})$$ ^647e61
```ad-note
È possibile dimostrare che quello è un punto di **minimo** e non massimo perché la derivata seconda è maggiore di 0
$$\frac{\partial^2}{\partial \alpha_k^2}J(\alpha_1, ..., \alpha_n, \mathbf{u}_1) = 2 > 0$$
```

Per trovare il miglior $\mathbf{u}_1$ dobbiamo iniziare considerando la matrice di covarianza del dataset
$$S = \frac{1}{n} \sum_{i=1}^{n}(\mathbf{x}_i - \mathbf{m})(\mathbf{x}_i - \mathbf{m})^T$$

Applicando il [[#^647e61|miglior]] $\alpha_k$ alla definizione di $J$ avremo
$$\begin{align}
J(\mathbf{u}_1)
&= \sum_{i=1}^{n} \alpha_i^2 + \sum_{i=1}^{n} \Vert\mathbf{x}_i - \mathbf{m}\Vert^2 - 2\sum_{i=1}^{n}\alpha_i\mathbf{u}_1^T(\mathbf{x_i} - \mathbf{m})\\
&= \sum_{i=1}^{n} (\mathbf{u}_1^T (\mathbf{x}_i - \mathbf{m}))^2 + \sum_{i=1}^{n} \Vert\mathbf{x}_i - \mathbf{m}\Vert^2 - 2\sum_{i=1}^{n}(\mathbf{u}_1^T (\mathbf{x}_i - \mathbf{m}))\mathbf{u}_1^T(\mathbf{x_i} - \mathbf{m})\\
&= - \sum_{i=1}^{n}\mathbf{u}_1^T (\mathbf{x}_i - \mathbf{m})(\mathbf{x}_i - \mathbf{m})^T\mathbf{u}_1  + \sum_{i=1}^{n} \Vert\mathbf{x}_i - \mathbf{m}\Vert^2\\
&= -n\mathbf{u}_1^T S\mathbf{u}_1  + \sum_{i=1}^{n} \Vert\mathbf{x}_i - \mathbf{m}\Vert^2
\end{align}$$

Osservare quindi che **minimizzare** $J(\mathbf{u}_1)$ equivale al **massimizzare** $\mathbf{u}_1^T S\mathbf{u}_1$.
Perciò questo si trasforma nel seguente problema di ottimizzazione
$$\begin{align}
u = \max_{\mathbf{u}_1} &\;\; \mathbf{u}_1^TS\mathbf{u}_1\\
s.t. &\;\; \Vert \mathbf{u}_1 \Vert^2 = 1
\end{align}$$
Applicando l'opportuno [[Support Vector Machines#Lagrangiano|moltiplicatore lagrangiano]] avremo
$$\max_{\mathbf{u}_1} \mathbf{u}_1^TS\mathbf{u}_1 - \lambda_1(\mathbf{u}_1\cdot\mathbf{u}_1 - 1)$$

Possiamo ottenere il punto di massimo annullando la derivata prima
$$\frac{\partial u}{\partial\mathbf{u}_1} = 2S\mathbf{u}_1 - 2 \lambda_1\mathbf{u}_1 = 0$$
$$\implies S\mathbf{u}_1 = \lambda_1\mathbf{u}_1$$

In altri termini $\mathbf{u}_1$ massimizza $J$ quando è un **autovettore** della matrice di covarianza $S$.
Inoltre, la varianza totale della proiezione equivale all'**autovalore** $\lambda_1$, infatti
$$\mathbf{u}_1^TS\mathbf{u}_1 = \mathbf{u}_1^T\lambda_1\mathbf{u}_1 = \lambda_1\mathbf{u}_1^T\mathbf{u}_1 = \lambda_1$$
Osservare inoltre che la varianza della proiezione è massimizzata, se si sceglie un autovettore $\mathbf{u}_1$ con **autovalore massimo**.

# PCA - $d' > 1$
In questo caso vogliamo un iperpiano definito da $d'$ vettori unitari $\mathbf{u}_1, ...,\mathbf{u}_{d'}$.
L'errore è **minimizzato** quando $\mathbf{u}_1, ...,\mathbf{u}_{d'}$ sono gli autovettore di $S$ con **autovalori massimi**.

Analogamente, se assumiamo che il dati siano distribuiti secondo una [[CLT - Central Limit Theorem#Densità normale multivariata|gaussiana multivariata]] $d$-dimensionale con media $\mu$ e matrice di covarianza $\Sigma$, possiamo tranquillamente considerare l'iperpiano $d'$-dimensionale definito dagli autovalori $\mathbf{u}_1, ...,\mathbf{u}_{d'}$ di $\Sigma$ con **autovalori massimi**.

![[ML/img/ML_15_3.png]]

# Choosing $d'$
La distribuzione della dimensione dell'autovalore è solitamente caratterizzata da una rapida diminuzione iniziale seguita poi da una **lenta diminuzione**.
Ciò significa che esiste un certo valore di $d'$ per il quale, anche aumentando la dimensione del sottospazio, non si ottengono informazioni aggiuntive.

![[ML/img/ML_15_4.png]]

Possiamo quindi identificare quante dimensioni ci servono, in base a quanta "*fedeltà*" dei dati voglio preservare.

Abbiamo detto che gli autovalori misurano la **quantità di varianza** mantenuta nella proiezione.
Per ogni $k < d$, definiamo il valore $$r_k := \frac{\sum_{i=1}^{k}\lambda_i^2}{\sum_{i=1}^{n}\lambda_i^2}$$ il quale ci fornisce una **misura** di quanta varianza preserviamo se utilizziamo i primi $k$ autovalori più grandi.

Quando abbiamo noti $r_1 < ... < r_d$, possiamo identificare $$d' = \arg \min_{k = 1,...,d} \lbrace r_k : r_k > p \rbrace$$ dove $p$ è la quantità minima di varianza che desideriamo preservare.

------
# [[Probabilistic PCA]]