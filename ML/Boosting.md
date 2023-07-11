Il boosting consiste nello sfruttare gli errori fatti dai predittori addestrati fino a un certo punto, per creare un nuovo predittore migliore che vada meglio dove gli altri vanno male.

Per fare ciò, si utilizza una versione **pesata** del dataset.
Vengono quindi pesati maggiormente gli elementi per i quali i predittori precedenti sbagliano molto.
In fine per fare une predizione si fa una **media pesata** delle singole predizioni, pesato in base a come si sono comportati i singoli.

Quindi siano $y_1, ..., y_M$ i predittori, con qualità $\alpha_1 ,..., \alpha_M$.
Per esempio, per fare classificazione, possiamo considerare la media pesata $$Y_M(x) = \text{sign}\left( \sum_{m=1}^{M}\alpha_m \cdot y_m(x)\right)$$

![[ML_13_1.png]]


# [[Adaboost]]

# [[Gradient Boosting]]

