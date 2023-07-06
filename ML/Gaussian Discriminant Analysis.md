Nella **Gaussian Discriminant Analysis** (**GDA**) si assume che ogni probabilità $p(\mathbf{x} \vert C_k)$ degli elementi $\mathbf{x} \in \mathbb{R}^d$ rispetto alla classe $C_k$ sia una [[CLT - Central Limit Theorem#Normali Multivariate|gaussiana multivariata]].

Sia quindi $\mu_k$ il punto medio della classe $C_k$ e $\Sigma \in \mathbb{R}^{d \times d}$ la **matrice di covarianza**, allora la probabilità di $\mathbf{x} \vert C_k$ avrà *densità* $$p(\mathbf{x} \vert C_k) = \frac{1}{(2\pi)^{d/2}\vert \text{det}(\Sigma) \vert^{1/2}} \exp{\left(-\frac{1}{2}(\mathbf{x} - \mu_k)^T\Sigma^{-1}(\mathbf{x} - \mu_k)\right)}$$

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

![[ML/img/ML_06_8.png]] ^e433ad


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

