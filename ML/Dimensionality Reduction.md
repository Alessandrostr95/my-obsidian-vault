Nel machine learning spesso occorre il problema del dover gestire un elevata quantità di feature.
Infatti, quando si hanno tante feature aumentano anche il numero di parametri da apprendere

Per esempio, consideriamo il seguente predittore
$$y(\mathbf{x}, \mathbf{w}) = w_0 + \sum_{i=1}^{D}w_ix_i + \sum_{i=1}^{D}\sum_{j=1}^{D}w_{ij}x_ix_j + \sum_{i=1}^{D}\sum_{j=1}^{D}\sum_{k=1}^{D}w_{ijk}x_ix_jx_k$$
In questo caso, al crescere del numero di feature $D$ il numero di parametri cresce come $O(D^3)$

In certi casi, la dimensione del training set *necessaria* per ottenere una certa precisione **cresce esponenzialmente** rispetto al numero di feature.
In poche parole maggiore è il numero di feature, maggiore deve essere grande il dataset affinché si possano ottenere buoni risultati.

Spesso accade che non tutte le feature siano **relamente informative** per gli scopi della predizione.
Perciò sarebbe desiderabile ridurre il numero di feature, conservando le sole rilevanti.

Esistono due approcci principali:
- **feature selection**: identificare quelle feature che sono rappresentative della varianza del dataset.
- **feature extraction**: identificare una **proiezione** del dataset in uno spazio con dimensione minore, in modo da preservare la varianza dei dati. ^2abcdd

# [[PCA - Principal Component Analysis]]
## [[Probabilistic PCA]]
