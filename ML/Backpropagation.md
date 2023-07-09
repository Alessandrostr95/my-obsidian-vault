Come visto, un [[Multilayer perceptrons|multilayerd perceptron]] è un **generalized linear model** "*[[Multilayer perceptrons#^e5d110|ricorsivo]]*".
Ciò che si vuole **apprendere** quindi sono le **matrici dei pesi** $\mathbf{w}^{(1)}, ..., \mathbf{w}^{(D)}$.

Purtroppo non si può applicare un approccio analitico per minimizzare una funzione di errore $E$, in quanto non si tratta di un modello lineare.
Quindi quello che si può fare è definire un approccio **iterativo**.

Il metodo migliore è l'applicazione di una tecnica di [[Gradient Descent]].
Sia $\mathbf{W}$ il **tensore** dei pesi della nostra rete.
Sai $$E(\mathbf{W}) = \sum_{i=1}^{n} E_i(\mathbf{W})$$ dove $E_i$ è l'errore rispetto all'$i$-esimo elemento del training set $\mathbf{X}$.
Questo è anche noto come [[Prediction Risk#^9cd1a0|rischio empirico]].

Possiamo usare tecniche iterative come lo [[Gradient Descent#Stochastic Gradient Descent|Stochastic Gradient Descent]] $$\mathbf{W}^{(k+1)} = \mathbf{W}^{(k)} - \eta \frac{\partial}{\partial \mathbf{W}}E_k(\mathbf{W}^{(k)})$$ oppure il [[Gradient Descent#Batch Gradient Descent|batch GD]] $$\mathbf{W}^{(k+1)} = \mathbf{W}^{(k)} - \eta\sum_{x_i \in B}\frac{\partial}{\partial \mathbf{W}}E_i(\mathbf{W}^{(k)})$$ dove $B$ è il batch considerato all'iterazione corrente.

# Da finire #todo