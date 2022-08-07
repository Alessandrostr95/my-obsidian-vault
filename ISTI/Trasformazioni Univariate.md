## Trasformazioni Univariate
Sia una v.a. $X$ con pdf $f_X(x)$.
Sia $S$ il **supporto** di $X$, ovvero l'insieme di tutte le $x$ per le quali $f_X(x) > 0$.
$$S \equiv \{x \in \mathbb{R} : f_X(x) > 0\}$$

Supponiamo di avere una trasformazione $y = g(x)$ **biettiva** e definita su $S$.
Data la biettività, allora esiste l'inversa $x = g^{-1}(x)$.


Sia la v.a. $Y \sim g(X)$.
Allora avremo che
$$f_Y(y) = f_X(g^{-1}(y)) \cdot \left\vert \frac{d}{dy} g^{-1}(y) \right\vert$$

### Esempio I 
Sia $X$ una v.a. con pdf
$$f(x) = \begin{cases}
\frac{1}{8}(4-x) &0 < x < 4\\
0 &\text{altrimenti}
\end{cases}$$

^6cca74

e sia $Y = X^2$ (biettiva in nel supporto $(0,4)$).

Un primo approccio per calcolare $f_Y$ è passando calcolando $F_Y$ e poi derivando
$$F_Y(y) = P(Y \leq y) = P(X^2 \leq y) = P(X \leq \sqrt{y}) = F_X(\sqrt{y})$$
Osservare che $g(x) = x^2$ e $g^{-1}(y) = \sqrt{y}$.
Perciò abbiamo che
$$F_Y(y) = F_X(g^{-1}(y))$$
Perciò
$$\begin{align*}
f_Y(y)
&= \frac{d}{dy} F_Y(y)\\
&= \frac{d}{dy}F_X(g^{-1}(y))\\
&= \frac{d}{d\sqrt{y}}F_X(\sqrt{y}) \cdot \frac{d}{dy}\sqrt{y}\\
&= f_X(\sqrt{y}) \cdot \frac{1}{2\sqrt{y}}\\
&= \frac{1}{8}\left( 4 - \sqrt{y} \right) \cdot \frac{1}{2\sqrt{y}}\\
&= \frac{1}{16}\left( \frac{4}{\sqrt{y}} - 1 \right)
\end{align*}$$

### Esempio II
Sia $X$ con pdf analoga al [[#^6cca74|precedente esercizio]], e sia $Y = g(X) = 16 - X^2$.

Abbiamo che $g$ è biettiva su $S = (0, 4)$, con supporto $g(S) = (0, 16)$.
Perciò la sua inversa sarà
$$x = g^{-1}(y) = \sqrt{16 - y}$$

Allora 
$$\begin{align*}
f_Y(y)
&= f_X(\sqrt{16-y}) \cdot \left\vert - \frac{1}{2\sqrt{16-x}} \right\vert\\
&= \frac{1}{8}\left( 4 - \sqrt{16-y}\right) \cdot \frac{1}{2\sqrt{16-x}}\\
&= \frac{1}{16}\left( \frac{4}{\sqrt{16-y}} - 1 \right)
\end{align*}$$
