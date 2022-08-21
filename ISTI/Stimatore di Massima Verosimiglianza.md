# Stimatore di Massima Verosimiglianza - MLE
Sia un [[Random Sample#Random Sample|campionamento]] $\mathbf{X} = (X_1, ..., X_n)$.
Per ogni possibile valore osservabile $\mathbf{x}$, sia $\hat{\theta}(\mathbf{x})$ un **parametro** che **massimizza** la [[Verosimiglianza#Likelihood function|funzione di massima verosimiglianza]] $L(\theta \vert \mathbf{x})$ (notare $\mathbf{x}$ fissato), ovvero
$$\hat{\theta}(\mathbf{x}) = \arg \max_{\theta \in \Theta} L(\theta \vert \mathbf{x})$$

Allora uno **stimatore di massima verosimiglianza** (o **MLE - maximum likelihood estimator**) del parametro $\theta$ basato sul campione $\mathbf{X}$ è $\hat{\theta}(\mathbf{X})$.

Dato che nel caso multivariato $L$ è uguale al prodotto delle singole densità, conviene calcolare il massimo della **log-verosimiglianza**, ovvero il logaritmo di $L(\theta \vert \mathbf{x})$.
Per via della **monotonia** del logaritmo, un punto di massimo per $L$ equivale a un punto di massimo anche per $\ln{L}$.

Perciò *derivando* in $\theta$ la *log-verosimiglianza* e ponendola pari a $0$ troveremo un potenziale punto di massimo.
Per assicurarci che tale punto è di massimo e non di minimo, servirà fare la seconda derivata e virificare che sia **minore** di $0$.

Ci riferiremo alla derivata della log-verosimiglianza con **score function** $$S(\theta, \mathbf{X}) = \frac{d}{d \theta}\ln{L(\theta \vert \mathbf{X})} = \sum_{i=1}^{n} \frac{d}{d \theta}\ln{f(x_i \vert \theta)}$$ ^03be6d

## Esempio - Normale
Sia un campionamento $X_1, ..., X_n$ di $N(\theta,1)$.
Avremo quindi che la verosimiglianza sarà pari a $$L(\theta \vert \mathbf{x}) = \prod_{i=1}^{n}\frac{1}{\sqrt{2\pi}}e^{-\frac{1}{2}(x_i - \theta)^2} = (2\pi)^{-n/2}e^{-\frac{1}{2}\sum_i (x_i - \theta)^2}$$ 
Mentre la *log-verosimiglianza* sarà $$\ln{(L(\theta \vert \mathbf{x}))} = -\frac{1}{2}\sum_{i=1}^{n}(x_i - \theta)^2 - \frac{n}{2}\ln{(2\pi)}$$

Per calcolare il $\theta$ che massimizza tale quantità inizamo col calcolare la derivata prima $$\frac{d}{d\theta} \ln{(L(\theta \vert \mathbf{x}))} = - \frac{1}{2} \sum_{i=1}^{n}2(x_i - \theta)(-1)= \sum_{i=1}^{n}(x_i - \theta) = n \overline{x} - n \theta$$
Tale derivata è pari a $0$ per $\theta = \overline{x}$.

Ora bisogna capire se tale valore massimizza oppure minimizza la verosimiglianza (ovvero se è un punto di *massimo* o di *minimo*).
Calcoliamo quindi la derivata seconda $$\frac{d^2}{d\theta^2} \ln{(L(\theta \vert \mathbf{x}))} \Big\vert_{\theta = \overline{x}} = -n\Big\vert_{\theta = \overline{x}} = -n < 0$$
Perciò $\theta = \overline{x}$ è un punto di massimo (quantomeno locale).

Facendo il limite per $\theta \to \pm \infty$ si ha che la *log-verosimiglianza* tende a $-\infty$, perciò $\theta = \overline{x}$ è l'unico punto di massimo.

In conclusione $\hat{\theta} = \overline{X}$ è uno **stimatore di massima verosimiglianza**.

## Esempio - Bernoulli
Sia il campionamento $X_1, ..., X_n$ di bernoulliane di parametro sconosciuto $p = \theta$.
Perciò la verosimiglianza risulta essere $$L(\theta \vert \mathbf{x}) = \prod_{i=1}^{n} \theta^{x_i}(1-\theta)^{1 - x_i} = \theta^{s}(1-\theta)^{n-s}$$ dove $s = x_1 + ... + x_n$.

La *log-verosimiglianza* risulta invece $$\ln{(L(\theta \vert \mathbf{x}))} = s\log(\theta) + (n-s)\log(1-\theta)$$ con derivarte $$\frac{d}{d\theta} \ln{(L(\theta \vert \mathbf{x}))} = \frac{s}{\theta} - \frac{n-s}{1 - \theta}$$ e $$\frac{d^2}{d\theta^2} \ln{(L(\theta \vert \mathbf{x}))} = -\frac{s}{\theta^2} - \frac{n-s}{(1 - \theta)^2}$$

La derivata prima è nulla quando $$\begin{align*}
\frac{s}{\theta} - \frac{n-s}{1 - \theta} &= 0\\
(1-\theta)s - \theta(n-s) &= 0\\
s - \cancel{s\theta} - n\theta + \cancel{s\theta} &= 0\\
\theta &= \frac{s}{n}
\end{align*}$$ ovvero quando $\theta = \overline{x}$
 
È possibile verificare che tale punto è anche un punto di massimo **globale**, perciò avremo che $\hat{p} = \overline{X}$ è uno stimatore di massima verosimiglianza.

------------------------
# Proprietà di un MLE

## Invarianza di uno MLE
Sia $\hat{\theta}$ uno **stimatore di massima verosimiglianza** di un parametro $\theta$.
Allora per ogni funzione $\tau(\theta)$ avremo che $\tau(\hat{\theta})$ è uno stimatore di massima verosimiglianza per $\tau(\theta)$.

## Proprietà asintotiche (molto importanti)
Gli stimatori di massima verosimiglianza possono essere distorti ma, sotto condizioni abbastanza generali, sono però **consistenti** e, per $n$ che tende ad infinito, risultano **asintoticamente corretti** e **massimamente efficienti**.
Inoltre, sempre per $n$ che tende ad infinito, la loro distribuzione tende ad una normale.

In termini tecnici, uno MLE $\hat\theta_{ML}$ per un parametro $\theta$ è:
1. **Asintoticamente corretto (non distorto)**: anche se $\hat\theta_{ML}$ risulta **distorto**, asintoticamente avremo sempre che $$\lim_{n \to \infty} \mathbb{E}\left[ \hat\theta_{ML} \right] = \theta$$
2. **Consistente**: ovvero che $\hat\theta_{ML}$ **[[Convergenza#Convergenza in probabilità|converge in probabilità]]** a $\theta$ $$\hat{\theta}_{ML} \xrightarrow{p} \theta$$
3. **Asintoticamente efficiente**: la varianza di $\hat{\theta}_{ML}$ tende asintoticamente la suo **[[Cramér-Rao Inequality#Cramér-Rao Inequality|limite inferiore]]**, ed essendo $\hat{\theta}_{ML}$ asintoticamente non distorto avremo che $$\lim_{n \to \infty} \text{Var}(\hat\theta_{ML}) = \frac{1}{I(\theta)}$$
4. **Asintoticamente normale**: per $n \to \infty$ avremo che $$\hat\theta_{ML} \sim N\left(\theta, \frac{1}{I(\theta)}\right)$$ oppure normalizzando $$\sqrt{n}(\hat\theta_{ML} - \theta) \approx \frac{S(\theta \vert \mathbf{X})}{nI(\theta)} \sim N\left(0, \frac{1}{I(\theta)}\right)$$ ^c3b404
