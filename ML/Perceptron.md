Introdotto nel 1958 da [Frank Rosenblatt](https://it.wikipedia.org/wiki/Frank_Rosenblatt), il **percettrone** è il modello alla base delle reti neurali.
Semplicemente il percettrone esegue una **combinazione lineare** del suo input $\mathbf{x}$ con dei pesi $\mathbf{w}$, ed applica al risultato una **funzione non lineare** $f$.
La funzione $f$ viene interpretata come una **funzione di attivazione** del percettrone, richiamando infatti il comportamento dei neuroni biologici.

![[ML/img/ML_06_4.png]]

![[ML/img/ML_06_5.png]]


In termini formali, il percettrone equivale semplicemente a un modello di [[Linear Classification|classificazione lineare binaria]], dove la funzione $f$ è la funzione **discriminante** che indica a quale classe appartiene un elemento dopo aver effettuato una [[Linear Discriminant Analysis - Fisher Linear Discriminant|proiezione lineare]].

In altri termini $$y(\mathbf{x}) = f(\mathbf{w}^T\mathbf{x})$$ dove $f$ è sostanzialmente la **funzione segno** $\text{sign}$
$$f(y) = \begin{cases}
1 &\text{if } y \geq 0\\
-1 &\text{if } y < 0
\end{cases}$$

```ad-note
Nella rappresentazione classica delle classi, indicavamo con $t = 0$ la classe $C_1$ e con $t = 1$ la classe $C_2$.
In questo caso invece indicheremo con $t = 0$ la classe $C_1$ e con $t = -1$ la classe $C_2$.
```

^132e5b

Il percettrone è quindi un **[[Generalized Linear Model|generalized linear model]]** dove $\phi$ che equivale alla **funzione identità**.

## Cost Function
Come al solito, si vuole trovare il valore dei parametri $\mathbf{w}$ che [[Prediction Risk#^9cd1a0|minimizzi un rischio empirico]].

Una funzione di costo abbasta naturale è il **numero di elementi** classificati male.
Purtroppo però, tale funzione risulta essere una funzione **costante a blocchi**, e quindi non si può applicare la [[Gradient Descent|discesa del gradiente]].

![[ML/img/ML_06_6.png]]

L'ideale invece sarebbe una funzione di costo **lineare a blocchi**.
Per come è definito il percettrone (e [[#^132e5b|codifica delle classi]]), noi vogliamo trovare un vettore di parametri $\mathbf{w}$ tale che per ogni elemento del dataset $\mathbf{x}_i$ abbiamo che
$$\begin{cases}
\mathbf{w}^T\mathbf{x}_i > 0 &\text{if } \mathbf{x} \in C_1\\
\mathbf{w}^T\mathbf{x}_i < 0  &\text{if } \mathbf{x} \in C_2
\end{cases}$$
Possiamo riscrivere in **forma compatta** la formula $$\mathbf{w}^T\mathbf{x}_i t_i > 0 \;\; \forall \mathbf{x}_i \in \mathbf{X}$$

Sia $\mathcal{M}$ l'insieme degli elementi **classificati male**, allora ogni elemento $\mathbf{x}_i$ contribuisce alla funzione di costo nel seguente modo:
$$\text{err}(\mathbf{x}_i \vert \mathbf{w}) = \begin{cases}
0 &\text{if } \mathbf{x}_i \notin \mathcal{M}\\
-\mathbf{w}^T\mathbf{x}_i t_i > 0 &\text{if } \mathbf{x}_i \in \mathcal{M}
\end{cases}$$

Perciò, la funzione di costo sarà della forma $$E(\mathbf{w}) = \sum_{\mathbf{x} \in \mathcal{M}} \text{err}(\mathbf{x}_i \vert \mathbf{w}) = -\sum_{\mathbf{x} \in \mathcal{M}}\mathbf{w}^T\mathbf{x}_i t_i$$

## Gradient Optimization
Applicando la [[Gradient Descent|discesa del gradiente]] avremo la seguente formula di aggiornamento dei parametri $$\mathbf{w}^{(k+1)} = \mathbf{w}^{(k)} -\eta\frac{\partial}{\partial\mathbf{w}}E(\mathbf{w}) = \mathbf{w}^{(k)} + \eta\sum_{\mathbf{x} \in \mathcal{M}_k}\mathbf{x}_i t_i$$ dove $\mathcal{M}_k$ è l'insieme degli elementi classificati male al tempo $k$.

In genere si preferisce usare lo [[Gradient Descent#Stochastic Gradient Descent|Stochastic Gradient Descent]] $$\mathbf{w}^{(k+1)} = \mathbf{w}^{(k)} + \eta\mathbf{x}_i t_i$$ dove $\mathbf{x}_i \in \mathcal{M}_k$ è un elemento presente nell'insieme di quelli classificati male al tempo $k$.
Questo metodo consiste nel iterare in maniera circolare su tutti gli elementi.

Osserviamo che, se l'elemento $\mathbf{x}_i$ considerato al tempo $k$ è classificato male, allora il suo contributo al tempo successivo sarà il seguente
$$\begin{align}
-t_i\mathbf{x}_i^T\mathbf{w}^{(k+1)}
&= -t_i\mathbf{x}_i^T\mathbf{w}^{(k)} - \eta t_i\mathbf{x}_i^T\mathbf{x}_it_i\\
&= -t_i\mathbf{x}_i^T\mathbf{w}^{(k)} - \eta  \Vert\mathbf{x}_i\Vert^2\\
&< -t_i\mathbf{x}_i^T\mathbf{w}^{(k)}
\end{align}$$
Questo garantisce che il contributo al costo dell'elemento $\mathbf{x}_i$ **decresce** però non è sufficiente a garantire la convergenza del metodo, in quanto $\mathbf{w}^{(k+1)}$ potrebbe tranquillamente **aumentare** il costo di altri elementi.

Esiste però il **Teorema di convergenza del Percettrone** che ci garantisce la convergenza di questo metodo, solo se le classi sono **linearmente separabili**.
