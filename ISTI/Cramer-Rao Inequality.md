# Cramér-Rao Inequality
Sia un [[Random Sample#Random Sample|campione]] $X_1, ..., X_n$ con [[Distribuzioni Multivariate#Joint PDF|densità congiunta]] $f(\mathbf{x} \vert \theta)$.
Sia $W(\mathbf{X})$ una **statistica** che rispetta le seguenti proprietà
1. $$\frac{d}{d\theta}\mathbb{E}_{\theta}\left[ W(\mathbf{X}) \right] = \int_{\mathbb{R}^n} \frac{\partial}{\partial \theta}\left[ W(\mathbf{x}) \cdot f(\mathbf{x} \vert \theta) \right] \, d\mathbf{x}$$
2. $$\text{Var}_{\theta}(W(\mathbf{X})) < \infty$$
Allora avremo che $$\text{Var}_{\theta}(W(\mathbf{X})) \geq \frac{\left(\frac{d}{d\theta}\mathbb{E}_{\theta}\left[ W(\mathbf{X}) \right]\right)^2}{\mathbb{E}_{\theta}\left[ (\frac{\partial}{\partial\theta} \log{f(\mathbf{X} \vert \theta)})^2 \right]} = \frac{\left(\frac{d}{d\theta}\mathbb{E}_{\theta}\left[ W(\mathbf{X}) \right]\right)^2}{I(\theta)}$$ dove $I(\theta)$ è l'[[Informazione di Fisher|informazione di Fisher]] del parametro $\theta$.

Ricordiamo che se $W$ è uno [[Stimatori Puntuali#^401481|stimatore non distorto]] di $\theta$ allora avremo che $$\mathbb{E}_{\theta}\left[ W(\mathbf{X}) \right] = \theta \implies \frac{d}{d\theta}\mathbb{E}_{\theta}\left[ W(\mathbf{X}) \right] = 1$$ e quindi la disuguaglianza diventa $$\text{Var}_{\theta}(W(\mathbf{X})) \geq \frac{1}{I(\theta)}$$ ^c8c14d

------------
## Esempio - Esponenziale
Consideriamo il campione [[Distribuzioni#Esponenziale|esponenziale]] $X_1, ..., X_n$.
La distribuzione congiunta è $$f(\mathbf{x} \vert \lambda) = \lambda^ne^{- \lambda\sum_i x_i}$$
Da [[Informazione di Fisher#Esempio - Campione esponenziale]] sappiamo che la sua informazione di Fisher è $$I(\lambda) = \frac{n}{\lambda^2}$$

Sia quindi la [[Random Sample#Media campionaria|media campionaria]] $\overline{X} = (X_1 + ... + X_n)/n$ uno [[Stimatori Puntuali#Stimatori puntuali|stimatore puntuale]] della media di una v.a. esponenziale, ovvero $1/\lambda$.
Si può verificare che $\overline{X}$ è [[Stimatori Puntuali#^401481|non distorto]], infatti $$\mathbb{E}_\lambda\left[ \overline{X} \right] = \frac{1}{n} \sum_{i=1}^{n}\mathbb{E}_\lambda\left[ X_i \right] = \frac{1}{\lambda}$$
Perciò applicando la [[#^c8c14d|disuguaglianza di Cramér-Rao]] avremo che $$\text{Var}_\lambda(\overline{X}) \geq \frac{1}{I(\theta)} = \frac{\lambda^2}{n}$$ 

Proviamo ora a considerare lo stimatore $$\hat{\lambda} = \frac{1}{\overline{X}}$$ per il parametro $\lambda$.

Dalle [[Distribuzioni#^50a917|proprietà delle normali]] avremo che la distribuzione di $\overline{X}$ tende **asintoticamente** a 
$$\overline{X} \xrightarrow{d} N(\mu, \sigma^2/n) = N\left(\frac{1}{\lambda}, \frac{1}{n\lambda^2} \right)$$

Applichiamo quindi il [[Delta Method#Teorema - Metodo Delta|Metodo Delta]] con funzione $$g(x) = \frac{1}{x}$$ ottenendo quindi $$\sqrt{n}\left( \hat{\lambda} - \lambda\right) = \sqrt{n}\left(g(1/\overline{X}) - g(1/\lambda)\right) \xrightarrow{d} N(0, \hat{\sigma}^2 \left[ g'(1/\lambda) \right]^2) = N\left(0, \frac{\lambda^2}{n} \right)$$
Ovvero $\hat{\lambda}$ [[Convergenza#Convergenza in Distribuzione|converge in distribuzione]] a $N(\lambda, \lambda^2/n)$, perciò $\hat{\lambda}$ è uno stiamtore **asintoticamente** non distorto per $\lambda$, ovvero $$\lim_{n \to \infty} \mathbb{E}\left[ \hat\lambda \right] = \lambda$$
Analogamente per la sua *varianza asintotica* $$\lim_{n \to \infty} \text{Var}\left( \hat\lambda \right) = \frac{\lambda^2}{n} = \frac{1}{I(\lambda)}$$ dunque la più piccola varianza che si può ottenere.

Possiamo quindi concludere che $\hat\lambda$ è **asintoticamente ottimale**.

(Osservare che avvolte valore atteso e varianza di uno stimatore non sono sempre ottimali per $n$ finito).