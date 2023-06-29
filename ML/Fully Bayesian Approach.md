Abbiamo visto che nell'[[Probabilistic Model for Regression#Approccio Bayesiano|approccio baiesiano]] alla regressione l'obiettivo è quello di stimare la [[Maximum a Posteriori - Bayesian Approach#^7ee03b|probabilità a posteriori]] $\mathbf{w}$, assumendo che **a priori** $\mathbf{w}$ sia distribuito secondo una gaussiana e che anche i target $\mathbf{t}$ siano distribuiti secondo una gaussiana con media $\mathbf{w}^T\mathbf{x}$.

L'**approccio completamente baiesiano** (o **fully bayesian**) consiste, contrariamente, nel cercare di stimare la probabilità di un dato target $y$ sapendo di aver osservato $x$, e date le osservazioni del training set $\mathcal{T}$.

Questo si può fare calcolando la **media** rispetto a tutti i valori di $\mathbf{w}$.
$$p(y \vert x, (\mathbf{X}, \mathbf{t}), \alpha, \beta) = \mathbb{E}_{p(\mathbf{w} \vert \mathbf{X}, \mathbf{t},\alpha, \beta)} \left[ p(y \vert x, \mathbf{w}, \beta) \right] = \int p(y \vert x, \mathbf{w}, \beta) \cdot p(\mathbf{w} \vert \mathbf{X}, \mathbf{t},\alpha, \beta) \,d\mathbf{w}$$
Ovvero la media di:
- la probabilità $p(y \vert x, \mathbf{w}, \beta)$ del target $y$ assumendo che sia distribuito come una gaussiana con media $\mathbf{w}^Tx$ e varianza $\beta^-1$
- dove i parametri $\mathbf{w}$ sono campionati rispetto alla distribuzione $p(\mathbf{w} \vert \mathbf{X}, \mathbf{t},\alpha, \beta)$