## Distribution Function
Sia $\Omega$ uno spazio di eventi.
Definiamo quindi:

#### CDF (Cumulative distribution function o funzione di ripartizione)
La funzione di ripartizione di una v.a. $X$ è definita come $F_X(x) = P(X \leq x)$.

#### PMF (Probability mass function o distribuzione)
Data una v.a. **discreta** $X$, la sua funzione di massa $f_X(x)$ (o distribuzione) è definita come la probabilità che $X$ assuma valore $x$, ovvero $f_X(x) = P(X = x)$

#### PDF (Probability density function)
Data una v.a. **continua** $X$, la sua funzione di densità $f_X(x)$ è una funzione continua per la quale vale la seguente uguaglianza
$$F_X(t) = \int_{-\infty}^{t} F_X(x) \, dx$$

------
## k-wise indipendence
Sia $W = \{ X_1, \dots, X_n \}$ un insieme di $n$ v.a.
L'insieme $W$ è detto $k$-wise indipendent se e solo se vale che 
$$P\left(\bigcup_{j=1}^{k} \{ X_{i_j} = x_{i_j}\} \right) = \prod_{j=1}^{k} P\left( X_{i_j} = x_{i_j}\right)$$
per ogni sottoinsieme $\{X_{i_1} , \dots, X_{i_k} \} \subseteq W$ di dimensione $k$.
Nel caso speciale in cui $k = n$ allora diremo che $W$ è **fully indipendent**.


## Union Bound
Siano gli eventi $A_1, \dots, A_n$.
Allora è vera la seguente disuguaglianza 
$$P\left(\bigcup_{i=1}^{n} A_i \right) \leq \sum_{i=1}^{n} P( A_i )$$


```ad-warning
Alcune volte questa disuguaglianza potrebbe risultare non informatia, in quanto il bound potrebbe ecceddere $1$.
```

## Law of Total Probability
Sia $\{ B_i \}_{i=1, \dots, n}$ una  **partizione** di $\Omega$.
Allora per ogni evento $A \subseteq \Omega$ avremo che
$$\begin{align*}
P(A)
&= P(A \cap \Omega)\\
&= P(A \cap (B_1 \cup \dots \cup B_n))\\
&= \sum_{i=1}^{n} P(A \cap B_i)\\
&= \sum_{i=1}^{n} P(A \mid B_i) \cdot P(B_i)\\
\end{align*}$$

