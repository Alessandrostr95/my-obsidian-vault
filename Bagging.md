Il **bagging** è un [[Ensemble methods|metod di ensamble]] che consiste dell'addestrare un insieme di modelli, per poi fare la predizione tramite una **media pesata** delle singole predizioni, pesandole in base alla qualità dei singoli.

Consideriamo per esempio i [[Decision Trees]].
In base a un dato dataset $\mathcal{T}$ avremo un albero, il quale rinforza la sua precisione man mano che si scende verso le foglie.
Questo decision tree dipende però troppo da $\mathcal{T}$, infatti a fronte di piccole perturbazioni sui dati abbiamo decision tree molto differenti.

## Bootstrap
Il **boostrap** è uno strumento statistico.
Quello che si vuole fare è cercare di stimare una distribuzione $\mathcal{F}$ tramite una **distribuzione empirica** $\hat{\mathcal{F}}$.
Consideriamo il dataset $(\mathbf{X}, \mathbf{t})$ grande $n$, e definiamo la distribuzione empirica $\hat{\mathcal{F}}$ come segue
$$\hat{p}(x, t) := \begin{cases}
\dfrac{1}{n} &\text{if } \exists (x_i, t_i) \in (\mathbf{X}, \mathbf{t}): (x_i, t_i) = (x, t)\\
\\
0 &\text{otherwise}
\end{cases}$$

Un **bootstrap sample** di $m$ elementi da un dataset $(\mathbf{X}, \mathbf{t})$ è composto da $m$ campionamenti $(x^*, t^*)$ da $(\mathbf{X}, \mathbf{t})$ ==con **rimpiazzo**==.
Questo bootstrap sampling equivale all'aver campionato $m$ elementi dalla distribuzione empirica $\hat{\mathcal{F}}$.
Questo può essere utilizzato come strumento per **estendere** la dimensione del dataset, ed è molto utile quando non si ha la possibilità di generare molti dataset.
Quando $m = n$ possiamo interpretarlo come **rigenerazione** del dataset.

# Bagging


