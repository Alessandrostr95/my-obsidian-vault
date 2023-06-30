Nella [[Fully Bayesian Approach|Fully Bayesian Regression]] si vuole stimare la probabilità $p(y \vert x)$ di un qualsiasi valore target $y$ data un'osservazione $x$.
Per fare ciò, viene calcolata la [[Fully Bayesian Approach#^73334c|posterior predictive distribution]], la quale sappiamo essere una **gaussiana** con
- media $$\mu(x) = \beta \phi(x)^TS \mathbf{\Phi}^T\mathbf{t}$$
- varianza $$\sigma^2(x) = \beta^{-1} + \phi(x)^T S \phi(x)$$
dove $$S = (\beta\Phi^T\Phi + \alpha I)^{-1}$$


Un'idea potrebbe essere quella di identificare il valore target $y(x)$ dell'osservazione $x$ come la **media** della sua predizione, ovvero $$y(x) = \mu(x) = \beta \phi(x)^TS \mathbf{\Phi}^T\mathbf{t}$$



