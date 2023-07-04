Nella [[Kernel Regression - Nadaraya-Watson Model]] la predizione viene fatta come una **media pesata e normalizzata** dei valori target, pesandoli in base a quanto i corrispettivi campioni $x_i \in X$ sono simili al valore in input $x$.

La **Locally Weighted Regression** migliora la Kernel Rergession, considerando come **[[Prediction Risk#^0b44e2|funaione loss]]** una media pesata degli **[[Some Loss Functions#Quadratic Loss|scarti quadratici]]** anziché dei valori target.

Più precisamente, vogliamo predire il valore target di un elemento $\mathbf{x}$ come la **combinazione lineare** $\mathbf{w}^T\mathbf{x}$, perciò avremo la seguente loss  $$L(x) = \sum_{i=1}^{n}\kappa_i(\mathbf{x})(\mathbf{w}^T\mathbf{x} - t_i)^2 = \sum_{i=1}^{n}\kappa_h(\mathbf{x} - \mathbf{x}_i)(\mathbf{w}^T\mathbf{x} - t_i)^2$$ dove $$\kappa_i(\mathbf{x}) = \kappa_h(\mathbf{x}-\mathbf{x}_i)$$

Si vuole quindi trovare il vettore di parametri che **minimizzi** questa loss function $$\mathbf{w}(\mathbf{x}) = \arg\min_{\mathbf{w}} \sum_{i=1}^{n}\kappa_i(\mathbf{x})(\mathbf{w}^T\mathbf{x} - t_i)^2$$ e questa ottimizzazione si può ottenere in **forma chiusa** $$\mathbf{w} = (\mathbf{X}^T \Psi(\mathbf{x})\mathbf{X})^{-1}\mathbf{X}^T \Psi(\mathbf{x})\mathbf{t}$$ dove $\Psi(\mathbf{x})$ è la **matrice diagonale** che contiene tutti gli elementi $\kappa_i(\mathbf{x})$
$$\Psi(\mathbf{x}) = \begin{pmatrix}
\kappa_1(\mathbf{x})  &0 &... &0\\
0 &\kappa_2(\mathbf{x}) &... &0\\
\vdots &\ddots &... &\vdots\\
0 &0 &... &\kappa_n(\mathbf{x})
\end{pmatrix} = \begin{pmatrix}
\kappa_h(\mathbf{x}-\mathbf{x}_1)  &0 &... &0\\
0 &\kappa_h(\mathbf{x}-\mathbf{x}_2) &... &0\\
\vdots &\ddots &... &\vdots\\
0 &0 &... &\kappa_h(\mathbf{x}-\mathbf{x}_n)
\end{pmatrix}$$

La predizione di un qualsiasi input $\mathbf{x}$ sarà effettuata come il prodotto $$y(\mathbf{x}) = \mathbf{w}(\mathbf{x})^T \mathbf{x}$$

```ad-attention
Questo metodo non è del tutto **non parametrico**, in quanto bisogna calcolare il miglior valore dei parametri $\mathbf{w}(\mathbf{x})$. 
D'altra parte non è nemmeno parametrico, perché non si calcola un insieme di parametri che valga per qualsiasi **input**, bensì dipende ogni volta dall'inptu che si riceve.
```
