Un altro approccio non parametri molto importante è la **Gaussian Processes Regression** (**GPR**).
L'idea di base è molto simile alla [[Fully Bayesian Approach]] per la regressione lineare.
Assumiamo di avere come **conoscenza a priori** una **distribuzione** di probabilità su una famiglia infinita di funzioni che descrivono i nostri dati.
Questa conoscenza a priori descrive come dovrebbe comportarsi la funzione target $f$ dato un qualsiasi input $x$, senza però aver fatto alcuna osservazione.
Quando iniziamo ad avere osservazioni (il dataset), invece di un numero infinito di funzioni, consideriamo solamente quelle funzioni che si adattano ai punti dati osservati.
Una volta fatte queste osservazioni, possiamo aggiornare la nostra conoscenza a priori per generare una **distribuzione a posteriori**.
Possiamo poi **iterare** questa procedura ogni volta che abbiamo nuove osservazioni.

Più formalmente, un **Gaussian Process Model** descrive una distribuzione di probabilità su tutte le funzioni possibili che passano attraverso i **punti osservati**.
Dato che abbiamo delle distribuzioni di probabilità su tutte queste funzioni, possiamo utilizzare come predittore la **media** di queste funzioni, e usare la varianza come **misura di confidenza** delle predizioni.

Il termine Gaussiano viene utilizzato perché viene assunto che a priori le funzioni sono **normalmente distribuite**.

