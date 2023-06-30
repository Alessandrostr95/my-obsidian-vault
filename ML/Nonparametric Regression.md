Nella [[Fully Bayesian Approach|Fully Bayesian Regression]] si vuole stimare la probabilità $p(y \vert x)$ di un qualsiasi valore target $y$ data un'osservazione $x$.
Per fare ciò, viene calcolata la [[Fully Bayesian Approach#^73334c|posterior predictive distribution]], la quale sappiamo essere una **gaussiana** con
- media $$\mu(x) = \beta \phi(x)^TS \mathbf{\Phi}^T\mathbf{t}$$
- varianza $$\sigma^2(x) = \beta^{-1} + \phi(x)^T S \phi(x)$$
dove $$S = (\beta\Phi^T\Phi + \alpha I)^{-1}$$


Un'idea potrebbe essere quella di identificare il valore target $y(x)$ dell'osservazione $x$ come la **media** della sua predizione, ovvero $$y(x) = \mu(x) = \beta \phi(x)^TS \mathbf{\Phi}^T\mathbf{t}$$

Osservare che la predizione $y(x)$ non viene fatta in base ad un insieme di parametri derivati dall'ottimizzazione di una certa [[Gradient Descent|loss function]].
Invece la predizione $y(x)$ può essere vista come una **combinazione lineare** dei valori target $t_i$, **pesati** rispetto al rispettivo elemento $x_i$ e l'osservazione $x$.
Infatti possiamo riscrivere la media $\mu(x)$ come la combinazione lineare $$\beta \phi(x)^TS \mathbf{\Phi}^T\mathbf{t} = \sum_{i=1}^{n}\beta \phi(x)^TS \phi(x_i)^Tt_i$$
Indichiamo con $$\kappa(x, x_i) = \beta \phi(x)^TS \phi(x_i)^T$$ la **funzione peso** rispetto all'osservazione $x$ e l'elemento $x_i$.
Tale funzione è anche nota come **funzione kernel equivalente**.

Riscriviamo quindi la predizione come $$y(x) = \sum_{i=1}^{n}\kappa(x,x_i)t_i$$
Nella figura vediamo destra i valori della *kernel equivalente* per ogni coppia di valori.
A sinistra invece i valori che possono assumere le funzioni $\kappa(\cdot , x_i), \kappa(\cdot , x_j), \kappa(\cdot , x_k)$.
![[ML/img/ML_05_1.png]]

Possiamo osservare che la funzione kernel $\kappa(x,x_i)$ tende a dare valori più alti per valori di $x$ che si avvicinano di più ad $x_i$.

Possiamo anche notare come cambia la funzione kernel equivalente al cambiare delle [[Some Base Function|base function]] che definiscono $\Phi$.
![[ML/img/ML_05_2.png]]


