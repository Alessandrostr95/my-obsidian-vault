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

```julia
using Plots
using Distributions
using LaTeXStrings

# real function to predict
f(x) = sin(2x) + rand(Normal(0, .1))

# gaussian kernel function RBF
κ(x, h=.5) = ℯ^(-(abs(x)^2)/(2h^2))

Σ = sum
𝑤(x, i, X, h=.5) = κ(x-X[i], h)/(Σ([κ(x - xᵢ, h) for xᵢ ∈ X]))
y(x, X, t, h=.5) = Σ([𝑤(x, i, X, h)*t[i] for i ∈ 1:length(X)])

function kernel_regression(n::Int)
    X = rand(0:0.001:2, n)
    t = f.(X)
    scatter(X, t, label=:none)
    plot!(0:0.01:2, x -> sin(2x), label=L"\sin(2x)", w=2)
    plot!(0:0.01:2, x -> y(x,X,t, 0.1), label=L"y(x), h=0.1", w=2)
    plot!(0:0.01:2, x -> y(x,X,t, 0.25), label=L"y(x), h=0.25", w=2)
    plot!(0:0.01:2, x -> y(x,X,t, 0.5), label=L"y(x), h=0.5", w=2)
    plot!(title="\$ n = $n \$")
end

kernel_regression(50)
```

![[ML/img/ML_05_4.png]] ^836c80

[[#^836c80|Notare]] come per valori piccoli di $h$ il predittore tende ad andare in [[Prediction Risk|overfitting]], mentre viceversa per valori grandi di $h$ va in underfitting.