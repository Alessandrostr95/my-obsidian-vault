## Random Sample
Le v.a. $X_1,...,X_n$ sono dette un **campionamento** (o **random sample**) di **dimensione** $n$ da una **popolazione** $f(x)$ se $X_1,...,X_n$ sono **mutualmente indipendenti** e se la [[Distribuzioni Multivariate#Teorema 4 1 6 - Distribuzioni marginali|densità marginale]] è la stessa $f(x)$.

Alternativamente $X_1, ..., X_n$ sono anche dette **i.i.d.** con *pdf* o *pmf* $f(x)$.

Osserviamo che se il campionamento $(X_1, ..., X_n)$ ha [[Distribuzioni Multivariate#Joint PMF|distribuzione congiunta]] $f(x_1, ..., x_n)$, per indipendenza avremo la seguente uguaglianza

$$f(x_1, ..., x_n) = f(x_1) \cdot ... \cdot f(x_n) = \prod_{i = 1}^{n} f(x_i)$$
Analogamente quando il campionamento dipende da un singolo **parametro condiviso** $\theta$.
$$f(x_1, ..., x_n \vert \theta) = \prod_{i=1}^{n} f(x_i \vert \theta)$$

### Esempio - campionamento di esponenziali
Sia $X_1,...,X_n$ un campionamento da una popolazione esponenziale $\text{Exp}(\beta)$.
Ovvero
$$X_i = e^{-x_i/\beta}$$
Supponendo quindi che $\beta$ è il **parametro** sul quale il campionamento dipende, avremo quindi una [[Distribuzioni Multivariate#Joint PDF|joint pdf]] del tipo
$$f(x_1,...,x_n \vert \beta) = \prod_{i=1}^{n}f(x_i \vert \beta) = \prod_{i=1}^{n}\frac{1}{\beta}e^{-x_i/\beta} = \frac{1}{\beta^n}e^{-(x_1+...+x_n)/\beta}$$
Questa *pdf* può essere molto utile per rispondere ad alcune domande.
Per esempio supponiamo che $X_i$ indichi il tempo (espresso in anni) entro il quale un circuito si rompe.
Perciò, qual è la probabilità che tutti i circuiti sopravvivano più di 2 anni?
$$\begin{align*}
P(X_1 > 2, ..., X_n > 2)
&= \int_{2}^{\infty} \cdots \int_{2}^{\infty} f(x_1, ..., x_n \vert \theta) \, dx_1 \cdots dx_n\\
&= \int_{2}^{\infty} \cdots \int_{2}^{\infty} \prod_{i=1}^{n}\frac{1}{\beta}e^{-x_i/\beta} \, dx_1 \cdots dx_n\\
&= \int_{2}^{\infty} \cdots \int_{2}^{\infty} \left[-e^{-x/\beta} \right]_{2}^{\infty} \prod_{i=2}^{n}\frac{1}{\beta}e^{-x_i/\beta} \, dx_2 \cdots dx_n\\
&= \int_{2}^{\infty} \cdots \int_{2}^{\infty}  e^{-2/\beta} \prod_{i=2}^{n}\frac{1}{\beta}e^{-x_i/\beta} \, dx_2 \cdots dx_n\\
&\;\;\vdots\\
&= \left( e^{-2/\beta}\right)^n\\
&= e^{-2n/\beta}
\end{align*}$$

Ricordando che $\beta$ è il **valore atteso** di $\text{Exp}(\beta)$ (in questo caso in quanto tempo in media si rompe un circuito), avremo che se $\beta \approx n$ allora tale probabilità sarà prossima ad 1.

--------------------------
## Somme di variabili aleatorie di un campionamento
Sia $X_1, ..., X_n$ un [[Random Sample#Random Sample|random sample]] di dimensione $n$ di una generica popolazione, e sia $T(x_1, ..., x_n)$ una funzione il cui dominio è lo spazio di campionamento di $(X_1, ..., X_n)$.

La v.a. (o vettore) $Y \sim T(X_1, ..., X_n)$ è chiamata **statistica**.

La distribuzione di probabilità di una statistica $Y$ è anche chiamata **distribuzione campionaria** (o **sampling distribution**).

### Media campionaria
La **media campionaria** di un sampling $X_1, ..., X_n$ è semplicemente la **media aritmetica**
$$\overline{X} = \frac{X_1 + ... + X_n}{n}$$
### Varianza campionaria
La **varianza campionaria** di un sampling $X_1, ..., X_n$ è definita come
$$S^2 = \frac{\sum_{i=1}^{n}(X_i - \overline{X})^2}{n-1}$$

Analogamente la **deviazione standard campionaria** è definita come $S = \sqrt{S^2}$.

### Teorema 5.2.4
Sia i valori $x_1, ..., x_n$ e i valori $\overline{x} = (x_1 + ... + x_n)/n$ e $s^2 = (\sum_i (x_1 - \overline{x})^2)/(n-1)$
Allora valgono le seguenti prorpietà:
1. $$\overline{x} = \arg \min_{a \in \mathbb{R}}\sum_{i=1}^{n}(x_i - a)^2$$
2. $$(n-1)s^2 = \sum_{i=1}^{n}x_i^2 - n\overline{x}$$

#### Proof
$$\begin{align*}
\sum_{i=1}^{n}(x_i - a)^2
&= \sum_{i=1}^{n}(x_i - \overline{x} + \overline{x} - a)^2\\
&= \sum_{i=1}^{n}((x_i - \overline{x}) + (\overline{x} - a))^2\\
&= \sum_{i=1}^{n}(x-\overline{x})^2 + 2\sum_{i=1}^{n}(x_i - \overline{x})(\overline{x} - a) + \sum_{i=1}^{n}(\overline{x} - a)^2
\end{align*}$$
e tale quantità è minimizzata per $a = \overline{x}$.

$$\begin{align*}
(n-1)s^2
&= \sum_{i=1}^{n}(x_i - \overline{x})^2\\
&= \sum_{i=1}^{n}x_i^2 - 2\sum_{i=1}^{n}x_i\overline{x} + \sum_{i=1}^{n}\overline{x}^2\\
&= \sum_{i=1}^{n}x_i^2 - \frac{2}{n}\sum_{i=1}^{n}x_i(x_1 + ... + x_i + ... + x_n) + n\overline{x}^2\\
&= \sum_{i=1}^{n}x_i^2 - \frac{2}{n}\sum_{i=1}^{n}(x_ix_1 + ... + x_i^2 + ... + x_ix_n) + n\overline{x}^2\\
&= \sum_{i=1}^{n}x_i^2 - \frac{2}{n}\sum_{i=1}^{n}\left[ x_i^2 + \sum_{j \neq i} x_ix_j \right] + n\overline{x}^2\\
&= \sum_{i=1}^{n}x_i^2 - \frac{2}{n}\underbrace{\left[\sum_{i=1}^{n} x_i^2 + 2\sum_{j \neq i} x_ix_j \right]}_{(x_1 + ... +x_n)^2 = \overline{x}^2n^2} + n\overline{x}^2\\
&= \sum_{i=1}^{n}x_i^2 - 2n\overline{x}^2 + n\overline{x}^2\\
&= \sum_{i=1}^{n}x_i^2 - n\overline{x}^2 \;\; \square
\end{align*}$$


### Lemma 5.2.5 - media e lemma
Sia $X_1, ..., X_n$ un random sampling, e sia una funzione $g(x)$ tale che $\mathbb{E}\left[ g(X_i)\right]$ e $\text{Var}(g(X_i))$ esistono.
Allora avremo che
1. $$\mathbb{E}\left[ \sum_{i=1}^{n}g(X_i) \right] = n\mathbb{E}\left[ g(X_1) \right]$$
2. $$\text{Var}\left( \sum_{i=1}^{n}g(X_i) \right) = n\text{Var}\left( g(X_1) \right)$$
#### Proof
Per il punto 1 basta sfruttare la **linearità**.
Per il punto 2 basta osservare che nel caso di **indipendenza** allora anche la varianza è lineare $\square$.

### Teorema 5.2.6 - Proprietà importanti
Sia il random sample $X_1, ..., X_n$ con media $\mu$ e vairanza finita $\sigma^2 < \infty$.
Allora valgono le seguenti proprietà:
1. $$\mathbb{E}\left[ \overline{X} \right] = \mu$$ ^69143c
2. $$\text{Var}\left(\overline{X}\right) = \frac{\sigma^2}{n}$$ ^2864bf
3. $$\mathbb{E}\left[ S^2 \right] = \sigma^2$$ ^89dd43

#### Proof
Per il [[#^69143c|punto 1]] sfruttiamo la linearità del valore atteso
$$\mathbb{E}\left[ \overline{X} \right] = \mathbb{E}\left[ \frac{1}{n}\sum_{i=1}^{n}X_i \right] = \frac{1}{n}n\mathbb{E}\left[ X_1 \right] = \mu $$

Analogamente per il [[#^2864bf|punto 2]]
$$\text{Var}\left( \overline{X} \right) = \text{Var}\left( \frac{1}{n} \sum_{i=1}^{n} X_i \right) = \frac{1}{n^2} \text{Var}\left( \sum_{i=1}^{n} X_i \right) = \frac{1}{n^2}n \text{Var}\left( X_1 \right) = \frac{\sigma^2}{n}$$

In fine per il [[#^89dd43|punto 3]] sfruttiamo il [[#Teorema 5 2 4]] 
$$\begin{align*}
\mathbb{E}\left[ S^2 \right]
&= \mathbb{E}\left[ \frac{1}{n-1} \sum_{i=1}^{n}(X_i - \overline{X})^2 \right]\\
&= \frac{1}{n-1} \mathbb{E}\left[ \sum_{i=1}^{n}X_i^2 - n\overline{X}^2 \right]\\
&= \frac{1}{n-1} \left( n\mathbb{E}\left[X_1^2\right] - n\mathbb{E}\left[\overline{X}^2 \right] \right)
\end{align*}$$
Osserviamo che
$$\text{Var}(X) = \mathbb{E}\left[X^2\right] - \mathbb{E}^2\left[X\right] \implies \sigma^2 = \mathbb{E}\left[X^2\right] - \mu^2 \implies \mathbb{E}\left[X^2\right] = \sigma^2 + \mu^2$$
Perciò sfruttando i punti [[#^69143c|1]] e [[#^2864bf|2]] avremo che
$$\begin{align*}
\mathbb{E}\left[ S^2 \right]
&= \frac{1}{n-1} \left( n(\sigma^2 - \mu^2) - n\left(\frac{\sigma^2}{n} - \mu^2 \right) \right)\\
&=\frac{n}{n-1} \left( \sigma^2 - \cancel\mu^2 - \frac{\sigma^2}{n} + \cancel\mu^2 \right)\\
&=\frac{n}{n-1} \left( \sigma^2 - \frac{\sigma^2}{n} \right)\\
&= \sigma^2 \;\; \square
\end{align*}$$

#### Oservazione
Il punto [[#^89dd43|punto 3]] spiegato il perché nella definizione di $S^2$ si divide per $n-1$ e non per $n$.

-----------------------------
## Teorema 5.2.7 - FGM della media campionaria
Sia $X_1,...,X_n$ un *random sample*, dove ogni v.a. ha [[Distribuzioni Multivariate#Moment Generating Function|fgm]] $M_X(t)$.
Allora la *fgm* della media campionaria $\overline{X}$ risulterà essere $$M_{\overline{X}}(t) = \left[ M_X(t/n) \right]^n$$ dove $X$ è un qualsiasi $X_i$ (tanto sono i.i.d.).

### Proof
Sfruttando il fatto che $X_1,...,X_n$ sono i.i.d. otteremo $$M_{\overline{X}}(t) = \mathbb{E}\left[ e^{t \overline{X}} \right] = \mathbb{E}\left[ e^{t (X_1 + ... + X_n)/n} \right] = \mathbb{E}\left[ \prod_{i=1}^{n} e^{(t/n)X_i} \right] =  \prod_{i=1}^{n} M_{X_i}(t/n) = \left[ M_X(t/n) \right] \; \; \square$$

### Esempio - Normale
Siano $X_1, ..., X_n$ un campio di normali $N(\mu, \sigma^2)$.
La funzione generatrice dei momenti della media campionaria sarà quindi
$$\begin{align*}
M_{\overline{X}}(t)
&= \left[ M_X(t/n) \right]^n\\
&= \left[ \mathbb{E}\left[ e^{(t/n) \cdot N(\mu, \sigma^2)} \right] \right]^n\\
&= \left[ \exp{\left( \mu (t/n) + \sigma^2 \frac{(t/n)^2}{2}\right)} \right]^n\\
&= \exp{\left( n \left( \mu (t/n) + \sigma^2 \frac{(t/n)^2}{2}\right) \right)}\\
&= \exp{\left( \mu t + (\sigma^2/n) \frac{t^2}{2} \right)}
\end{align*}$$

Perciò $\overline{X}$ ha una distribuzione $N(\mu, \sigma^2/n)$.

Osserviamo dal [[#Teorema 5 2 6 - Proprietà importanti|teorema 5.6.2]] che la distribuzione normale che descrive $\overline{X}$ ha appunto parametri $\mathbb{E}\left[\overline{X}\right]$ e $\text{Var}(\overline{X})$.

-------------------
## Teorema 5.2.9. - Convolution formula
Siano $X, Y$ due v.a. **cpntinue**, **indipendenti** e con *pdf* relativamente $f_X(x)$ e $f_Y(y)$.
Sia $Z = X + Y$, allora la sua pdf sarà
$$f_Z(z) = \int_{-\infty}^{\infty} f_X(w) f_Y(z-w) \, dw$$

#### Proof
Sia $W = X$.
Il *Jacobiano* della trasformazione da $(X, Y)$ a $(Z, W)$ sarà 1.
Infatti avremo che $Z = g_1(X,Y) = X+Y$ e $W = g_2(X,Y) = X$ con **inverse**
$$\begin{align*}
X = g^{-1}_1(Z, W) &= W\\
Y = g^{-1}_2(Z, W) &= Z-W
\end{align*}$$

Infatti
$$J = \left\vert
\begin{array}{c}
\dfrac{\partial}{\partial Z} W & \dfrac{\partial}{\partial W} W\\
\dfrac{\partial}{\partial Z} Z-W & \dfrac{\partial}{\partial W} Z-W
\end{array}
\right\vert
= -1$$

Perciò la distribuzione congiunta di $Z,W$ risulterà essere $$f_{Z,W}(z, w) = f_{X,Y}(w, z-w) \vert J \vert = f_{X}(w)f_Y(z-w)$$
Per ottenere la distribuzione marginale di $Z$ basta fissare un valore di $z$ e integrare su $w$
$$f_Z(z) = \int_{-\infty}^{\infty} f_{Z, W}(z, w) \, dw = \int_{-\infty}^{\infty} f_X(w)f_Y(z-w) \, dw \;\; \square$$

### Esempio - somma di v.a. di Cauchy
Sia il campionamento $Z_1, ..., Z_n$ campionati da una $\text{Cauchy}(0,1)$.
Ricordiamo che $$\text{Cauchy}(x_0, \gamma) = \frac{1}{\pi} \frac{\gamma}{(x- x_0)^2 + \gamma^2}$$ perciò $$Z_i \sim \text{Cauchy}(0,1) = \frac{1}{\pi} \frac{1}{x^2 + 1}$$
[[Distribuzioni#Somma di Cauchy indipendinti|Sappiamo]] che la somma di $n$ $\text{Cauchy}(0,1)$ è ancora una $\text{Cauchy}(0, n)$.
$$Z = \sum_{i=1}^{n}Z_i \sim \text{Cauchy}(0,n) \implies f_Z(z) = \frac{1}{n\pi}\frac{1}{(z/n)^2 + 1}$$
Dato che $\overline{Z} = g(Z) = Z/n$, e dato $Z = g^{-1}(\overline{Z}) = n\overline{Z}$, avremo che la distribuzione della media campionaria $\overline{Z}$ è <u>ancora</u> una $\text{Cauchy}(0,1)$!
Infatti $$f_{\overline{Z}}(z) = f_Z(g^{-1}(z)) \cdot \left\vert \frac{d}{dz} g^{-1}(z) \right\vert = f_Z(nz) \cdot n = \cancel{n}\frac{1}{\cancel{n}\pi}\frac{1}{\left( \cfrac{\cancel{n}z}{\cancel{n}}\right)^2 + 1} = \frac{1}{\pi}\frac{1}{z^2 + 1} = \text{Cauchy}(0,1)$$

