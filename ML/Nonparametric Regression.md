# Equivalent Kernel
Nella [[Fully Bayesian Approach|Fully Bayesian Regression]] si vuole stimare la probabilità $p(y \vert x)$ di un qualsiasi valore target $y$ data un'osservazione $x$.
Per fare ciò, viene calcolata la [[Fully Bayesian Approach#^73334c|posterior predictive distribution]], la quale sappiamo essere una **gaussiana** con
- media $$\mu(x) = \beta \phi(x)^TS \mathbf{\Phi}^T\mathbf{t}$$
- varianza $$\sigma^2(x) = \beta^{-1} + \phi(x)^T S \phi(x)$$
dove $$S = (\beta\Phi^T\Phi + \alpha I)^{-1}$$


Un'idea potrebbe essere quella di identificare il valore target $y(x)$ dell'osservazione $x$ come la **media** della sua predizione, ovvero $$y(x) = \mu(x) = \beta \phi(x)^TS \mathbf{\Phi}^T\mathbf{t}$$

Osservare che la predizione $y(x)$ non viene fatta in base ad un insieme di parametri derivati dall'ottimizzazione di una certa [[Gradient Descent|loss function]].
Invece la predizione $y(x)$ può essere vista come una **combinazione lineare** dei valori target $t_i$, **pesati** rispetto al rispettivo elemento $x_i$ e l'osservazione $x$.
Infatti possiamo riscrivere la media $\mu(x)$ come la combinazione lineare $$\beta \phi(x)^TS \mathbf{\Phi}^T\mathbf{t} = \sum_{i=1}^{n}\beta \phi(x)^TS \phi(x_i)t_i$$
Indichiamo con $$\kappa(x, x_i) = \beta \phi(x)^TS \phi(x_i)$$ la **funzione peso** rispetto all'osservazione $x$ e l'elemento $x_i$.
Tale funzione è anche nota come **funzione kernel equivalente**. ^0e14e6

Riscriviamo quindi la predizione come $$y(x) = \sum_{i=1}^{n}\kappa(x,x_i)t_i$$
Nella figura vediamo destra i valori della *kernel equivalente* per ogni coppia di valori.
A sinistra invece i valori che possono assumere le funzioni $\kappa(\cdot , x_i), \kappa(\cdot , x_j), \kappa(\cdot , x_k)$.
![[ML/img/ML_05_1.png]]

Possiamo osservare che la funzione kernel $\kappa(x,x_i)$ tende a dare valori più alti per valori di $x$ che si avvicinano di più ad $x_i$.

Possiamo anche notare come cambia la funzione kernel equivalente al cambiare delle [[Some Base Function|base function]] che definiscono $\Phi$.
![[ML/img/ML_05_2.png]]
A sinistra vediamo $\kappa(0,x)$ nel caso di [[Some Base Function#Polynomial|base function polinomiali]], e a destra nel caso di [[Some Base Function#Gaussian|base function gaussiane]].


## Kernel as Covariance
La [[#^0e14e6|funzione kernel equivalente]] può anche essere vista come la **covarianza** tra le combinazioni lineari $\phi(x)^T\mathbf{w}$ e $\phi(x')^T\mathbf{w}$, infatti $$\text{cov}(\phi(x)^T\mathbf{w}, \phi(x')^T\mathbf{w}) = \phi(x)^TS\phi(x) = \frac{1}{\beta}\kappa(x,x')$$

==Più due punti $x,x'$ sono **simili/vicini** maggiore è la loro correlazione==.
Perciò, invece di utilizzare una serie di **base function** per definire una funzione kernel, possiamo definire una funzione kernel in **maniera diretta** e utilizzarla per fare predizioni.
L'importante è che rispetti questo rapporto tra **località** e **correlazione** tra coppie di punti. ^23c639

----
# [[Kernel Regression - Nadaraya-Watson Model]]
# [[Locally Weighted Regression]]
# [[Local Logistic Regression]]