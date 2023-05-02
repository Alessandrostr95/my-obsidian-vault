Per semplicità, consideriamo il caso della [[Supervised Learning#^42e3b6|regressione]].
Assumimo quindi che sia $y = h(x)$ che il relatico target $t$ siano *valori reali*.

# Quadratic Loss
Una comune [[Prediction Risk#^0b44e2|funzione loss]] [[Convessità|convessa]] è la **distanza quadratia**.
$$L(y,t) = (y-t)^2$$
![](./img/ML_03_4.png)
Questa funzione loss è anche nota come **quadratic loss**.

Se applichiamo quindi la *loss quadratica* alla funzione di [[Prediction Risk#^9cd1a0|rischio empirico]] avremo $$\overline{\mathcal{R}}_{\mathcal{T}}(h) = \frac{1}{\vert \mathcal{T} \vert} \sum_{(x,t) \in \mathcal{T}}(h(x)-t)^2$$

Consideriamo per esempio la [[Linear Regression]], ovvero $$h_{\mathbf{w}, w_0}(x) = \mathbf{w}^T\mathbf{x} + w_0 = w_0 + w_1x_1 + w_2x_2 + ... + w_dx_d$$
Abbiamo quindi che i parametri da cui dipende $h$ sono i coefficienti $(w_0, w_1, ..., w_d)$.

La funzione loss sarà quindi $$\mathcal{L}(\mathbf{w}, w_0 \vert \mathcal{T}) = \sum_{(\mathbf{x}, t) \in \mathcal{T}} (\mathbf{w}^T\mathbf{x} + w_0 - t)^2$$

Le **derivate parziali** saranno
$$\begin{align}
\dfrac{\partial}{\partial w_i} \mathcal{L}(\mathbf{w}, w_0 \vert \mathcal{T}) &= \sum_{(\mathbf{x}, t) \in \mathcal{T}} (\mathbf{w}^T\mathbf{x} + w_0 - t)w_i &\forall i = 1, ..., d\\
\dfrac{\partial}{\partial w_0} \mathcal{L}(\mathbf{w}, w_0 \vert \mathcal{T}) &= \sum_{(\mathbf{x}, t) \in \mathcal{T}} (\mathbf{w}^T\mathbf{x} + w_0 - t)
\end{align}$$



