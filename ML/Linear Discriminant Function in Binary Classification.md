Abbiamo come dominio lo spazio $\mathbb{R}^D$, e vogliamo trovare l'**iperpiano** di dimensione $D-1$ che **separa** tutti gli elementi della classe $C_0$ da quelli della classe $C_1$.

Dato un vettore di parametri $(w_0, w_1, ..., w_D)$, supponiamo che l'**iperpiano di separazione** tra le regioni che identificano le classi è definito dall'equazione $$y(\mathbf{w}) = \mathbf{w}^T\mathbf{x} + w_0 = 0$$ per ogni $\mathbf{x}$ appartenente all'iperpiano di separazione.

Se questo è vero per ogni punto $\mathbf{x}_1, \mathbf{x}_2$ dell'iperpiano, allora deve essere vero che $$y(\mathbf{x}_1) - y(\mathbf{x}_2)= (\mathbf{w}^T\mathbf{x}_1+w_0) - (\mathbf{w}^T\mathbf{x}_2+w_0) = \mathbf{w}^T(\mathbf{x}_1-\mathbf{x}_2) = 0$$ ovvero $(\mathbf{x}_1-\mathbf{x}_2)$ è un vettore **ortogonale** a $\mathbf{w}$, e quindi l'iperpiano sarà **perpendicolare** al vettore $\mathbf{w}$.

Perciò una **condizione** per **discriminare** la classe $C_0$ dalla classe $C_1$ è la seguente $$\begin{cases}
\mathbf{x} \in C_0 \iff y(\mathbf{x}) > 0\\
\mathbf{x} \not\in C_0 \iff y(\mathbf{x}) < 0\\
\end{cases}$$

Nel caso di $K > 2$ classi possiamo definire $K$ **funzioni discriminanti** $$y_i(\mathbf{x}) = w_{i0} + \mathbf{w}_{i}^T\mathbf{x}$$Perciò un elemento $\mathbf{x}$ appartiene alla classe $C_k$ se e solo se $y_k(\mathbf{x}) > y_j(\mathbf{x})$ per ogni altro $j \neq k$.
Perciò la [[Linear Classification#^3cfa43|discriminant function]] è della forma $$k = \arg\max_{1 \leq j \leq K}y_j(\mathbf{x})$$

Per quanto riguarda i **margini** tra ogni coppia di regioni $C_i, C_j$ sono tutti quei punti $\mathbf{x}$ tali che $y_i(\mathbf{x}) = y_j(\mathbf{x})$, o in altri termini l'iperpiano che comprende tutti quei punti $\mathbf{x}$ tali che $$(\mathbf{w}_i - \mathbf{w}_j)^T\mathbf{x}+(w_{i0} - w_{j0}) = 0$$
![[ML_06_1.png]]