Abbiamo come dominio lo spazio $\mathbb{R}^D$, e vogliamo trovare l'**iperpiano** di dimensione $D-1$ che **separa** tutti gli elementi della classe $C_0$ da quelli della classe $C_1$.

Dato un vettore di parametri $(w_0, w_1, ..., w_D)$, supponiamo che l'**iperpiano di separazione** tra le regioni che identificano le classi è definito dall'equazione $$y(\mathbf{w}) = \mathbf{w}^T\mathbf{x} + w_0 = 0$$ per ogni $\mathbf{x}$ appartenente all'iperpiano di separazione.

Se questo è vero per ogni punto $\mathbf{x}_1, \mathbf{x}_2$ dell'iperpiano, allora deve essere vero che $$y(\mathbf{x}_1) - y(\mathbf{x}_2)= (\mathbf{w}^T\mathbf{x}_1+w_0) - (\mathbf{w}^T\mathbf{x}_2+w_0) = \mathbf{w}^T(\mathbf{x}_1-\mathbf{x}_2) = 0$$ ovvero $(\mathbf{x}_1-\mathbf{x}_2)$ è un vettore **ortogonale** a $\mathbf{w}$, e quindi l'iperpiano sarà **perpendicolare** al vettore $\mathbf{w}$.

Perciò una **condizione** per **discriminare** la classe $C_0$ dalla classe $C_1$ è la seguente $$\begin{cases}
\mathbf{x} \in C_0 \iff y(\mathbf{x}) > 0\\
\mathbf{x} \not\in C_0 \iff y(\mathbf{x}) < 0\\
\end{cases}$$