Ricapitolando, i concetti chiave sono:
1. la distribuzione delle funzioni viene **aggiornata** man mano che si fanno nuove osservazioni.
2. un Gaussian Process Model è una **distribuzione di probabilità** su funzioni, e questa distribuzione di funzioni segue una [[Distribuzioni#Normale|distribuzione gaussiana]].
3. la **media** delle funzioni secondo la distribuzioni a posteriori è usata come funzione per fare predizioni.

Consideriamo l'insieme di punti $\mathbf{X} = (x_1, ..., x_m) \subseteq \mathbb{R}^d$, e sia $\mathcal{H}$ la **famiglia di funazioni** $f: \mathbb{R}^d \to \mathbb{R}$ (la famiglia di tutti i possibili predittori).
**Per semplicità**, assumiamo che le funzioni in $\mathcal{H}$ abbiano un dominio **finito**, ovvero che $f: \mathbf{X} \to \mathbb{R}$.
Questa assunzione verrà poi rimossa e la descrizione **generalizzata in seguito**.
Perciò per descrivere una qualsiasi funzione $f \in \mathcal{H}$ possiamo usare il **vettore** $m$-dimensionale $\mathbf{f} = f(\mathbf{X}) = (f(x_1), ..., f(x_m)) \in \mathbb{R}^m$.
Così facendo abbiamo definito una **biezione** tra $\mathcal{H}$ e $\mathbb{R}^m$.

Per semplicità, al momento assumiamo che $p(\mathbf{f})$ segua una [[CLT - Central Limit Theorem#Normali Multivariate|distribuzione gaussiana multivariata]] con media $\mathbf{0} = (0, ..., 0)$ e matrice di covarianza $\sigma^2I$.
$$p(\mathbf{f}) = p(\mathbf{f} \vert \mathbf{X}, \sigma^2) = \prod_{i=1}^{m} \frac{1}{\sqrt{2\pi\sigma^2}}e^{-\tfrac{f(x_i)^2}{2\sigma^2}} =\frac{1}{(2\pi\sigma^2)^{m/2}}e^{-\tfrac{1}{2\sigma^2}\Vert \mathbf{f} \Vert^2}$$

```ad-attention
title: Warning
Dato che stiamo considerando come **matrice di covarianza** la **matrice diagonale** $\sigma^2I$ ciò equivale a dire che tutte le predizioni $y_i = f(x_i)$ sono **indipendenti tra loro**.
Per modellare la **dipendenza** tra le predizioni, basta generalizzare utilizzando una matrice di convarianza non diagonale.
```

Assumiamo ora di avere a disposizione i valori target $\mathbf{t} = (t_1, ..., t_m)$ corrispondenti ai punti $\mathbf{X}$.

Come per il [[Fully Bayesian Approach|fully bayesian regression]] possiamo **aggiornare** la distribuzione $p(\mathbf{f})$ utilizzando la **formula di bayes** $$p(\mathbf{f} \vert \mathbf{X}, \mathbf{t}) \propto \underbrace{p(\mathbf{t} \vert \mathbf{X}, \mathbf{f})}_{\text{likelihood}} \cdot \underbrace{p(\mathbf{f}  \vert \mathbf{X})}_{\text{prior}}$$

Affinché la **distribuzione a posteriori** sia una **gaussiana** è necessario assumere che anche la verosimiglianza $p(\mathbf{t} \vert \mathbf{X}, \mathbf{f})$ sia **gaussiana**.
Perciò, assumendo che dato un valore $x$ il suo target $t$ segua la gaussiana $$t \vert x, \beta \sim \mathcal{N}(f(x), \beta^{-1})$$ avremo che $$p(\mathbf{t} \vert \mathbf{X}, \mathbf{f}) = \prod_{i=1}^{m} p(t_i \vert x_i, f(x_i))$$

In conclusione, come [[Fully Bayesian Approach|già visto]], sarà anch'essa una **gaussiana** con:
- varianza $$S = (\beta\mathbf{X}^T\mathbf{X} + \sigma^2 I)^{-1}$$
- media $$\mu = \beta S\mathbf{X}^T \mathbf{t}$$
## Generalization
Nel caso più generale  $p(\mathbf{f}) = p(f(\mathbf{X}))$ segue una distribuzione gaussiana multivariata $$\mathbf{f} \vert \mathbf{X} \sim \mathcal{N}(\mu(\mathbf{X}), \Sigma(\mathbf{X}))$$

In un processo gaussiano, la matrice di covarianza è definita come $$\Sigma(\mathbf{X}) = \kappa(\mathbf{X}) + \varepsilon \mathbf{I}$$ dove
$$\kappa(\mathbf{X}) = \begin{pmatrix}
\kappa(x_1,x_1) &\kappa(x_1,x_2) &... &\kappa(x_1,x_m)\\
\kappa(x_2,x_1) &\kappa(x_2,x_2) &... &\kappa(x_2,x_m)\\
\vdots &\vdots &\ddots &\vdots\\
\kappa(x_m,x_1), &\kappa(x_m,x_2), &..., &\kappa(x_m,x_m)\\
\end{pmatrix}$$
e
$$\kappa(x_i,x_j) = \sigma^2 \exp\left(-\frac{1}{2\ell^2}(x_i-x_j)^2\right)$$
I **meta parametri** $\sigma^2, \ell, \varepsilon$ possono essere interpretati nel seguente modo:
- $\ell$ è un **fattore di stretch orizzontale**. Più è alto il suo valore, più la funzione risulta **morbida** ![[ML/img/ML_05_5.png]]
-  $\sigma^2$ è un **fattore di stretch verticale**. Più è alto il suo valore, più la funzione risulta assumi valori "**ampi**" ![[ML/img/ML_05_6.png]]
- $\varepsilon$ è un **rumore** ![[ML/img/ML_05_7.png]]

Come **distribuzione a priori** abbiamo $$\mathbf{f} \vert \mathbf{X} \sim \mathcal{N}(\mathbf{0} \vert \Sigma(\mathbf{X}))$$

Supponiamo ora di ottenere un nuovo set di osservazioni $\mathbf{X}^* = (x^*_1, ..., x^*_r)$.
La distribuzione del vettore $(\mathbf{f}, \mathbf{f}^*)$, con $\mathbf{f} = f(\mathbf{X})$ e $\mathbf{f}^* = f(\mathbf{X}^*)$, segue sempre una **gaussiana** con media $$\mu(\mathbf{X},\mathbf{X}^*) = (\mu(\mathbf{X}), \mu(\mathbf{X}^*))$$ e matrice di covarianza
$$\Sigma(\mathbf{X},\mathbf{X}^*) = \begin{bmatrix}
\kappa(\mathbf{X}) &\kappa(\mathbf{X},\mathbf{X}^*)\\
\kappa(\mathbf{X},\mathbf{X}^*)^T &\kappa(\mathbf{X}^*)\\
\end{bmatrix}$$
dove
$$\kappa(\mathbf{X},\mathbf{X}^*) = \begin{pmatrix}
\kappa(x_1,x^*_1) &\kappa(x_1,x^*_2) &... &\kappa(x_1,x^*_r)\\
\kappa(x_2,x^*_1) &\kappa(x_2,x^*_2) &... &\kappa(x_2,x^*_r)\\
\vdots &\vdots &\ddots &\vdots\\
\kappa(x_m,x^*_1), &\kappa(x_m,x^*_2), &..., &\kappa(x_m,x^*_r)\\
\end{pmatrix}$$

In sostaza $$\begin{bmatrix} \mathbf{f}\\ \mathbf{f}^*\end{bmatrix} \sim \mathcal{N}\left( \begin{bmatrix}
\mu(\mathbf{X})\\
\mu(\mathbf{X}^*)\\
\end{bmatrix} \;\Bigg\vert\; \begin{bmatrix}
\kappa(\mathbf{X}) &\kappa(\mathbf{X},\mathbf{X}^*)\\
\kappa(\mathbf{X},\mathbf{X}^*)^T &\kappa(\mathbf{X}^*)\\
\end{bmatrix}\right)$$

Usando la formula di Bayes, avremo che i valori della funzione $\mathbf{f}^*$ saranno distribuiti come
$$\mathbf{f}^* \vert \mathbf{X}, \mathbf{X}^*, \mathbf{f} \sim \mathcal{N}(\tilde\mu, \tilde\Sigma)$$

$$\tilde\mu = \mu(\mathbf{X}^*) + \kappa(\mathbf{X},\mathbf{X}^*)^T\kappa(\mathbf{X},\mathbf{X})^{-1}(\mathbf{f} - \mu(\mathbf{X}))$$
$$\tilde\Sigma = \kappa(\mathbf{X}^*) - \kappa(\mathbf{X}^*, \mathbf{X})\kappa(\mathbf{X})^{-1}\kappa(\mathbf{X}^*, \mathbf{X})^T$$
![[ML_05_8.png]]