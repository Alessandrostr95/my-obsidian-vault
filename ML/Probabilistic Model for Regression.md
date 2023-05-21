eSupponiamo che dato un punto $\mathbf{x} \in \mathcal{X}$ il relativo target sconoscituo $t$ sia **distribuito** nell'intorno del valore dato dal modello $\mathbf{w}^T\mathbf{x}$, con una **fissata** varianza $\sigma^2=\beta^{-1}$ $$t_i \sim \mathcal{N}(y(x_i, \mathbf{w}), \beta^{-1})$$ o $$p(t \;\vert\; x_i, \mathbf{w}, \beta) = \mathcal{N}(t \;\vert\; y(x_i, \mathbf{w}), \beta^{-1})$$

![[ML/img/ML_04_13.png]]


Con questo approccio si vogliono quindi stiamare i migliori valori dei parametri $\mathbf{x}, \beta$ che minimizzino una [[Probabilistic Learning#Misura di qualità $q$|misura di qualità]].

# Approccio frequentista
Con un approccio frequentista si assume che $\mathbf{w}, \beta$ sono parametri ignoti da stimare mediate le nostre osservazioni $\mathcal{T}$.
In genere si cerca di identificare uno [[Stimatore di Massima Verosimiglianza]] $$L(\mathbf{t} \;\vert\; \mathbf{X}, \mathbf{w}, \beta) = p(\mathbf{t} \;\vert\; \mathbf{X}, \mathbf{w}, \beta) = \prod_{i=1}^{n} \mathcal{N}(t_i \;\vert\; x_i, \mathbf{w}, \beta^{-1})$$ o in alternativa la log-verosimiglianza $$\begin{align}
l(\mathbf{t} \;\vert\; \mathbf{X}, \mathbf{w}, \beta)
&= \log{L(\mathbf{t} \;\vert\; \mathbf{X}, \mathbf{w}, \beta)}\\
&= \sum_{i=1}^{n} \log{\mathcal{N}(t_i \;\vert\; x_i, \mathbf{w}, \beta^{-1})}\\
&= -\frac{\beta}{2}\sum_{i=1}^{n}(t_i - y(x_i, \mathbf{w}))^2 + \frac{n}{2}\log{\beta} + \text{const}
\end{align}$$

La massimizzazione rispetto al solo paremtro $\mathbf{w}$ è data dalla massimizzazione della sola funzione $$\frac{1}{2}\sum_{i=1}^{n}(t_i - y(x_i, \mathbf{w}))^2$$ ovvero minimizzando gli [[Gradient Descent#^f0316c|scarti quadratici]].

Invece, per quanto riguarda la minimizzazione del parametro $\beta$ basta annullare la sola derivata parziale $$\dfrac{\partial}{\partial \beta}l(\mathbf{t} \; \vert\; \mathbf{X}, \mathbf{w}, \beta) = -\frac{1}{2}\sum_{i=1}^{n}(t_i - y(x_i, \mathbf{w}))^2 + \frac{n}{2\beta}$$ ovvero quando $$\beta_{ML}^{-1} = \frac{1}{n}\sum_{i=1}^{n}(t_i - y(x_i, \mathbf{w}))^2$$

Come risultato avremo, per ogni punto $x$, una **distribuzione** gaussiana con media $y(x, \mathbf{w}_{ML})$ e varianza $\beta_{ML}^{-1}$ $$p(t \;\vert\; x, \mathbf{w}_{ML}, \beta_{ML}) = \sqrt{\frac{\beta_{ML}}{2\pi}} \cdot e^{-\frac{\beta_{ML}}{2}(t - y(x, \mathbf{w}_{ML}))^2}$$

# Approccio Bayesiano
Assumiamo che il parametro ignoto $\mathbf{w}$ sia distribuito anch'esso secondo una [[CLT - Central Limit Theorem#Normali Multivariate|gaussiana multivariata]] con media $\mathbf{0}$ e matrice di covarianza $\alpha^{-1}\mathbf{I}$ $$p(\mathbf{w} \vert \alpha) = \mathcal{N}(\mathbf{w} \,\vert\, \mathbf{0}, \alpha^{-1}\mathbf{I}) = \left( \frac{\alpha}{2\pi}\right)^{\frac{m+1}{2}}\cdot e^{-\frac{\alpha}{2}\mathbf{w}^T\mathbf{w}}$$
![[ML/img/ML_04_14.png]]