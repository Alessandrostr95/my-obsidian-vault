# Normale
### Notazione
$$N(\mu, \sigma^2)$$
### Parametri
- **media** $\mu$
- **varianza** $\sigma^2$

### PDF
$$X \sim N(\mu, \sigma^2) \implies f_X(x) = \frac{1}{\sigma\sqrt{2\pi}}e^{- \dfrac{1}{2}\left( \dfrac{x-\mu}{\sigma}\right)^2}$$

### MGF
$$X \sim N(\mu, \sigma^2) \implies M_X(t) = \exp{\left( \mu t + \sigma^2 \frac{t^2}{2} \right)}$$

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