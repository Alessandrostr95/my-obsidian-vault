# Markov's inequality
Sia $X \in [ 0, + \infty)$ una v.a. **non negativa**, con [[Basic concept and properties#Expected Value|media]] $E[X]$, e sia $a > 0$ una qualsiasi costante.
Allora avremo che
$$P(X \geq a) \leq \frac{E[X]}{a}$$

Nel caso di $X$ discreta avremo che
$$\begin{align*}
E[X]
&= \sum_{x=0}^{+\infty} x \cdot P(X = x)\\
&\geq \sum_{x=a}^{+\infty} x \cdot P(X = x)\\
&\geq a \cdot \sum_{x=a}^{+\infty} P(X = x)\\
&= a \cdot P(X \geq a)
\end{align*}$$
Nel caso di v.a. continue la dimostrazione è analoga.


------
# Chebyshev's inequality
Questa disuguaglianza da un upper bound alla probabilità che una v.a. $X$ devi dalla sua media $E[X]$ di un valore fisso $\varepsilon$.
Più precisamente, sia $X$ una v.a. con media $E[X]$, e $\varepsilon>0$ un qualsiasi valore costante.
Allora
$$P(\vert X - E[X] \vert \geq \varepsilon) \leq \frac{\text{Var}(X)}{\varepsilon^2}$$

Osserviamo che $(X-E[X])^2$ è una v.a. **non negativa**, e possiamo quindi applicare il [[#Markov's inequality]].
$$P(\vert X - E[X] \vert \geq \varepsilon) = P((X-E[X])^2 \geq \varepsilon^2) \leq \frac{E[(X-E[X])^2]}{\varepsilon^2}=\frac{\text{Var}(X)}{\varepsilon^2}$$


## Boosting Chebyshev's by averaging
Un trucco per migliorare il bound dato dalla disuguaglianza di Chebyshev è quello di campionare $n$ [[Basic concept and properties#k-wise indipendence|pair-wise indipendenti]] v.a. $X$ **indipendenti** per poi farne la [[Random Sample#Media campionaria|madia campionaria]] $\overline{X}$.
Sia $\varepsilon > 0$ una qualsiasi costante, e sia $n \geq 1$ un intero qualsiasi.
Siano $X_1, \dots, X_n$ variabili aleatorie [[Basic concept and properties#k-wise indipendence|pair-wise indipendenti]] e **identicamente distribuite**, con [[Random Sample#Media campionaria|madia campionaria]] $\overline{X}= \frac{1}{n}\sum_{i=1}^{n}X_i$.
Allora avremo che
$$P(\vert \overline{X} - E[X] \vert \geq \varepsilon) \leq \frac{\text{Var}(X)}{n \cdot \varepsilon^2}$$
Dato che tutti gli $X_i$ sono **identicamente distribuite**, allora per [[Basic concept and properties#Linearity|linearità]] del valore atteso avremo che $E[\overline{X}] = E[X]$.
Inoltre, essendo **pair-wise indipendenti** allora $\text{Var}(\overline{X})=n \cdot \text{Var}(X)$ (vedi [[Basic concept and properties#Varianza di somma di v.a. indipendenti|qui]]).
Perciò abbiamo che
$$\begin{align*}
P(| \overline{X} - E[X] | \geq \varepsilon)
&= P\left( \left\vert \frac{1}{n}\sum_{i=1}^{n}X_i -E\left[ \frac{1}{n}\sum_{i=1}^{n}X_i\right]\right\vert \geq \varepsilon\right)\\
&= P\left(\frac{1}{n} \left\vert \sum_{i=1}^{n}X_i -E\left[\sum_{i=1}^{n}X_i\right]\right\vert \geq \varepsilon\right)\\
&= P\left(\left\vert \sum_{i=1}^{n}X_i -E\left[\sum_{i=1}^{n}X_i\right]\right\vert \geq n\varepsilon\right)\\
&= \frac{\text{Var}\left(\sum_{i=1}^{n}X_i\right)}{n^2\cdot\varepsilon^2}\\
&= \frac{n\cdot \text{Var}(X)}{n^2\cdot\varepsilon^2}\\
&= \frac{\text{Var}(X)}{n\cdot\varepsilon^2}
\end{align*}$$



------


# Chernoff-Hoeffding's inequalities
La disuguaglianza di Chernoff-Hoeffing (o semplicemente **Chernoff bound**) da un bound alla probabilità che la somma di variabili aleatorie indipendenti e identicamente distribuite si distacca (di una certa quantità) rispetto alla sua [[Basic concept and properties#Expected Value|media]].

## Additive Chernoff's Bound
Siano $Y_1, \dots, Y_n$ variabili aleatorie **completamente indipendenti** secondo una [[ISTI/Distribuzioni#Bernoulli|Bernoilliana]] di parametro $p$.
Sia $Y = \sum_{i=1}^{n}Y_i$.
Allora per ogni $t \geq 0$ abbiamo le seguenti disuguaglianze:
- **Lato destro**: $$P(Y \geq E[Y] + \delta) \leq e^{-\tfrac{\delta^2}{2n}}$$
- **Lato sinistro**: $$P(Y \leq E[Y] - \delta) \leq e^{-\tfrac{\delta^2}{2n}}$$
- **Entrambi i lati**: $$P(|Y - E[Y]| \geq \delta) \leq 2e^{-\tfrac{\delta^2}{2n}}$$

#### Proof:
Sia la v.a. $X_i = Y_i - E[Y_i]$.
Dato che ogni $Y_i$ è una bernulliana, e quindi assume soli valori $\{0,1\}$, allora avremo che $X_i$ può assumere valori $[-p,1-p]$.
Inoltre la media $X_i$ è $$E[X_i] = E[Y_i - E[Y_i]] = E[Y_i] - E[Y_i] = 0$$
Osserviamo inoltre che
- $$P(X_i = 1-p) = P(Y_i - E[Y_i] = 1-p) = P(Y_i = 1) = p$$
-  $$P(X_i = -p) = P(Y_i - E[Y_i] = -p) = P(Y_i = 0) = 1-p$$


Definiamo l'evento $$X = \sum_{i=1}^{n}X_i = \sum_{i=1}^{n}Y_i-\sum_{i=1}^{n}E[Y_i] = Y - E[Y]$$
Perciò avremo che $$P(X \geq \delta) = P(Y-E[Y] \geq \delta) = P(Y \geq E[Y] +\delta)$$
Quindi ci basta dimostrare che $P(X \geq \delta) \leq e^{-\tfrac{\delta^2}{2n}}$.
Stesso argomento vale per il caso sinistro $P(-X \geq \delta) = P(E[Y] -Y \geq \delta) = P(Y \leq E[Y]-\delta)$, e unendo i due bound otterremo anche il caso per entrambi i lati $P(|Y -E[Y]| \geq \delta)$.

Sia $s$ un **parametro libero** che ottimizzeremo in seguito per ottenere il bound.
Osserviamo che i seguenti eventi sono **equivalenti**
$$X \geq \delta \equiv e^{sX} \geq e^{s\delta} \equiv e^{s\sum_{i=1}^{n}X_i} \geq e^{s\delta}$$

Dato che la v.a. $e^{s\sum_{i=1}^{n}X_i}$ è **non negativa** possiamo applicare il [[#Markov's inequality|Markov bound]]
$$P(X \geq \delta) = P(e^{s\sum_{i=1}^{n}X_i} \geq e^{s\delta}) \leq \frac{E\left[e^{s\sum_{i=1}^{n}X_i} \right]}{e^{s\delta}} = \frac{E\left[ \prod\limits_{i=1}^{n}e^{sX_i}\right]}{e^{s\delta}}$$
Dato che ogni v.a. $X_i$ è **indipendente** dalle altre, avremo che
$$P(X\geq \delta) \leq \frac{\left(E\left[ e^{sX_1}\right]\right)^n}{e^{s\delta}}$$

Diamo ora un upperbound al valore $E[e^{sX}]$.
Definiamo i valori $A = \dfrac{1+X_1}{2}$ e  $B=\dfrac{1-X_2}{2}$.
Osservare che
- $A \geq 0$ e $B\geq 0$ perché $X_1 \leq 1$.
- $A+B =1$ e quindi $-B = 1-A$
- $A-B = X_1$

> [!tldr] Jensen Inequality
> 
Sia $f(x)$ una **funzione convessa**. Allora per ogni valore $0 \leq a \leq 1$ abbiamo che $f(ax +(1-a)y) \leq af(x) + (1-a)f(y)$.

^d5a18d

Dato $e^{sX_1}$ è una **funzione convessa**, allora possiamo applicare la [[#^d5a18d|disuguaglianza di Jensen]].
$$\begin{align*}
e^{sX_1}
&= e^{s(A-B)}\\
&= e^{sA -sB}  &(= e^{sA + s(1-A)})\\
&\leq Ae^s + Be^{-s}\\
&= \frac{1+X_1}{2}e^s + \frac{1-X_1}{2}e^{-s}\\
&= \frac{1}{2} (e^s+e^{-s} +X_1e^{s} -X_1e^{-s})\\
&= \frac{e^s+e^{-s}}{2} + X_1 \frac{e^s-e^{-s}}{1}
\end{align*}$$

Dato che $E[X_1] = 0$ allora avremo che
$$\begin{align*}
E[e^{sX_1}]
&\leq E\left[ \frac{e^s-e^{-s}}{2} + X_1\frac{e^s-e^{-s}}{2}\right]\\
&= \frac{e^s+e^{-s}}{2} +E[ X_1] \cdot\frac{e^s-e^{-s}}{2}\\
&= \frac{e^s+e^{-s}}{2} +0\cdot\frac{e^s-e^{-s}}{2}\\
&= \frac{e^s+e^{-s}}{2}
\end{align*}$$

L'espansione in serie di Taylor di $e^s$ e $e^{-s}$ sono:
- $$e^s = \sum_{i=0}^{\infty}\frac{s^i}{i!}$$
- $$e^{-s} = \sum_{i=0}^{\infty}(-1)^{i} \cdot \frac{s^i}{i!}$$

E quindi avremo che $$e^s+e^{-s} = (1+s+\tfrac{s^2}{2!}+\tfrac{s^3}{3!}+\dots ) + (1-s+\tfrac{s^2}{2!}+\tfrac{s^3}{3!}-\dots ) = 2 + 2\frac{s^2}{2!} + 2\frac{s^4}{4!} + \dots = 2\sum_{i=0}^{\infty} \frac{s^{2i}}{(2i)!}$$
Perciò avremo $$E[e^{sX}] \leq \frac{e^{s}+e^{-s}}{2} = \sum_{i=0}^{\infty} \frac{s^{2i}}{(2i)!}$$

Osserviamo ora che $$(2i)! = 2i \cdot (2i-1) \dots (i+1) \cdot i \cdot (i-1) \dots 2 \cdot 1 \geq 2^i \cdot i!$$
perciò
$$E[e^{sX}] \leq \sum_{i=0}^{\infty} \frac{s^{2i}}{(2i)!} \leq \sum_{i=0}^{\infty} \frac{(s^{2})^i}{2^i \cdot i!} = \sum_{i=0}^{\infty} \frac{\left( \cfrac{s^{2}}{2} \right)^i}{i!} = e^{\tfrac{s^2}{2}}$$

Ricapitolando, abbiamo che $$P(X \geq \delta) \leq \frac{\left( e^{\tfrac{s^2}{2}}\right)^n}{e^{s\delta}} = e^{(ns^2-2s\delta)/2}$$

Essendo $e^{(ns^2-2s\delta)/2}$ una funzione concava, essa avrà un punto di minimo quando $(ns^2 - 2s\delta) = 2ns -2\delta=0$, ovvero quando $s = \cfrac{\delta}{n}$.
In conclusione 
$$P(X \geq \delta) \leq \frac{\left( e^{\tfrac{\delta^2}{2n^2}}\right)^n}{e^{\tfrac{\delta^2}{n}}}= e^{-\tfrac{\delta^2}{2n}}$$


## Multiplicative Chenroff's Bound
Siano $Y_1, \dots, Y_n$ variabili aleatorie **completamente indipendenti** secondo una [[ISTI/Distribuzioni#Bernoulli|Bernoilliana]] di parametro $p$.
Sia la v.a. $Y = \sum_{i=1}^{n}Y_i$ con media $\mu = E[Y] = np$.
Allora per ogni valore $0 < \varepsilon < 1$ abbiamo le seguenti disuguaglianze:
- **Lato destro**: $$P(Y \geq (1+\varepsilon)\mu) \leq e^{-\mu\tfrac{\varepsilon^2}{3}}$$
- **Lato sinistro**: $$P(Y \leq (1-\varepsilon)\mu) \leq e^{-\mu\tfrac{\varepsilon^2}{2}}$$
- **Entrambi i lati**: $$P(|Y - \mu | \geq \varepsilon\mu ) \leq 2e^{-\tfrac{\varepsilon^2}{3}}$$

