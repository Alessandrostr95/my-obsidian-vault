Nei metodi di **kernel regression** la predizione del target $t$ per ogni data osservazione $x$ è fatta facendo riferimento agli items del *training set* $\mathcal{T}$, specialmente a tutti quegli item più vicini ad $x$. (vedi [[Nonparametric Regression#Kernel as Covariance]]).

Questo riferimento agli elementi del training set viene *controllato* attraverso una [[Nonparametric Regression#^0e14e6|funzione kernel]] $\kappa_h(\cdot)$ che è **non nulla** solamente in un intorno di 0.
In genere il parametro $h$ indica di quanto è ampio l'intervallo non nullo di $\kappa_h$.

Un esempio è il **gaussian kernel** (o **RBF**) $$\kappa_h(x) = e^{-\tfrac{\Vert x \Vert^2}{2h^2}}$$
![[ML/img/ML_05_3.png]]

In generale nella regressione, siamo interessati a calcolare mediamente quanto vale il target $t$ (il quale è una **variabile aleatoria**) condizionato al fatto che abbiamo osservato un elemento $x$ (il nostro input).
$$y(x) = \mathbb{E}\left[ t \vert x\right] = \int_{\mathbb{R}} t \cdot p(t \vert x) \,dt = \int_{\mathbb{R}} t \cdot \frac{p(t,x)}{p(x)} \,dt = \frac{\displaystyle\int_{\mathbb{R}} t \cdot p(t,x) \,dt}{p(x)} = \frac{\displaystyle\int_{\mathbb{R}} t \cdot p(t,x) \,dt}{\displaystyle\int_{\mathbb{R}} p(t,x) \,dt}$$

Possiamo approssimare la probabilità congiunta $p(t,x)$ con la media di una kernel function come $$p(t,x) \approx \frac{1}{n}\sum_{i=1}^{n} \kappa_h(x-x_i)\kappa_h(t-t_i)$$

Perciò avremo che
$$\begin{align}
y(x)
&= \mathbb{E}\left[ t \vert x\right]\\
&= \frac{\displaystyle\int_{\mathbb{R}} t \cdot p(t,x) \,dt}{\displaystyle\int_{\mathbb{R}} p(t,x) \,dt}\\
&\approx \frac{\displaystyle\int_{\mathbb{R}} \frac{1}{n} \displaystyle\sum_{i=1}^{n}\kappa_h(x-x_i)\kappa(t-t_i) \cdot t \,dt}{\displaystyle\int_{\mathbb{R}} \frac{1}{n} \displaystyle\sum_{i=1}^{n}\kappa_h(x-x_i)\kappa(t-t_i) \,dt}\\
&= \frac{\displaystyle\sum_{i=1}^{n}\kappa_h(x-x_i) \left(\displaystyle\int_{\mathbb{R}} \kappa(t-t_i) \cdot t \,dt \right)}{\displaystyle\sum_{i=1}^{n}\kappa_h(x-x_i)\left(\displaystyle\int_{\mathbb{R}} \kappa(t-t_i) \,dt \right)}
\end{align}$$

Se assumiamo che la funzione kernel $\kappa_h$ sia una **distribuzione** allora abbiamo che $$\int_{\mathbb{R}}\kappa_h(t-t_i) \,dt = 1$$ e $$\int_{\mathbb{R}}\kappa_h(t-t_i) \cdot t \,dt = t_i$$

In conclusione, la media risulterà essere $$y(x) = \mathbb{E}\left[ t \vert x\right] \approx \frac{\sum_{i=1}^{n}\kappa_h(x-x_i) t_i}{\sum_{i=1}^{n}\kappa_h(x-x_i)}$$

Osservare che se poniamo $$w_i(x) = \frac{\kappa_h(x-x_i)}{\sum_{j=1}^{n}\kappa_h(x-x_j)}$$ avremo che la predizione risulta essere una **combinazione lineare** dei **valori target del dataset** $$y(x) = \sum_{i=1}^{n}w_i(x) \cdot t_i$$ dove però i coefficienti dipendono anche dall'osservazione in input $x$.