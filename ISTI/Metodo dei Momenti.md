# Metodo dei Momenti
Sia il [[Random Sample#Random Sample|campione]] $X_1, ..., X_n$ di una popolazione con *pdf* o *pmf* $f(x \vert \theta_1, ..., \theta_k)$, ovvero dipendente da certi parametri (sconosciuti) $\theta_1, ..., \theta_k$.

Il **metodo dei momenti** consente nell'eguagliare i primi $k$ momenti con i primi $k$ **momenti campionari**.

Il **momento campionario** $j$-esimo di un campione $X_1, ..., X_n$ è definito come $$M_j = \frac{1}{n}\sum_{i=1}^{n}X_i^j$$

Dato che invece il momento (semplice) $j$-esimo $\mu_j$ dipende dai parametri $(\theta_1, ..., \theta_k)$, scriveremo $$\mu_j = \mu_j(\theta_1, ...,\theta_k) = \mathbb{E}_{\theta_1, ..., \theta_k} \left[ X^j \right]$$

Otterremo così un sistema di $k$ equazioni a $k$ incognite
$$\begin{cases}
\mu_1(\theta_1, ..., \theta_k) &= m_1\\
\mu_2(\theta_1, ..., \theta_k) &= m_2\\
&\vdots\\
\mu_k(\theta_1, ..., \theta_k) &= m_k\\
\end{cases}$$

Risolvendo questo sistema (se possibile) troveremo una prima stima $(\hat{\theta}_1, ..., \hat{\theta}_k)$ dei parametri $(\theta_1, ..., \theta_k)$.

## Esempio - Normale
Sia un *campione normale* $X_1, ..., X_n$ iid in $N(\mu, \sigma^2)$, con $\mu, \sigma^2$ i parametri sconosciuti.

Poniamo quindi $(\theta_1, \theta_2) = (\mu, \sigma^2)$, e applichiamo il metodo dei momenti per $k=2$.

Avremo quindi
$$\begin{align*}
\mu_1 = \mathbb{E}\left[ X_i \right] = \mu, &&M_1 = \frac{1}{n}\sum_{i=1}^{n}X_i = \overline{X}\\
\mu_2 = \mathbb{E}\left[ X_i \right] = \mu^2 + \sigma^2, &&M_2 = \frac{1}{n}\sum_{i=1}^{n}X_i^2
\end{align*}$$
Perciò avremo
$$\begin{cases}
\theta_1 = \overline{X}\\
\theta_1^2 + \theta_2 = \frac{1}{n}\sum_{i=1}^{n}X_i^2
\end{cases}$$
$$\begin{align*}
\implies\theta_2
&= \frac{1}{n}\sum_{i=1}^{n}X_i^2 - \overline{X}^2\\
&= \frac{1}{n}\left[\sum_{i=1}^{n}X_i^2 - n\overline{X}^2\right]\\
&= \frac{1}{n}\left[\sum_{i=1}^{n}(X_i - \overline{X})^2\right]\\
&= \frac{n-1}{n}S^2
\end{align*}$$
(Per quest'utlima equazione vedere [[Random Sample#Teorema 5 2 4]]).

In conclusione i nostri stimatori saranno
$$\begin{align*}
\hat{\mu} &= \overline{X}\\
\hat{\sigma}^2 &= \frac{n-1}{n}S^2
\end{align*}$$

## Esempio - Binomiale
Sia il campione $X_1, ..., X_n$ di binomiali $\text{Bin}(N, p)$, ovvero tali che $$P(X_i = h \vert N,p) = \binom{N}{h}p^{h}(1-p)^{n-h}\;\; \forall h = 0,1,...,N$$

In questo caso i parametri che vogliamo stimare sono $(\hat{N}, \hat{p}) = (N, p)$, perciò poniamo il sistema
$$\begin{cases}
\hat{N}\hat{p} &= \overline{X}\\
\hat{N}\hat{p}(1-\hat{p}) &= \frac{1}{n}\sum_{i=1}^{n}X_i^2\\
\end{cases}
\; \longrightarrow \;
\begin{cases}
\hat{p} &= \overline{X}/\hat{N}\\
\overline{X}(1-\overline{X}/\hat{N}) &= \frac{1}{n}\sum_{i=1}^{n}X_i^2\\
\end{cases}$$
$$\longrightarrow \;
\begin{cases}
\hat{p} &= \overline{X}/\hat{N}\\
1-\overline{X}/\hat{N} &= \dfrac{\sum_{i=1}^{n}X_i^2/n}{\overline{X}}
\end{cases}
\; \longrightarrow \;
\overline{X}/\hat{N} = 1 - \dfrac{\sum_{i=1}^{n}X_i^2/n}{\overline{X}} = \dfrac{\overline{X} - \sum_{i=1}^{n}X_i^2/n}{\overline{X}}
$$
$$\; \longrightarrow \;
\dfrac{\hat{N}}{\overline{X}} = \dfrac{\overline{X}}{\overline{X} - \sum_{i=1}^{n}X_i^2/n}
$$
$$\implies
\begin{align*}
\overline{X} &= \dfrac{\overline{X}^2}{\overline{X} - \sum_{i=1}^{n}X_i^2/n}\\
\hat{p} &= \dfrac{\overline{X}}{\hat{N}}
\end{align*}$$

