Abbiamo come dominio lo spazio $\mathbb{R}^D$, e vogliamo trovare l'**iperpiano** di dimensione $D-1$ che **separa** tutti gli elementi della classe $C_0$ da quelli della classe $C_1$.

Dato un vettore di parametri $(w_0, w_1, ..., w_D)$, un **iperpiano di separazione** tra le regioni che identificano le classi sarà della forma $$y(\mathbf{w}) = \mathbf{w}^T\mathbf{x} + w_0 = 0$$ per ogni $\mathbf{x}$ appartenente all'iperpiano di separazione.

Se questo è vero per ogni punto $\mathbf{x}_1, \mathbf{x}_2$ dell'iperpiano, allora deve essere vero che $$y(\mathbf{x}_1) - y(\mathbf{x}_2)= (\mathbf{w}^T\mathbf{x}_1+w_0) - (\mathbf{w}^T\mathbf{x}_2+w_0) = \mathbf{w}^T(\mathbf{x}_1-\mathbf{x}_2) = 0$$ ovvero $(\mathbf{x}_1-\mathbf{x}_2)$ è un vettore **ortogonale** a $\mathbf{w}$, e quindi l'iperpiano sarà **perpendicolare** al vettore $\mathbf{w}$.

