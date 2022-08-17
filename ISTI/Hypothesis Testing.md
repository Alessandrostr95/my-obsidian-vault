# Hypothesis Testing
A fronte di un [[Random Sample#Random Sample|campionamento]] $\mathbf{X}$ da una popolazione $f(\mathbf{X} \vert \theta)$, un **test d'ipotesi** cerca di **decidere** se il campionamento $\mathbf{X}$ è compatibile rispetto ad una data **ipotesi** riguardante il parametro **sconosciuto** $\theta$.

Dato che $\theta$ appartiene ad uno *spazio di parametri* $\Theta$, un'ipotesi è semplicemente un'asserzione del tipo $$\theta \in \Theta_0 \subset \Theta$$ detta anche **ipotesi nulla** e segnata con $H_0$.

Rifiutare l'ipotesi nulla equivale semplicemente nell'accettare il suo complemento, chiameremo **ipotesi alternativa** e la indicheremo con $H_A$ o $H_1$, e sono del tipo $$\theta \in \Theta_0^c \equiv \Theta \setminus \Theta_0$$

Un test d'ipotesi è quindi semplicemente una **regola** che, per ogni possibile campione $\mathbf{X}$, ci dice se **rifiutare o no** l'ipotesi nulla $H_0$.
> **Esempio**
> Prendiamo un campione $X_1,...,X_n$ di una popolazione $N(\theta, \sigma^2)$, con $\theta$ sconosciuto.
> Consideriamo una ipotesi $$H_0: \theta \leq 17$$
> Un test famoso consiste nel <u>rifutare</u> $H_0$ se e solo se $\overline{X} > 17 + \sigma/\sqrt{n}$.

Il sottoinsieme $C$ dei dati che portano al **rifuto** di $H_0$ è detta **regione critica**.
> Nell'esempio precedente, la regione critca equivale al sottoinsieme di $\mathbb{R}^n$ di tutti quei $n$-uple $\mathbf{x}$ tali che $$\frac{1}{n}\sum_{i=1}^{n}x_i > 17 + \frac{\sigma}{\sqrt{n}}$$

La probabilità quindi che $H_0$ sia rifiutata equivale quindi alla probabilità di campionare all'interno della regione critica, e tale probabilità è nota come **funzione critica** $$\psi(\mathbf{X}) = P(\text{rifiuto } H_0) = P(\mathbf{X} \in C)$$
