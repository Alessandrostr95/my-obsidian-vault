# Normale
### Notazione
$$N(\mu, \sigma^2)$$
### Parametri
- **media** $\mu$
- **varianza** $\sigma^2$

### CDF
$$X \sim N(\mu, \sigma^2) \implies F_X(x) = \frac{1}{\sqrt{2\pi}} \int_{-\infty}^{x}e^{-\dfrac{1}{2}\left( \dfrac{t-\mu}{\sigma} \right)^2} \,dt$$


### PDF
$$X \sim N(\mu, \sigma^2) \implies f_X(x) = \frac{1}{\sigma\sqrt{2\pi}}e^{- \dfrac{1}{2}\left( \dfrac{x-\mu}{\sigma}\right)^2}$$

### MGF
$$X \sim N(\mu, \sigma^2) \implies M_X(t) = \exp{\left( \mu t + \sigma^2 \frac{t^2}{2} \right)}$$

## Normale Standard
Una **normale standard** è un normale con parametri $\mu = 0$ e $\sigma^2 = 1$, ovvero $N(0,1)$.
Avremo quindi
- Supporto $\mathbb{R}$
- Media $\mu = 0$
- Varianza $\sigma^2 = 1$
- cdf $$\Phi(x) = \frac{1}{\sqrt{2\pi}} \int_{-\infty}^{x}e^{-t^2/2} \,dt$$
- pdf $$\phi(x) = \frac{1}{\sqrt{2\pi}}e^{-x^2/2}$$
- mgf $M_X(t) = e^{t^2/2}$

Osserviamo che $X \sim N(\mu, \sigma^2)$ avra *pdf* $$f_X(x) = \frac{1}{\sigma} \cdot \phi\left( \frac{x - \mu}{\sigma} \right)$$ e *cdf* $$f_X(x) = \Phi\left( \frac{x - \mu}{\sigma} \right)$$

## Proprietà importanti
Sia $X \sim N(\mu, \sigma^2)$
1. La v.a. $aX + b$ equivlae a una normale $N(a\mu + b, |a|\sigma)$.
2. Caso particolare del predente: sia $Z \sim N(0, 1)$. Allora la v.a. $\sigma^2 Z + \mu$ è una normale $N(\mu, \sigma^2)$. 
3. $e^X \sim \ln{(N(\mu, \sigma^2))}$.
4. La v.a. $\vert X - \mu \vert / \sigma$ avrà distribuzione **chi** con 1 grado di libertà: $\vert X - \mu \vert / \sigma \sim \chi_1$.
5. La v.a. $(X/\sigma)^2$ ha distribuzione **chi quadro non centrata** con $1$ grado di libertà: $$\left( \frac{X}{\sigma}\right)^2 \sim \chi_1^2\left( \frac{\mu^2}{\sigma^2} \right)$$
   Nel caso in cui $\mu = 0$ avremo semplicemente una **chi quadro** $\chi_1^2$
