Nella **Gaussian Discriminant Analysis** (**GDA**) si assume che ogni probabilità $p(\mathbf{x} \vert C_k)$ degli elementi $\mathbf{x} \in \mathbb{R}^d$ rispetto alla classe $C_k$ sia una [[CLT - Central Limit Theorem#Normali Multivariate|gaussiana multivariata]].

Sia quindi $\mu_k$ il punto medio della classe $C_k$ e $\Sigma \in \mathbb{R}^{d \times d}$ la **matrice di covarianza**, allora la probabilità di $\mathbf{x} \vert C_k$ avrà *densità* $$p(\mathbf{x} \vert C_k) = \frac{1}{(2\pi)^{d/2}\vert \text{det}(\Sigma) \vert^{1/2}} \exp{\left(-\frac{1}{2}(\mathbf{x} - \mu_k)^T\Sigma^{-1}(\mathbf{x} - \mu_k)\right)}$$ ^cfba72

Ricordiamo che per il [[Naive Bayes Classifier|caso binario]] abbiamo che $$p(C_1 \vert \mathbf{x}) = \sigma(a(\mathbf{x}))$$ con
$$\begin{align}
a(\mathbf{x})
&= \log\frac{p(\mathbf{x} \vert C_1)p(C_1)}{p(\mathbf{x} \vert C_2)p(C_2)}\\
&= \log\frac{p(\mathbf{x} \vert C_1)}{p(\mathbf{x} \vert C_2)} + \log\frac{p(C_1)}{p(C_2)}\\
&= \frac{1}{2}(\mathbf{x} - \mu_2)^T\Sigma^{-1}(\mathbf{x} - \mu_2) -\frac{1}{2}(\mathbf{x} - \mu_1)^T\Sigma^{-1}(\mathbf{x} - \mu_1) + \log\frac{p(C_1)}{p(C_2)}\\
&= \frac{1}{2}(\mathbf{x}^T\Sigma^{-1}\mathbf{x} - \mathbf{x}^T\Sigma^{-1}\mu_2 - \mu_2^T\Sigma^{-1}\mathbf{x} + \mu_2^T\Sigma^{-1}\mu_2) -\frac{1}{2}(\mathbf{x}^T\Sigma^{-1}\mathbf{x} - \mathbf{x}^T\Sigma^{-1}\mu_1 - \mu_1^T\Sigma^{-1}\mathbf{x} + \mu_1^T\Sigma^{-1}\mu_1) + \log\frac{p(C_1)}{p(C_2)}\\
\end{align}$$

^3af7ff

Osserviamo che $$\mathbf{x}^T\Sigma^{-1}\mu_i = \mu_i^T\Sigma^{-1}\mathbf{x}$$

Andando a semplificare l'[[#^3af7ff|equazione]]
$$\begin{align}
a(\mathbf{x})
&= \cdots\\
&= (\mathbf{x}^T\Sigma^{-1}\mu_1 - \mathbf{x}^T\Sigma^{-1}\mu_2) +\frac{1}{2}(\mu_2^T\Sigma^{-1}\mu_2 - \mu_1^T\Sigma^{-1}\mu_1) + \log{\frac{C_1}{C_2}}\\
&= (\mu_1 - \mu_2)^T\Sigma^{-1}\mathbf{x} + \frac{1}{2}(\mu_2^T\Sigma^{-1}\mu_2 - \mu_1^T\Sigma^{-1}\mu_1) + \log{\frac{C_1}{C_2}}\\
&= \mathbf{w}^T\mathbf{x} + w_0 
\end{align}$$
con
$$\begin{align}
\mathbf{w} &= \Sigma^{-1}(\mu_1 - \mu_2)\\
w_0 &= \frac{1}{2}(\mu_2^T\Sigma^{-1}\mu_2 - \mu_1^T\Sigma^{-1}\mu_1) + \log{\frac{C_1}{C_2}}
\end{align}$$

Perciò, assumendo $p(\mathbf{x} \vert C_k)$ sia gaussiana, allora la probabilità $p(C_k \vert \mathbf{x})$ sarò un [[Generalized Linear Model]].
Infatti $$p(C_k \vert \mathbf{x}) = \sigma(a(\mathbf{x})) = \sigma(\mathbf{w}^T\mathbf{x} + w_0)$$ ovvero un'applicazione **non lineare** sulla **combinazione lineare** del punto $\mathbf{x}$.

![[ML_07_2.png]] ^e433ad


Nella [[#^e433ad|figura]] a sinistra, possiamo vedere le distribuzioni di $p(\mathbf{x} \vert C_1)$ (in rosso) e $p(\mathbf{x} \vert C_2)$ (in blu), mentre a destra vediamo la distribuzione $p(C_k \vert \mathbf{x})$.

# Funzione Discriminante
La funzione discriminante, ovvero quella che **separa** una regione dall'altra, può essere ottenuta ponendo $$p(C_1 \vert \mathbf{x}) = p(C_2 \vert \mathbf{x})$$ ovvero $$\sigma(a(\mathbf{x})) = \sigma(-a(\mathbf{x}))$$

Ancora, l'uguaglianza è sempre soddisfatta per $$a(\mathbf{x}) = - a(\mathbf{x})$$ visto che $\sigma$ è **monotona**.

Perciò avremo
$$a(\mathbf{x}) = - a(\mathbf{x})\implies 2a(\mathbf{x}) = 0 \implies a(\mathbf{x})=0$$ ovvero $$\mathbf{w}^T\mathbf{x} + w_0 = 0$$
$$\implies (\mu_1 - \mu_2)^T\Sigma^{-1}\mathbf{x} + \frac{1}{2}(\mu_2^T\Sigma^{-1}\mu_2 - \mu_1^T\Sigma^{-1}\mu_1) + \log{\frac{C_1}{C_2}} = 0$$

Per esempio, quando abbiamo un caso semplice del tipo $\Sigma = \lambda I$ allora basta risolvere l'equazione
$$\lambda^{-1}(\mu_1 - \mu_2)^T\mathbf{x} + \frac{\lambda^{-1}}{2}(\Vert \mu_2 \Vert^2 - \Vert \mu_1 \Vert^2) + \log\frac{p(C_1)}{p(C_2)} = 0$$


# Multiple Classes
Consideriamo il caso [[Naive Bayes Classifier#Caso Multiclasse - Softmax Regression|multiclasse]].
In questo caso abbiamo che $$p(C_k \vert \mathbf{x}) = s(a_k(\mathbf{x}))$$ con $$a_k(\mathbf{x}) = \log(p(\mathbf{x}\vert C_k)p(C_k))$$

Come prima, se assumiamo che $p(\mathbf{x} \vert C_k)$ sia una **gaussiana** multivariata con media $\mu_k$ allora è facile derivare che
$$\begin{align}
a_k(\mathbf{x})
&= \log{\frac{1}{(2\pi)^{d/2}\vert \Sigma \vert^{1/2}}}-\frac{1}{2}(\mathbf{x} - \mu_k)^T\Sigma^{-1}(\mathbf{x}-\mu_k) + \log p(C_k)\\
&= \mu_k^{T}\Sigma^{-1}\mathbf{x} - \frac{1}{2}\mu_k^T\Sigma^{-1}\mu_k - + \frac{d}{2}\log{(2\pi)} + \frac{1}{2}\log(\Sigma) + \log p(C_k)\\
&= \mathbf{w}_k^T\mathbf{x} + w_{k0}
\end{align}$$

Perciò anche il caso multiclasse $p(C_k \vert \mathbf{x}) = \sigma(\mathbf{w}_k^T\mathbf{x} + w_{k0})$ è un [[Generalized Linear Model]].

## Funzione discrimante per Classi Multiple
Come prima, l'iperpiano che separa due classi $C_i,C_j$ è definito da tutti quei punti $\mathbf{x}$ tali che
$$\begin{align}
p(C_i \vert \mathbf{x}) &= p(C_j \vert \mathbf{x})\\
\\
\implies s(a_i(\mathbf{x})) &= s(a_j(\mathbf{x}))\\
\\
\implies \frac{e^{a_i(\mathbf{x})}}{\sum_{k=1}^{K}e^{a_k(\mathbf{x})}} &= \frac{e^{a_j(\mathbf{x})}}{\sum_{k=1}^{K}e^{a_k(\mathbf{x})}}\\
\\
\implies e^{a_i(\mathbf{x})} &= e^{a_j(\mathbf{x})}\\
\\
\implies a_i(\mathbf{x}) &=a_j(\mathbf{x})\\
\\
\implies  \mathbf{w}_i^T\mathbf{x} + w_{i0} &=  \mathbf{w}_j^T\mathbf{x} + w_{j0}
\end{align}$$

E da come si evince, tali bordi sono **lineari**.

# General Covariance Matrices
Nel caso iniziale assumevamo che entrambe le distribuzioni $\mathbf{x} \vert C_1$ e $\mathbf{x} \vert C_2$ avevano [[#^cfba72|stessa matrice di covarianza]] $\Sigma \in \mathbb{R}^{d \times d}$.

Generalizzando, assumiamo che $$\mathbf{x} \vert C_1 \sim \mathcal{N}(\mu_1 \vert \Sigma_1)$$ e che $$\mathbf{x} \vert C_2 \sim \mathcal{N}(\mu_2 \vert \Sigma_2)$$ con $\Sigma_1, \Sigma_2$ non necessariamente uguali.

In questo caso avremo $$a(\mathbf{x}) = \frac{1}{2}\left( (\mathbf{x} - \mu_2)^T\Sigma_2(\mathbf{x} - \mu_1) - (\mathbf{x} - \mu_1)^T\Sigma_1(\mathbf{x} - \mu_1) \right) + \frac{1}{2}\log\frac{\vert \Sigma_2 \vert}{\vert \Sigma_1 \vert} + \log\frac{p(C_1)}{p(C_2)}$$
Si può dimostrare che in questo caso i **decision boundary** non sono necessariamente **lineari**.

Infatti lo si può osservare dalla [[#^e350eb|figura]], nel caso multiclasse con $K = 3$.

![[ML_07_3.png]] ^e350eb

# Deriving $p(\mathbf{x} \vert C_k)$ from dataset
La probabilità $p(\mathbf{x} \vert C_k)$ può essere derivata dal dataset mediante [[Stimatore di Massima Verosimiglianza]].

Per semplicità assumiamo di avere solo $K=2$ classi.
Dato che assumiamo la probabilità a priori nota $p(C_1) = \pi$ (e $p(C_2) = 1 - \pi$) dobbiamo solo stimare $\mu_1, \mu_2, \Sigma$.

Osservando il dataset $\mathcal{T}$ avremo la verosimiglianza
$$\begin{align}
L(\pi,\mu_1,\mu_2, \Sigma \vert \mathcal{T})
&= \prod_{i=1}^{n} p(x_i,t_i \vert\pi,\mu_1,\mu_2,\Sigma)\\
&= \prod_{i=1}^{n} p(x_i \vert t_i,\pi,\mu_1,\mu_2,\Sigma) \cdot p(t_i \vert\pi)\\
&= \prod_{i=1}^{n} (\pi \cdot\mathcal{N}(x_i \vert \mu_1, \Sigma_1))^{t_i}((1-\pi) \cdot\mathcal{N}(x_i \vert \mu_2, \Sigma_2))^{1-t_i}
\end{align}$$

```ad-tldr
Dato che possiamo codificare in **binario** i valori target
$$t_i = \begin{cases}
0 &\text{if } x_i \notin C_1\\
1 &\text{if } x_i \in C_1\\
\end{cases}$$

Allora possiamo esprimere la probabilità $$p(t_i) = p(C_1)^{t_1}\cdot p(C_2)^{1-t_i}$$

Infatti,
$$x_i \in C_1 \implies p(t_i = 1) = p(C_1)^1 \cdot p(C_2)^{1-1} = p(C_1)$$
$$x_i \in C_2 \implies p(t_i = 0) = p(C_1)^0 \cdot p(C_2)^{1-0} = p(C_2)$$
```

```ad-tldr
Ossevare che $$p(x_i, t_i) = p(x_i \vert t_i) \cdot p(t_i) = (p(x_i \vert C_1)p(C_1))^{t_i} \cdot (p(x_i \vert C_2) p(C_2))^{1- t_i}$$
```

La verosimiglianza corrispondente sarà invece
$$\begin{align}
l(\pi, \mu_1, \mu_2, \Sigma \vert \mathcal{T})
&= \sum_{i=1}^{n}t_i\log(\pi \cdot\mathcal{N}(x_i \vert \mu_1, \Sigma_1)) +  \sum_{i=1}^{n}(1-t_i)\cdot\log((1-\pi) \cdot\mathcal{N}(x_i \vert \mu_2, \Sigma_2))\\
&= \sum_{i=1}^{n}t_i\log\pi + t_i\log \mathcal{N}(x_i \vert \mu_1, \Sigma_1) +  \sum_{i=1}^{n}(1-t_i)\cdot\log(1-\pi) + (1-t_i)\cdot\log\mathcal{N}(x_i \vert \mu_2, \Sigma_2)
\end{align}$$

Se deriviamo in $\pi$ possiamo trovare il punto di massimo
$$\frac{\partial}{\partial\pi}l(\pi,\mu_1,\mu_2, \Sigma \vert\mathcal{T}) = \sum_{i=1}^{n} \left( \frac{t_i}{\pi} - \frac{1-t_i}{1-\pi}\right) = \sum_{t_i=1}\frac{1}{\pi} - \sum_{t_i=0}\frac{1}{1-\pi} = \frac{\vert C_1 \vert}{\pi} - \frac{\vert C_2 \vert}{1-\pi}$$
Tale derivata si annulla per
$$\begin{align}
\vert C_1 \vert(1-\pi) &= \pi\vert C_2 \vert\\
\vert C_1 \vert &= \pi(\vert C_2 \vert + \vert C_1\vert)\\
\pi &= \frac{\vert C_1 \vert}{\vert C_1 \vert + \vert C_2 \vert} = \frac{\vert C_1 \vert}{n}
\end{align}$$
come [[Naive Bayes Classifier#^bf4983|già visto]].


Per quanto riguarda $\mu_1, \mu_2$ dobbiamo vedere quando si annulla
$$\frac{\partial l}{\partial \mu_1} = \frac{\partial }{\partial \mu_1}\sum_{i=1}^{n}t_i \log(\mathcal{N}(x_i \vert\mu_1, \Sigma))= \Sigma^{-1}\sum_{i=1}^{n}t_i(x_i - \mu_1)$$
il quale si annulla per
$$\sum_{i=1}^{n}t_ix_i = \sum_{i=1}^{n}t_i\mu_1 = \mu_1\sum_{i=1}^{n}t_i = \mu_1 \cdot \vert C_1 \vert$$
ovvero $$\mu_1 = \frac{1}{\vert C_1 \vert} \sum_{x \in C_1}x$$
Analogamente per $\mu_2$ avremo $$\mu_2 = \frac{1}{\vert C_2 \vert}\sum_{x \in C_2}x$$

Per quanto riguarda $\Sigma$ invece abbiamo
$$\Sigma = \frac{S_1 + S_2}{n}$$
$$S_1 = \sum_{x \in C_1} (x - \mu_1)^T(x - \mu_1)$$
$$S_2 = \sum_{x \in C_2} (x - \mu_2)^T(x - \mu_2)$$