------
# Expected Value
Il **valore atteso** (o **media**) di una variabile aleatoria $X \in \Omega$ **discreta** è definita come la media dei valori che $X$ può assumere, rispetto alla loro probabilità.
Formalmente $$E[X] = \sum_{x \in \Omega} x \cdot P(X = x)$$
Nel caso di variabili **continue** il valore atteso si ottiene integrando rispetto alla [[#PDF (Probability density function)|densità]] di $X$, ovvero
$$E[X] = \int_{-\infty}^{+\infty}x \cdot f_X(x) \, dx$$
Il valore atteso di un valore costante $a \in \mathbb{R}$ è esattamente $a$ ($E[a] = a$).

## Linearity
È facile verificare che il valore atteso è una **funzione lineare**, ovvero $$E\left[\sum_{i=1}^{n} a_i \cdot X_i \right] =  \sum_{i=1}^{n}a_i \cdot E[X_i]$$
Certamente la media è **omogenea**, in quanto
$$E[aX] = \sum_{x\in \Omega} a\cdot x \cdot P(X=x) =a\cdot  \sum_{x\in \Omega}x \cdot P(X=x) = a \cdot E[X]$$
per ogni costante $a \in \mathbb{R}$  e per ogni v.a. $X \in \Omega$.

Per quanto riguarda l'**additività** invece abbiamo
$$\begin{align*}
E\left[ \sum_{i=1}^{n} X_i \right]
&= \sum_{x_1 \in \Omega} \dots \sum_{x_n \in \Omega} (x_1 + \dots + x_n) \cdot P(X_1 = x_1 \,,\, \dots \,,\, X_n = x_n)\\
&= \sum_{x_1 \in \Omega} \dots \sum_{x_n \in \Omega} x_1 \cdot P(X_1 = x_1 \,,\, \dots \,,\, X_n = x_n) + \dots + \sum_{x_1 \in \Omega} \dots \sum_{x_n \in \Omega} x_n \cdot P(X_1 = x_1 \,,\, \dots \,,\, X_n = x_n)\\
&= \sum_{x_1 \in \Omega} x_1 \sum_{x_2 \in \Omega} \dots \sum_{x_n \in \Omega} P(X_1 = x_1 \,,\, \dots \,,\, X_n = x_n) + \dots + \sum_{x_n \in \Omega} x_n \sum_{x_1 \in \Omega} \dots \sum_{x_{n-1} \in \Omega} P(X_1 = x_1 \,,\, \dots \,,\, X_n = x_n)\\
&= \sum_{x_1 \in \Omega} x_1 P(X_1 = x_1) + \dots +\sum_{x_n \in \Omega} x_n P(X_n = x_n)\\
&= \sum_{i=1}^{n} E[X_i]
\end{align*}$$
per ogni sottoinsieme di v.a. $X_1, \dots, X_n \in \Omega$.
Argomentazione analoga nel caso di v.a. continue.


## Expectation of a product of fully indipendent random variables
Un'altra proprietà interessante del valore atteso è la seguente.
Siano $X_1, \dots, X_n$ v.a. [[#k-wise indipendence|fully indiepents]], allora è vero che 
$$E\left[ \prod_{i=1}^{n} X_i\right] =\prod_{i=1}^{n} E\left[  X_i\right]$$
$$\begin{align*}
E\left[ \prod_{i=1}^{n} X_i \right]
&= \sum_{x_1 \in \Omega} \dots \sum_{x_n \in \Omega} (x_1 \, \cdot \, ... \, \cdot \, x_n) \cdot P(X_1 = x_1 \,,\, \dots \,,\, X_n = x_n)\\
&= \sum_{x_1 \in \Omega} \dots \sum_{x_n \in \Omega} (x_1 \, \cdot \, ... \, \cdot \, x_n) \cdot P(X_1 = x_1) \,\cdot \,...\,\cdot\, P(X_n = x_n)\\
&= \sum_{x_1 \in \Omega} x_1 \cdot P(X_1 = x_1) \left( \sum_{x_2 \in \Omega} \dots \sum_{x_n \in \Omega} (x_2 \, \cdot \, ... \, \cdot \, x_n) \cdot P(X_2 = x_2) \,\cdot \,...\,\cdot\, P(X_n = x_n) \right)\\
&\;\;\vdots\\
&=\left( \sum_{x_1 \in \Omega} x_1 \cdot P(X_1 = x_1) \right) \, \cdot \, ... \, \cdot \, \left( \sum_{x_n \in \Omega} x_n \cdot P(X_n = x_n) \right)\\
&= \prod_{i=1}^{n} E[X_i]
\end{align*}$$


## Expectation of non-negative random variables
Sia $X \in [0, + \infty)$ una v.a. continua **non negativa**.
Allora vale la seguente uguaglianza
$$E[X] = \int_0^{+\infty} P(X \geq x) \, dx$$

Per dimostrarlo, osserviamo che $$P(X \geq x) = P(X \leq +\infty) - P(X < x) = F_X(+\infty) - F_X(x) = \int_x^{+\infty}f_X(t) \, dt$$
perciò
$$\int_0^{+\infty}P(X \geq x) \, dx = \int_0^{+\infty} \int_x^{+\infty}f_X(t) \, dt \,dx$$

Dato che si integra per valori di $t \geq x$, possiamo pensare il primo integrale per $x \leq t$, ovvero 
$$\int_0^{+\infty} \int_x^{+\infty}f_X(t) \, dt \,dx = \int_0^{+\infty} \int_0^{t}f_X(t) \, dx \,dt$$
In conclusione, visto che $\int_0^tf_X(t)\,dx =f_X(t)\int_0^t\,dx = f_X(t) \cdot t$, abbiamo che
$$\int_0^{+\infty} P(X \geq x) \,dx = \int_0^{+\infty} \int_0^{t}f_X(t) \, dx \,dt = \int_0^{+\infty} t \cdot f_X(t) \,dt = E[X]$$


## Media condizionata
Sia $X \in \Omega$ una v.a. e $A \subseteq \Omega$ un evento.
Allora la **media condizionata** di $X$ rispetto all'evento $A$ è definita come
$$E[ X \mid A] = \sum_{x \in \Omega} x \cdot P(X = x \mid A)$$

## Law of Total Expectation
Una versione equivalente della [[#Law of Total Probability]] per il valore atteso è la seguente.
Sia $\{ B_i \}_{i=1, \dots, n}$ una  **partizione** di $\Omega$.
Allora per ogni evento $X \subseteq \Omega$ avremo che
$$\begin{align*}
E[X]
&= \sum_{x \in \Omega} x \cdot P(X = x)\\
&= \sum_{x \in \Omega} x \cdot \left( \sum_{i=1}^{n} P(\{ X = x \} \mid B_i) \cdot P(B_i) \right)\\
&= \sum_{x \in \Omega} \sum_{i=1}^{n} x \cdot P(\{ X = x \} \mid B_i) \cdot P(B_i)\\
&= \sum_{i=1}^{n}\sum_{x \in \Omega} x \cdot P(\{ X = x \} \mid B_i) \cdot P(B_i)\\
&= \sum_{i=1}^{n} E[X \mid B_i] \cdot P(B_i)
\end{align*}$$

------
# Variance
La varianza di una v.a. $X$ ci indica quanto essa si distacchi (in media) dal suo [[#Expected Value|valore atteso]].
Infatti essa è definita come la media della v.a. $(X-E[X])^2$, che rappresenta lo scarto quadratico tra $X$ ed $E[X]$.
Per questo motivo è anche nota come **scarto quadratico medio**.
Più formalmente, sia $X$ una v.a. con valore atteso $E[X]$.
Allora la varianza $\text{Var}(X)$ di $X$ è definita come $$\text{Var}(X) = E\left[(X - E[X])^2\right] = E\left[X^2\right] - E[X]^2$$

Infatti abbiamo che
$$\begin{align*}
\text{Var}(X)
&= E[(X - E[X])^2]\\
&= E\left[ X^2 - 2XE[X] + E[X]^2 \right]\\
&= E\left[ X^2 \right] - E\left[ 2XE[X] \right] + E\left[E[X]^2 \right]\\
&= E\left[ X^2 \right] - 2E[X]E[X] + E[X]^2\\
&= E\left[ X^2 \right] - E[X]^2
\end{align*}$$

Per ogni costante $a > 0$ vale che
$$\text{Var}(aX) = a^2 \cdot \text{Var}(X)$$
infatti
$$\begin{align*}
\text{Var}(aX)
&= E\left[a^2X^2\right] - E[aX]^2\\
&= a^2E\left[X^2\right]- E[aX]E[aX]\\
&= a^2E\left[X^2\right] - a^2E[X]^2\\
&= a^2 \cdot \text{Var}(X)
\end{align*}$$

### Varianza di somma di v.a. indipendenti
Siano $X_1, \dots X_n$ v.a. [[#k-wise indipendence|pair-wise indipendenti]], allora vale che $$\text{Var}\left( \sum_{i=1}^{n} X_i \right) = \sum_{i=1}^{n}\text{Var}(X_i)$$

Innanzitutto osserviamo che $$E\left[ \left( \sum_{i=1}^{n} X_i \right)^2 \right] = E\left[ \sum_{i=1}^{n}X_i^2 + 2\sum_{i\neq j}X_iX_j \right] = \sum_{i=1}^{n}E\left[X^2 \right] + 2 \sum_{i \neq j}E[X_i X_j]$$
Ora invece abbiamo che
$$E\left[ \sum_{i=1}^{n}X_i \right]^2 = \left( \sum_{i=1}^{n}E\left[X_i \right] \right)^2 =  \sum_{i=1}^{n}E[X_i]^2 + 2\sum_{i\neq j}E[X_i]E[X_j]$$


Dato che le v.a. sono pair-wise indipendenti allora $E[X_i X_j] = E[X_i]E[X_j]$.
Perciò
$$\begin{align*}
\text{Var}\left( \sum_{i=1}^{n}X_i \right) &= E\left[ \left( \sum_{i=1}^{n} X_i \right)^2 \right] - E\left[ \sum_{i=1}^{n} X_i \right]^2\\
&=\sum_{i=1}^{n}E\left[ X_i^2 \right] - \sum_{i=1}^{n}E[X_i]^2\\
&= \sum_{i=1}^{n}\left(E\left[ X_i^2 \right] - E\left[ X_i \right]^2 \right)\\
&= \sum_{i=1}^{n}\text{Var}(X_i)
\end{align*}$$


