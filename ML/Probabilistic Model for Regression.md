Supponiamo che dato un punto $\mathbf{x} \in \mathcal{X}$ il relativo target $t$ (*sconosciuto*) sia **distribuito** nell'intorno del valore dato dal modello $\mathbf{w}^T\mathbf{x}$, con una **fissata** varianza $\sigma^2=\beta^{-1}$ $$t_i \sim \mathcal{N}(y(x_i, \mathbf{w}), \beta^{-1})$$ o $$p(t \;\vert\; x_i, \mathbf{w}, \beta) = \mathcal{N}(t \;\vert\; y(x_i, \mathbf{w}), \beta^{-1})$$

![[ML/img/ML_04_13.png]]


Con questo approccio si vogliono quindi stimare i migliori valori dei parametri $\mathbf{w}, \beta$ che minimizzino una [[Probabilistic Learning#Misura di qualità $q$|misura di qualità]].

# Approccio frequentista
Con un approccio *frequentista* si assume che $\mathbf{w}, \beta$ sono parametri ignoti da stimare mediate le nostre osservazioni $\mathcal{T}$.
In genere si cerca di identificare uno [[Stimatore di Massima Verosimiglianza]] $$L(\mathbf{t} \;\vert\; \mathbf{X}, \mathbf{w}, \beta) = p(\mathbf{t} \;\vert\; \mathbf{X}, \mathbf{w}, \beta) = \prod_{i=1}^{n} \mathcal{N}(t_i \;\vert\; \mathbf{w}^T\Phi(x_i), \beta^{-1})$$ o in alternativa la *log-verosimiglianza* $$\begin{align}
l(\mathbf{t} \;\vert\; \mathbf{X}, \mathbf{w}, \beta)
&= \log{L(\mathbf{t} \;\vert\; \mathbf{X}, \mathbf{w}, \beta)}\\
&= \sum_{i=1}^{n} \log{\mathcal{N}(t_i \;\vert\; \mathbf{w}^T\Phi(x_i), \beta^{-1})}\\
&= -\frac{\beta}{2}\sum_{i=1}^{n}(t_i - \mathbf{w}^T\Phi(x_i))^2 + \frac{n}{2}\log{\beta} + \text{const}
\end{align}$$

La massimizzazione rispetto al solo parametro $\mathbf{w}$ è data dalla massimizzazione della sola funzione $$\frac{1}{2}\sum_{i=1}^{n}(t_i - y(x_i, \mathbf{w}))^2$$ ovvero minimizzando gli [[Gradient Descent#^f0316c|scarti quadratici]].

```ad-note
Ricorda che $$y(x_i, \mathbf{w}) = \mathbf{w}^T\Phi(x_i)$$
```


Invece, per quanto riguarda la minimizzazione del parametro $\beta$ basta annullare la sola derivata parziale $$\dfrac{\partial}{\partial \beta}l(\mathbf{t} \; \vert\; \mathbf{X}, \mathbf{w}, \beta) = -\frac{1}{2}\sum_{i=1}^{n}(t_i - y(x_i, \mathbf{w}))^2 + \frac{n}{2\beta}$$ ovvero quando $$\beta_{ML}^{-1} = \frac{1}{n}\sum_{i=1}^{n}(t_i - y(x_i, \mathbf{w}))^2$$

Come risultato avremo, per ogni punto $x$, una **distribuzione** gaussiana con media $y(x, \mathbf{w}_{ML})$ e varianza $\beta_{ML}^{-1}$ $$p(t \;\vert\; x, \mathbf{w}_{ML}, \beta_{ML}) = \sqrt{\frac{\beta_{ML}}{2\pi}} \cdot e^{-\tfrac{\beta_{ML}}{2}(t - y(x, \mathbf{w}_{ML}))^2}$$

# Approccio Bayesiano
Assumiamo che il parametro ignoto $\mathbf{w}$ sia distribuito anch'esso secondo una [[CLT - Central Limit Theorem#Normali Multivariate|gaussiana multivariata]] con media $\mathbf{0}$ e matrice di covarianza $\alpha^{-1}\mathbf{I}$ $$p(\mathbf{w} \vert \alpha) = \mathcal{N}(\mathbf{w} \,\vert\, \mathbf{0}, \alpha^{-1}\mathbf{I}) = \left( \frac{\alpha}{2\pi}\right)^{\tfrac{m+1}{2}}\cdot e^{-\tfrac{\alpha}{2}\mathbf{w}^T\mathbf{w}}$$
![[ML/img/ML_04_14.png]] ^202a9e

Il vantaggio di assumere una distribuzione **gaussiana a priori** è che se anche la propbabilità $p(t \vert x)$ è gaussiana allora anche la **probabilità a posteriori** sarà gaussiana. ^600665

Ovvero avremo la probabilità a posteriori $$p(\mathbf{w} \,\vert\, \mathbf{t}, \mathbf{X}, \alpha, \beta) = \frac{p(\mathbf{t} \,\vert\, \mathbf{X}, \mathbf{w}, \beta) \cdot p(\mathbf{w} \,\vert\, \alpha)}{p(\mathbf{t} \,\vert\, \mathbf{X})} \propto p(\mathbf{t} \,\vert\, \mathbf{X}, \mathbf{w}, \beta) \cdot p(\mathbf{w} \,\vert\, \alpha)$$

## Gausian a Priori 2 Gaussian a Posteriori
Come accennato, [[#^600665|il coniugato di una gaussiana è ancora una gaussiana]].
Più in generale, avremo che:
- data una **gaussiana a priori** con media $m_0$ e matrice di covarianza $\Sigma_0$ $$p(\mathbf{w}) = \mathcal{N}(\mathbf{w} \,\vert\, m_0, \Sigma_0)$$
- data una **gaussiana multivariata** $$p(\mathbf{t} \,\vert\, \mathbf{\Phi}, \mathbf{w}, \beta) = \prod_{i=1}^{n}\mathcal{N}(t_i \,\vert\, \mathbf{w}^T\Phi(\mathbf{x}_i), \beta^{-1})$$ con vettore media $(\mathbf{w}^T\Phi(\mathbf{x}_1), ..., \mathbf{w}^T\Phi(\mathbf{x}_n))$ e matrice di covarianza $\beta^{-1}\mathbf{I}$

Allora la probabilità a posteriori sarà anch'essa una gaussiana $$p(\mathbf{w} \,\vert\, \mathbf{t}, \mathbf{\Phi}) = \mathcal{N}(\mathbf{w} \,\vert\, m_p, \Sigma_p)$$  con $$\Sigma_p = (\Sigma_0^{-1} + \beta\mathbf{\Phi}^{T}\mathbf{\Phi})^{-1}$$ ed $$m_p = \Sigma_p(\Sigma_0^{-1}m_0 + \beta\mathbf{\Phi}^{T}\mathbf{t})$$

Nel [[#^202a9e|precedente esempio]], avremo quindi che $$p(\mathbf{w} \,\vert\, \mathbf{t}, \mathbf{X}, \alpha, \beta) = \mathcal{N}(\mathbf{w} \,\vert\, m_p, \Sigma_p)$$ cone $$\Sigma_p = (\alpha\mathbf{I} + \beta\mathbf{\Phi}^{T}\mathbf{\Phi})^{-1}$$ e $$m_p = \beta\Sigma_p\mathbf{\Phi}^{T}\mathbf{t}$$

```ad-note
Osservare che quando $\alpha \to 0$ avremo che $$\Sigma_p \to (\beta\mathbf{\Phi}^{T}\mathbf{\Phi})^{-1}$$ e quindi che $$m_p \to (\mathbf{\Phi}^{T}\mathbf{\Phi})^{-1}\mathbf{\Phi}^{T}\mathbf{t}$$

Ovvero che $m_p$ equivale alla [[Linear Regression#^6eecde|formula chiusa]] che azzera il gradiente per la [[Linear Regression#Regression Loss|Regression Loss]].
```

## Maximum a Posteriori
Ricapitolando, abbiamo che $$p(\mathbf{w} \,\vert\, \mathbf{t}, \mathbf{X}, \alpha, \beta) \propto p(\mathbf{t} \,\vert\, \mathbf{X}, \mathbf{w}, \beta) \cdot p(\mathbf{w} \,\vert\, \alpha)$$
Stiamo assumendo inoltre che 
$$\mathbf{w} \sim \mathcal{N}(\mathbf{0}, \alpha^{-1}I)$$ e $$\mathbf{t} \sim \mathcal{N}(\mathbf{w}^T\mathbf{X}, \beta^{-1}I)$$

Perciò abbiamo che $$p(\mathbf{w} \,\vert\, \mathbf{t}, \mathbf{X}, \alpha, \beta) \propto \left(\prod_{i=1}^{n}\sqrt{\frac{\beta}{2\pi}}e^{-\tfrac{\beta}{2}(t_i - \mathbf{w}^Tx_i)^2}\right)\left(\frac{\alpha}{2}\right)^{\tfrac{M+1}{2}}e^{-\tfrac{\alpha}{2}\mathbf{w}^T\mathbf{w}}$$
Abbiamo anche [[#Gausian a Priori 2 Gaussian a Posteriori|visto prima]] che tale distribuzione è ancora una gaussiana con matrice di covarianza $$\Sigma = (\alpha\mathbf{I} + \beta\mathbf{X}^{T}\mathbf{X})^{-1}$$ e media $$m = \beta\Sigma\mathbf{X}^{T}\mathbf{t}$$

Vogliamo ora trovare il valore di $\mathbf{w}$ che **massimizzi** la sua **[[Maximum a Posteriori - Bayesian Approach#^7ee03b|probabilità a posteriori]]** $$\begin{align}
\mathbf{w}_{MAP}
&= \arg \max_{\mathbf{w}} p(\mathbf{w} \,\vert\, \mathbf{t}, \mathbf{X}, \alpha, \beta)\\
&= \arg \max_{\mathbf{w}} \left(\prod_{i=1}^{n}\sqrt{\frac{\beta}{2\pi}}e^{-\tfrac{\beta}{2}(t_i - \mathbf{w}^Tx_i)^2}\right)\left(\frac{\alpha}{2\pi}\right)^{\tfrac{M+1}{2}}e^{-\tfrac{\alpha}{2}\mathbf{w}^T\mathbf{w}}
\end{align}$$

Per comodità possiamo considerare di massimizzare il **logaritmo** della probabilità a posteriori, ovvero massimizzare
$$\begin{align}
&\log(p(\mathbf{w} \,\vert\, \mathbf{t}, \mathbf{X}, \alpha, \beta))\\
\\
=&\frac{n}{2}\log{\frac{\beta}{2\pi}} - \frac{\beta}{2}\sum_{i=1}^{n}(t_i - \mathbf{w}^Tx_i)^2 + \frac{M+1}{2}\log{\frac{\alpha}{2\pi}} - \frac{\alpha}{2}\mathbf{w}^T\mathbf{w}
\end{align}$$

Possiamo ignorare tutti gli elementi che **non** dipendono da $\mathbf{w}$ e quindi considerare di massimizzare la sola funzione
$$\begin{align}
\mathbf{w}_{MAP}
&= \arg \max_{\mathbf{w}} - \frac{\beta}{2}\sum_{i=1}^{n}(t_i - \mathbf{w}^Tx_i)^2 - \frac{\alpha}{2}\mathbf{w}^T\mathbf{w}\\
&= \arg \min_{\mathbf{w}} \frac{\beta}{2}\sum_{i=1}^{n}(t_i - \mathbf{w}^Tx_i)^2 + \frac{\alpha}{2}\Vert\mathbf{w}\Vert^2\\
\end{align}$$

```ad-note
Per quanto riguarda il valore della varianza $\beta$, possiamo tranquillamente stimarlo come la [[Random Sample#Varianza campionaria|varianza campionaria]] sulle nostre osservazioni $\mathcal{T}$.
$$\beta = \frac{1}{n-1}\sum_{i=1}^{n}(t_i - \overline{t})^2$$
il quale è facile dimostrare corrispondere allo [[Stimatore di Massima Verosimiglianza]].
```

----
## Esempio
Supponiamo di avere in input un valore reale $x$ e di voler predire il suo valore target $t$, tramite una **regressione lineare** $y(x,w_0,w_1) = w_0 + w_1 x$.
Assumiamo che la reale funzione target è $$y(x) = a_0 + a_1 x$$ con $a_i$ campionato **uniformemente** in $\left[ -1, 1\right]$ con un **rumore gaussiano** campionato da $\mathcal{N}(0, 0.2)$.

Assumiamo **a priori** che $(w_0, w_1)$ sono campionati dalla gaussiana bivariata $$\sim \mathcal{N}(\mathbf{0}, 0.04I)$$

![[ML/img/ML_04_15.png]] ^04a809

In [[#^04a809|figura]] abbiamo a sinistra la distribuzione iniziale di $(w_0, w_1)$ e a destra 6 rette campionate da $\mathcal{N}(\mathbf{0}, 0.04I)$.

Supponiamo di osservare $(x_1, t_1)$.
Possiamo quindi calcolare la **probabilità a posteriori** come visto prima
$$p(w_0, w_1 \vert (x_1, t_1)) \propto p(t_1 \vert x_1, w_0,w_1) \cdot p(w_0,w_1)$$
Questa sappiamo essere una gaussiana.
![[ML/img/ML_04_16.png]]

Continuando, avremo la seguente distribuzione osservando i campioni $(x_1, t_1), (x_2, t_2)$.
![[ML/img/ML_04_17.png]]

Così via, sempre meglio al crescere dei campioni
![[ML/img/ML_04_18.png]]

Nella [[#^6e6bb0|seguente figura]] possiamo vedere delle funzioni campionate dalla **distribuzione a posteriori**, al crescere delle osservazioni.

![[ML/img/ML_04_19.png]]

```ad-note
Nella [[#^6e6bb0|figura]] precedente sono state utilizzate 9 [[Some Base Function#Gaussian|base function gaussiane]], perciò i parametri da stimare erano 10.
Ovvero, per ogni putni $x$ abbiamo la retta di regressione
$$y(x,\mathbf{w}) = w_0 + w_1\phi_1(x) + ... + w_9\phi_9(x)$$ con $$\phi_i(x) = \exp{\left( -\frac{1}{2}\left( \frac{x - \mu_i}{\sigma} \right)^2\right)}$$
```
