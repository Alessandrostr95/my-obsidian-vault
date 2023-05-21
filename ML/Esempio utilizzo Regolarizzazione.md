Consideriamo la [[Linear Regression#^db2e08|solita relazione]] $t = \sin{2\pi x}$, e supponiamo di avere a disposizione $L = 100$ *training set* $\mathcal{T}_1, ..., \mathcal{T}_L$, ognuno dei quali grande $n$.

Fissiamo il parametro $m = 24$, e utiliziamo $m$ [[Some Base Function#Gaussian|base function gaussiane]] $\phi_1, ..., \phi_m$.
Ora per ogni *training set* $\mathcal{T}_i$ definiamo un **predittore** $y_i(\mathbf{x})$ derivato **minimizzando** l'[[Model Selection#Limitare la complessità del Modello - Ridge Regression|errore regolarizzato]] $$E(\mathbf{w}) = \frac{1}{2}(\mathbf{\Phi w - t})^T(\mathbf{\Phi w - t}) + \frac{\lambda}{2}\mathbf{w}^T\mathbf{w}$$

Osserviamo come si comportano mediamente i predittori a variare del [[Model Selection#^49f3ad|coefficiente di regolarizzazione]] $\lambda$.

![[ML/img/ML_04_10.png]]
