Consideriamo la [[Linear Regression#^db2e08|solita relazione]] $t = \sin{2\pi x}$, e supponiamo di avere a disposizione $L = 100$ *training set* $\mathcal{T}_1, ..., \mathcal{T}_L$, ognuno dei quali grande $n$.

Fissiamo il parametro $m = 24$, e utiliziamo $m$ [[Some Base Function#Gaussian|base function gaussiane]] $\phi_1, ..., \phi_m$.
Ora per ogni *training set* $\mathcal{T}_i$ definiamo un **predittore** $y_i(\mathbf{x})$ derivato **minimizzando** l'[[Model Selection#Limitare la complessità del Modello - Ridge Regression|errore regolarizzato]] $$E(\mathbf{w}) = \frac{1}{2}(\mathbf{\Phi w - t})^T(\mathbf{\Phi w - t}) + \frac{\lambda}{2}\mathbf{w}^T\mathbf{w}$$

Osserviamo come si comportano mediamente i predittori a variare del [[Model Selection#^49f3ad|coefficiente di regolarizzazione]] $\lambda$.

![[ML/img/ML_04_10.png]] ^f4c576

Abbiamo a sinistra della [[#^f4c576|figura]] tutti le $L$ regressioni $y_1, ..., y_L$, mentre a destra abbiamo in verde la funzione desiderata e in rosso la **media** degli $L$ predittori.

Per esempio, per $\lambda = 13.46$, abbiamo tutte le funzioni non differiscono molto tra di loro [[From Learning to Optimization#Bias vs Varianza|bassa varianza]].
Però purtroppo la loro media è una brutta approssimazione della funzione ignota, ovvero abbiamo [[From Learning to Optimization#Bias vs Varianza|alto bias]].

![[ML/img/ML_04_11.png]]

Viceversa, con un valore basso $\lambda = 0.09$, abbiamo che i predittori $y_1,...,y_L$ sono tutti **molto differenti** tra di loro (ovvero [[From Learning to Optimization#Bias vs Varianza|alta varianza]]), però **in media** approssimano bene l'obiettivo (ovvero [[From Learning to Optimization#Bias vs Varianza|basso bias]]).

![[ML/img/ML_04_12.png]]
Più in generale possiamo [[#^ed3a2f|empiricamente osservare]] che:
1. al crescere di $\lambda$ aumenta il bias e diminusce la varianza.
2. al decrescere di $\lambda$ dimiuisce il bias e aumenta la varianza.
3. il punto di minimo della somma bias+varianza corrisponde allo stesso valore di $\lambda$ per cui l'**errore è minimo**.