6. La [[Random Sample#Media campionaria|media campionaria]] $\overline{X}_n$ di $n$ normali **indipendenti** ha una distribuzione $N(\mu, \sigma^2/n)$. ^50a917
7. La sommma di $n$ normali **indipendenti** ha distribuzione $N(n\mu, n\sigma^2)$.


### Bin(n,p) $\to$ N(np, np(1-p))
Sia $X \sim \text{Bin}(n,p)$ con media $\mu = np$ e varianza $\sigma^2 = np(1-p)$.
Possiamo approssimare $X$ con una normale $Y \sim N(np, np(1-p))$.

![|400](isti-distr-1.png)




----------------------
# Cauchy
### Notazione
$$\text{Cauchy}(x_0, \gamma)$$

## PDF
$$ f_{(x_0, \gamma)}(x) = \frac{1}{\pi} \frac{\gamma}{(x- x_0)^2 + \gamma^2}$$
oppure moltiplicando per $\gamma^2/\gamma^2$
$$ f_{(x_0, \gamma)}(x) = \frac{1}{\gamma\pi} \frac{1}{\left(\cfrac{x- x_0}{\gamma}\right)^2 + 1}$$

### Media e Varianza
La media di $X \sim \text{Cauchy}(x_0, \gamma)$ è $\mu = x_0$.
$X$ non ha varianza.

#### Casi particolari
$$X \sim \text{Cauchy}(0, 1) \implies f_X(x) = \frac{1}{\pi}\frac{1}{x^2 + 1}$$

$$X \sim \text{Cauchy}(0, \gamma) \implies f_X(x) = \frac{1}{\gamma\pi}\frac{1}{(x/\gamma)^2 + 1}$$
## Somma di Cauchy indipendinti
Siano $X \sim \text{Cauchy}(0, \sigma)$ e $Y \sim \text{Cauchy}(0, \tau)$ **indipendenti**, ovvero tali che
$$f_X(x) = \frac{1}{\sigma\pi}\frac{1}{(x/\sigma)^2 + 1}$$
$$f_Y(y) = \frac{1}{\tau\pi}\frac{1}{(y/\tau)^2 + 1}$$
$$f_{X,Y}(x,y) = f_X(x)f_Y(y)$$

Sia la v.a. $Z = X+Y$.
Grazie al [[Random Sample#Teorema 5 2 9 - Convolution formula|teorema 5.2.9]] possiamo dire che $Z$ avrà pdf
$$f_Z(z) = \int_{-\infty}^{\infty}f_X(w)f_Y(z-w) \, dw = \int_{-\infty}^{\infty} \frac{1}{\sigma\pi}\frac{1}{(w/\sigma)^2 + 1}\frac{1}{\tau\pi}\frac{1}{((z-w)/\tau)^2 + 1}$$
Integrando accuratamente per *decomposizioni parziali* e qualche *antidifferenziazione* otteremo che $$f_Z(z) = \frac{1}{(\sigma + \tau)\pi}\frac{1}{\left(\cfrac{z}{\sigma+\tau}\right)^2 + 1}$$
ovvero $Z$ è una $\text{Cauchy}(0, \sigma + \tau)$.

----------------------------

# Esponenziale
## Notazione
$$\text{Exp}(\lambda)$$

## Parametri
$$\lambda > 0$$
## Supporto
$$\mathbb{R}^+$$

## Funzione di ripartizione
$$F(x) = 1 - e^{-\lambda x}$$

## Funzione di densità
$$f(x) = \lambda e^{-\lambda x}$$

## Media
$$\frac{1}{\lambda}$$

## Varianza
$$\frac{1}{\lambda^2}$$

## Funzione generatrice dei momenti
$$\left(1 - \frac{t}{\lambda}\right)^{-1}$$

--------------------
# Poisson
## Notazione
$$\text{Poisson}(\lambda)$$

## Parametri
$$\lambda > 0$$
## Supporto
$$\mathbb{N}^+$$

## Funzione di ripartizione
$$F(x) = \frac{\Gamma(n+1, \lambda)}{x!}$$

## Funzione di densità
$$f(x) = \frac{\lambda^x}{x!} e^{-\lambda}$$

## Media
$$\lambda$$

## Varianza
$$\lambda$$

## Funzione generatrice dei momenti
$$e^{\lambda(e^t - 1)}$$

## Proprietà
Siano $Y_1 \sim \text{Poisson}(\lambda_1)$ e $Y_2 \sim \text{Poisson}(\lambda_2)$ **indipendenti**.
Allora avremo che
- $Y = Y_1 + Y_2$ è una poisson di parametro $\lambda = \lambda_1 + \lambda_2$.
- La distribuzione di $Y_1$ **condizionata** al fatto che $Y = n$ è una **binomiale** di parametri $n$ e $\lambda_1/\lambda$.


-----------------------
# Chi Quadro
La **distribuzione chi quadrato** è la distribuzione della somma dei quadrati di variabili aleatorie normali **indeipendenti**.
## Notazione
$$\chi^2_{k}$$

## Parametri
- **Gradi di lebertà** $$k \in \mathbb{N} \setminus \{ 0 \}$$
## Supporto
$$\left[ 0, \infty \right]$$

## Funzione di ripartizione
$$F(x) = \frac{1}{\Gamma\left( \frac{k}{2} \right)} \cdot \gamma\left( \frac{k}{2}, \frac{x}{2} \right)$$ dove $$\gamma(s,x) = \int_{0}^{x}t^{s-1}e^{-t} \, dt$$

## Funzione di densità
$$f(x) = \frac{1}{ 2^{k/2} \cdot \Gamma\left( \frac{k}{2} \right)} x^{k/2 - 1} e^{-x/2}$$

## Media
$$k$$

## Varianza
$$2k$$

## Funzione generatrice dei momenti
$$M_X(t) = (1 - 2t)^{}-k/2 \;\; \forall t \in \left[ -\frac{1}{2}, \frac{1}{2} \right]$$

## Proprietà
- La somma di due v.a. $\chi^2_n, \chi^2_m$ è una $\chi^2_{n+m}$
- Sia $X_1, ..., X_n$ un campione di normali $N(\mu, \sigma^2)$. Allora lo stimatore $S^2$ segue una distribuzione $$S^2 \sim \frac{\sigma^2}{n-1}\chi^2_{n-1} \iff (n-1)\frac{S^2}{\sigma^2} \sim \chi^2_{n-1}$$