Purtroppo risolvere una [[Support Vector Machines]] richiede tempo cubico $O(n^3)$ nel numero degli elementi del dataset, il quale è un tempo non ragionevole.

Per risolvere questo problema si possono usare tecniche di **approssimazione** della soluzione, oppure tecniche di [[Gradient Descent]].
Per applicare il GD dobbiamo definire una [[Prediction Risk|funzione loss]] e il suo gradiente.

Osserviamo che dati dei parametri $\mathbf{w}, w_0$ qualsiasi, le [[SVM - removing separability assumption#^4c4a92|variabili di slack]] $\xi_i$ sono minimizzate quando
$$\min \mathbf{\xi}_i = \begin{cases}
0 &\text{if } t_i(\mathbf{w}\cdot\mathbf{x}_i + w_0) \geq 1\\
1 - t_i(\mathbf{w}\cdot\mathbf{x}_i + w_0)  &\text{otherwise}
\end{cases}$$
ovvero
$$\min \mathbf{\xi}_i = \max(0, 1 - t_i(\mathbf{w}\cdot\mathbf{x}_i + w_0))$$
Questa quantità equivale alla [[Some Loss Functions#Hinge Loss|Hinge Loss]]. 