Nel caso di $K > 2$ classi possiamo definire $K$ **funzioni discriminanti** $$y_i(\mathbf{x}) = w_{i0} + \mathbf{w}_{i}^T\mathbf{x}$$Perciò un elemento $\mathbf{x}$ appartiene alla classe $C_k$ se e solo se $y_k(\mathbf{x}) > y_j(\mathbf{x})$ per ogni altro $j \neq k$.
Perciò la [[Linear Classification#^3cfa43|discriminant function]] è della forma $$k = \arg\max_{1 \leq j \leq K}y_j(\mathbf{x})$$

Per quanto riguarda i **margini** tra ogni coppia di regioni $C_i, C_j$ sono tutti quei punti $\mathbf{x}$ tali che $y_i(\mathbf{x}) = y_j(\mathbf{x})$, o in altri termini l'iperpiano che comprende tutti quei punti $\mathbf{x}$ tali che $$(\mathbf{w}_i - \mathbf{w}_j)^T\mathbf{x}+(w_{i0} - w_{j0}) = 0$$
![[ML/img/ML_06_1.png]]

## Learning parameters $\mathbf{w}$
Supponiamo di voler **discriminare** $K \geq 2$ classi, e supponiamo usare la [[Linear Classification#^85e2b9|codifca one-hot]].
Quindi ad ogni elemento $\mathbf{x}$ è associato un vettore $\mathbf{z} = (z_1, ..., z_K) \in \lbrace 0,1 \rbrace^K$ tale che $z_i = 1 \iff \mathbf{x} \in C_i$.

Definiamo quindi $K$ funzioni discriminative lineari che identificano gli elementi di $\mathbf{z}$ 
$$\begin{bmatrix}
y_1(\mathbf{x})\\
y_2(\mathbf{x})\\
\vdots\\
y_K(\mathbf{x})
\end{bmatrix} = 
\begin{bmatrix}
w_{10} + \mathbf{w}_1^T\mathbf{x}\\
w_{20} + \mathbf{w}_2^T\mathbf{x}\\
\vdots\\
w_{K0} + \mathbf{w}_K^T\mathbf{x}\\
\end{bmatrix}$$
tali che $$z_i = 1 \iff i = \arg \max_{1 \leq i \leq K} y_i(\mathbf{x})$$ e vale 0 altrimenti.

```ad-info
In parole povere, $\mathbf{z}$ è il vettore che contiene un solo 1 nella posizione in cui si trova l'indice del **valore massimo** tra le funzioni discriminative $y_i$.
```

Raggruppando tutte queste funzioni in una **matrice** avremo 

$$\mathbf{y}(\mathbf{x}) = \begin{bmatrix}
y_1(\mathbf{x})\\
y_2(\mathbf{x})\\
\vdots\\
y_K(\mathbf{x})
\end{bmatrix} = 
\begin{bmatrix}
w_{10} + \mathbf{w}_1^T\mathbf{x}\\
w_{20} + \mathbf{w}_2^T\mathbf{x}\\
\vdots\\
w_{K0} + \mathbf{w}_K^T\mathbf{x}\\
\end{bmatrix} = 
\mathbf{W}^T\mathbf{x} = 
\begin{bmatrix}
w_{10} &w_{12} & \cdots &w_{1D}\\
w_{20} &w_{22} & \cdots &w_{2D}\\
\vdots &\vdots & \ddots &\vdots\\
w_{K0} &w_{K2} & \cdots &w_{KD}
\end{bmatrix} 
\begin{bmatrix}
1\\
x_1\\
\vdots\\
x_D
\end{bmatrix}$$
con $\mathbf{W} \in \mathbb{R}^{(D+1) \times K}$.

Sarebbe desiderabile che $$\begin{align}
y_i(\mathbf{x})
&\approx \mathbb{E}\left[ z_i \vert \mathbf{x}\right]\\
&= 1 \cdot P(z_i = 1 \vert \mathbf{x}) + 0 \cdot 1 \cdot P(z_i = 0 \vert \mathbf{x})\\
&= P(z_i = 1 \vert \mathbf{x})\\
&= P(C_i \vert \mathbf{x})\\
\end{align}$$ 

Sia la matrice dei $\mathbf{X} \in \mathbb{R}^{n \times(D+1)}$ che indica gli $n$ elementi del training set (con un 1) iniziale
$$\mathbf{X} = \begin{bmatrix}
1 &x_{11} &\cdots &x_{1D}\\
1 &x_{21} &\cdots &x_{2D}\\
\vdots  &\vdots &\ddots &\vdots \\
1 &x_{n1} &\cdots &x_{nD}
\end{bmatrix}$$
Indichiamo invece con $\mathbf{T} \in \mathbb{R}^{n \times K}$ la matrice contente la one-hot encoding dei target del dataset 
$$\mathbf{T} = \begin{bmatrix}
t_{11} &t_{12} &\cdots &t_{1K}\\
t_{21} &t_{22} &\cdots &t_{2K}\\
\vdots  &\vdots &\ddots &\vdots \\
t_{n1} &t_{n2} &\cdots &t_{nK}
\end{bmatrix}$$


Possiamo quindi rappresentare la matrice $\mathbf{Y} \in \mathbb{R}^{K \times n}$ che contiene tutte le funzioni $y_i(\mathbf{x}_j)$ come il prodotto
$$\mathbf{W}^T \mathbf{X}^T = \mathbf{X} \mathbf{W} = \mathbf{Y} = \begin{bmatrix}
y_1(\mathbf{x}_1) &y_1(\mathbf{x}_2) &\cdots &y_1(\mathbf{x}_n)\\
y_2(\mathbf{x}_1) &y_2(\mathbf{x}_2) &\cdots &y_2(\mathbf{x}_n)\\
\vdots  &\vdots &\ddots &\vdots \\
y_K(\mathbf{x}_1) &y_K(\mathbf{x}_2) &\cdots &y_K(\mathbf{x}_n)
\end{bmatrix}$$

Dato che è desiderabile che $y_i(\mathbf{x}_j)$ sia una **stima del valore atteso** di $t_{j,i}$, allora possiamo esprimere lo **scarto** $(y_i(\mathbf{x}_j) - t_{j,i})$ in **forma matriciale** come
$$(\mathbf{Y} - \mathbf{T}^T) = (\mathbf{X}\mathbf{W} - \mathbf{T}^T) \in \mathbb{R}^{K \times n}$$

Consideriamo ora la **diagonale** della matrice $$(\mathbf{Y} - \mathbf{T}^T)^T(\mathbf{Y} - \mathbf{T}^T) \in\mathbb{R}^{n \times n}$$
È facile verificare che gli elementi sulla diagonale corrispondono agli **scarti quadratici** $$((\mathbf{Y} - \mathbf{T}^T)^T(\mathbf{Y} - \mathbf{T}^T))_{\ell,\ell} = \sum_{i=1}^{n}(y_\ell(\mathbf{x}_i) - t_{i, \ell})^2$$
Perciò, si vuole che la matrice $\mathbf{W}$ **minimizzi** gli scarti quadratici tra predizione e valore reale della classe, o in termini matriciali 
$$\mathbf{W}^* := \arg\min_{\mathbf{W} \in \mathbb{R}^{(D+1) \times K}} \sum \text{tr}\left( (\mathbf{X}\mathbf{W} - \mathbf{T}^T)^T(\mathbf{X}\mathbf{W} - \mathbf{T}^T) \right)$$

In **forma chiusa** la soluzione risulta essere $$\mathbf{W} = (\mathbf{X}^T\mathbf{X})^{-1}\mathbf{X}^T\mathbf{T}$$
## Generalized discriminant functions
Un approccio **generalizzato** consiste nell'applicare delle [[Some Base Function|base function]] $\phi_1, ..., \phi_m: \mathbb{R}^D \to \mathbb{R}$, così ottenendo la funzione lineare $$y(\mathbf{x}) = w_0 + \sum_{i=1}^{m}w_i\phi_i(\mathbf{x})$$
