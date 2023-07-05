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

Il percettrone è quindi un **[[Linear Discriminant Function in Binary Classification#Generalized discriminant functions|generalized linear model]]** dove $\phi$ che equivale alla **funzione identità**.

## Cost Function
Come al solito, si vuole trovare il valore dei parametri $\mathbf{w}$ che [[Prediction Risk#^9cd1a0|minimizzi un rischio empirico]].

Una funzione di costo abbasta naturale è il **numero di elementi** classificati male.
Purtroppo però, tale funzione risulta essere una funzione **costante** a gradini, e quindi non si può applicare la [[Gradient Descent|discesa del gradiente]].

![[ML/img/ML_06_6.png]]

