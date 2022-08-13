# Informazione di Fisher
Sia $\mathbf{X}$ un [[Random Sample#Random Sample|campione aleatorio]] dipendente da un parametro (sconosciuto) $\theta$.
L'**informazione di Fisher** è una funzione definita in due modi
1. $$I(\theta) = \mathbb{E}_\theta \left[ \left(\frac{\partial}{\partial \theta}\ln{f(\mathbf{X} \vert \theta)} \right)^2 \right] = \mathbb{E}_\theta \left[ \left(\frac{\partial}{\partial \theta}\ln{L(\theta \vert \mathbf{X})} \right)^2 \right]$$
2. $$I(\theta) = -\mathbb{E}_\theta \left[ \frac{\partial^2}{\partial \theta^2}\ln{f(\mathbf{X} \vert \theta)} \right] = -\mathbb{E}_\theta \left[ \frac{\partial^2}{\partial \theta^2}\ln{L(\theta \vert \mathbf{X})} \right]$$

## Esempio - Campione esponenziale
Sia $\mathbf{X}$ un [[Random Sample#Random Sample|campione]] [[Distribuzioni#Esponenziale|esponenziale]] di parametro $\lambda$.
La [[Verosimiglianza#Likelihood function|verosimiglianza]] risulta quindi essere $$L(\lambda \vert \mathbf{x}) = f_{\mathbf{X}}(\mathbf{x} \vert \theta) = \prod_{i=1}^{n}\lambda e^{-\lambda x_i} = \lambda^n e^{-\lambda\sum_i x_i}$$
Il suo logaritmo risulta invece $$\ln{L(\lambda \vert \mathbf{x})} = -\lambda\sum_{i=1}^{n}x_i + n\ln{\lambda}$$ con derivate
$$\begin{align*}
\frac{d}{d\lambda} \ln{L(\lambda \vert \mathbf{x})} &= \frac{n}{\lambda} - \sum_{i=1}^{n}x_i\\
\frac{d^2}{d\lambda^2} \ln{L(\lambda \vert \mathbf{x})} &= -\frac{n}{\lambda^2}\\
\end{align*}$$

Perciò (dato che la derivata seconda non dipende più dal campione $\mathbf{X}$) l'informazione di Fisher sarà $$I(\lambda) = \frac{n}{\lambda^2}$$
# Proprietà
## Adattività
L'informazione di Fisher è **adattiva** rispetto a due campioni $\mathbf{X}, \mathbf{Y}$ **indipendenti**.
$$I_{\mathbf{X}, \mathbf{Y}}(\theta) = I_{\mathbf{X}}(\theta) + I_{\mathbf{Y}}(\theta)$$

Ne segue quindi che l'informazione di un campione $X_1, ..., X_n$ di v.a. **i.i.d.** è pari a $n$ volte l'informazione di Fisher di un generico $X_i$.
$$I_{X_1, ..., X_n}(\theta) = n \cdot I_{X_1}(\theta)$$

## Implicazioni sulla Sufficienza di una statistica
Richiamando il [[Statistiche Sufficienti#Teorema di Fattorizzazione|Teorema di Fattorizzazione]] avremo che data una **statistica sufficiente** $T$ per un parametro $\theta$
$$\begin{align*}
\frac{\partial}{\partial \theta}\ln{L(\theta \vert \mathbf{X})}
&= \frac{\partial}{\partial \theta}\ln{f(\mathbf{X} \vert \theta)}\\
&= \frac{\partial}{\partial \theta}\ln{(g(T(\mathbf{X}) \vert \theta) \cdot h(\mathbf{X}))}\\
&= \frac{\partial}{\partial \theta}\ln{g(T(\mathbf{X}) \vert \theta)}\\
&= \frac{\partial}{\partial \theta}\ln{L(\theta \vert T(\mathbf{X}))}
\end{align*}$$

Perciò per statistiche sufficienti $T$ si ha che $$I_{\mathbf{X}}(\theta) = I_{T(\mathbf{X})}(\theta)$$

Invece per statistiche $T$ *in generale* vale che $$I_{\mathbf{X}}(\theta) \geq I_{T(\mathbf{X})}(\theta)$$
## Implicazioni sulla Efficienza di una statistica
La [[Disuguaglianza di Cramér-Rao]] stabilisce una correlazione tra **informazione di Fisher** e **varianza di uno stimatore non distorto**.
Più precisamente ricordiamo che dato uno stimatore non distorto $\hat{\theta}$ del parametro $\theta$ vale che $$\text{Var}(\hat{\theta}) \geq \frac{1}{I(\theta)}$$

----------------------------
## Esempio - Bernoulliane
Sia il [[Random Sample#Random Sample|campione]] $X_1, ..., X_n$ di $\text{Bernoulli}(\theta)$, con [[Distribuzioni Multivariate#Joint PMF|probabilità congiunta]]
$$f_{\mathbf{X}}(\mathbf{x} \vert \theta) = \prod_{i=1}^{n}\theta^{x_i}(1-\theta)^{1 - x_i} = \theta^s(1-\theta)^{n-s}$$ dove $s = \sum_i x_i$.

Per [[#Adattività|adattività]] avremo che
$$\begin{align*}
I_{\mathbf{X}}(\theta)
&= n \cdot I_{X_1}(\theta)\\
&= -n \cdot \mathbb{E}_{\theta}\left[ \frac{\partial^2}{\partial \theta^2}\ln{L(\theta \vert X_1)} \right]\\
&= -n \cdot \mathbb{E}_{\theta}\left[ \frac{\partial^2}{\partial \theta^2}\ln{\left( \theta^{X_1}(1-\theta)^{1-X_1} \right)} \right]\\
&= -n \cdot \mathbb{E}_{\theta}\left[ -\frac{X_1}{\theta^2} - \frac{(1-X_1)}{(1 - \theta)^2} \right]\\
&= -n\cdot \left( - \frac{1}{\theta} - \frac{1}{1-\theta} \right)\\
&= n \cdot \frac{1}{\theta(1-\theta)}
\end{align*}$$