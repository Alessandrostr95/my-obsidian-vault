Abbiamo visto che nell'[[Probabilistic Model for Regression#Approccio Bayesiano|approccio baiesiano]] alla regressione l'obiettivo è quello di stimare la [[Maximum a Posteriori - Bayesian Approach#^7ee03b|probabilità a posteriori]] dei parametri $\mathbf{w}$, assumendo che **a priori** $\mathbf{w}$ sia distribuito secondo una gaussiana e che anche i target $\mathbf{t}$ siano distribuiti secondo una gaussiana con media $\mathbf{w}^T\phi(\mathbf{x})$.
$$\mathbf{w} \sim \mathcal{N}(\mathbf{0}, \alpha^{-1} I)$$
$$t \vert x, \mathbf{w} \sim \mathcal{N}(\mathbf{w}^T\phi(x), \beta^{-1})$$
Abbiamo poi visto che, sotto queste assunzioni, possiamo ricavare una distribuzione a posteriori di $\mathbf{w}$
$$p(\mathbf{w} \vert \mathbf{t}, \mathbf{X}, \alpha, \beta) \propto p(\mathbf{t} \vert \mathbf{w}, \mathbf{X}, \beta) \cdot p(\mathbf{w} \vert \alpha) = \left( \prod_{i=1}^{n} p(t \vert \mathbf{w}, x, \beta) \right) \cdot p(\mathbf{w} \vert \alpha)$$ la quale è una **gaussiana** con:
- varianza $$S = (\beta\Phi^T\Phi + \alpha I)^{-1}$$
- media $$\mu = \beta S\Phi^T \mathbf{t}$$
A differenza della regressione lineare classica, in cui si stima un unico set di coefficienti di regressione, la regressione **lineare completamente bayesiana** (o **fully bayesian regression**) considera i coefficienti di regressione come **variabili aleatorie** con distribuzioni di probabilità.
Così facendo, si può stimare **direttamente** la probabilità di una predizione $y$ semplicemente facendo la **media** rispetto a tutti i possibili parametri $\mathbf{w}$.

Perciò, assumendo di avere i campioni del dataset $\mathcal{T} = (\mathbf{X}, \mathbf{t})$ e di aver osservato un nuovo elemento $x$, vogliamo stimare la probabilità che $y$ sia il suo target, e possiamo farlo nel seguente modo.
$$p(y \vert x, \mathbf{X}, \mathbf{t}, \alpha, \beta) = \int \underbrace{p(y \vert x, \mathbf{w}, \beta)}_{\text{likelihood } L(\mathbf{w}\vert x,t,\beta)} \cdot \underbrace{p(\mathbf{w} \vert \mathbf{t}, \mathbf{X}, \alpha, \beta)}_{\text{posterior distribution}} \,d\mathbf{w}$$
Tale valore è anche detto **posterior predictive mean**. ^73334c

```ad-info
Ricordiamo che con $\mathbf{\Phi}$ si intende la matrice ottenuta applicando delle [[Some Base Function|funzioni base]] agli elementi $\mathbf{X}$.
```

È possibile dimostrare che tale distribuzione è ancora una **gaussiana**
$$y \vert x, \mathbf{X}, \mathbf{t}, \alpha, \beta \sim \mathcal{N}(\mu(x), \sigma^2(x))$$ con:
- media $$\mu(x) = \beta \phi(x)^TS \mathbf{\Phi}^T\mathbf{t}$$
- varianza $$\sigma^2(x) = \beta^{-1} + \phi(x)^T S \phi(x)$$

```ad-important
**Importante** notare che il calcolo della *posterior predictive mean* non diepnde da alcun parametro $\mathbf{w}$.
Per questo, è considerato un approccio **non parametrico**.
```


Nella [[#^0b25b4|figura]] possiamo vedere gli **intervalli di confidenza** della *posterior predictive mean* per ogni punto, al crescere delle osservazioni.

![[ML/img/ML_04_20.png]] ^0b25b4


```ad-note
Nella [[#^0b25b4|figura]] precedente sono state utilizzate 9 [[Some Base Function#Gaussian|base function gaussiane]], perciò i parametri da stimare erano 10.
Ovvero, per ogni putni $x$ abbiamo la retta di regressione
$$y(x,\mathbf{w}) = w_0 + w_1\phi_1(x) + ... + w_9\phi_9(x)$$ con $$\phi_i(x) = \exp{\left( -\frac{1}{2}\left( \frac{x - \mu_i}{\sigma} \right)^2\right)}$$
```


## Esempio
![[ML_04_21.png]]