## Trasformazioni Bivariate
Sia $(X,Y)$ una v.a. bivariata con distribuzione nota.
Consideriamo ora una seconda v.a. bivariata $(U,V)$ con $U \sim g_1(X,Y)$ e $V \sim g_2(X,Y)$.

Sia un qualsiasi sottoinsieme $B \subseteq \mathbb{R}^2$.
Allora avremo che $(U,V) \in B$ se $(X,Y) \in A$, dove
$$A := \{ (x,y) : (g_1(x,y), g_2(x,y)) \in B\}$$
Perciò
$$P((U,V) \in B) = P((X,Y) \in A)$$
ovvero la distribuzione di $(U,V)$ è completamente definita dalla distribuzione di $(X,Y)$.

Inoltre, fissati dei valori $(U,V) = (u,v)$ definiamo l'insieme $A_{u,v} = \{ (x,y) : g_1(x,y) = u \land g_2(x,y) = v\}$.
Quindi
$$f_{U,V}(u,v) = P(U=u, V=v) = P((X,Y) \in A_{u,v}) = \sum_{(x,y) \in A_{u,v}} f_{X,Y}(x,y)$$

### Esempio - somme di Poisson indipendeti
Siano $X,Y$ due v.a. di *Poisson* **indipendenti** di parametri rispettivamente $\theta$ e $\lambda$.
Ovvero
$$f_X(x) = \theta^x\frac{e^{-\theta}}{x!}$$
$$f_Y(y) = \lambda^y\frac{e^{-\lambda}}{y!}$$
Per ogni $x,y \in \mathbb{N}$.

Avremo che la loro [[Distribuzioni Multivariate#Joint PMF|probabilità congiunta]] sarà
$$f_{X,Y}(x,y) = f_X(x)f_Y(y) = \frac{\theta^xe^{-\theta}}{x!}\frac{\lambda^ye^{-\lambda}}{y!}$$

Definiamo ora $U = X+Y$ e $V=Y$, ovvero $g_1(x,y) = x+y$ e $g_2(x,y)=y$.

Descriviamo l'insieme $B$ dei possibili valori di $u,v$.
Sicuramente $v$ può assumere come valore un qualsiasi valore intero non negativo.
Invece, dato che $v=y$, avremo che $u = x+y = x+v$.
Perciò $u$ può assumere un valore non minore di $v$.
Quindi $$B := \{(u,v) \in \mathbb{N}^2 : u \geq v \geq 0\}$$

Invece, per ogni $(u,v) \in B$, l'unica coppia $(x,y)$ che soddisfa $u = g_1(x,y)$ e $v = g_2(x,y)$ è definita come
$$\begin{cases}
y = v\\
x = u - v
\end{cases}$$
Perciò avremo che $A_{u,v}$ sarà sempre composto dal singolo elemento $(u-v,v)$.
$$A_{u,v} := \{(u-v, v)\}$$
Perciò avremo che la densità conginuta di $(U,V)$ sarà
$$f_{U,V}(u,v) = f_{X,Y}(u-v, v) = \frac{\theta^{(u-v)}e^{-\theta}}{(u-v)!}\frac{\lambda^ve^{-\lambda}}{v!}$$
per ogni $u \geq v \geq 0$ interi.

Calcoliamo ora le densità marginali di $U$ e $V$.
Dato che $v \leq u$ allora avremo che
$$\begin{align*}
f_U(u)
&= \sum_{v=0}^{u}\frac{\theta^{(u-v)}e^{-\theta}}{(u-v)!}\frac{\lambda^ve^{-\lambda}}{v!}\\
&= e^{-(\theta+\lambda)}\sum_{v=0}^{u}\frac{\theta^{(u-v)}}{(u-v)!}\frac{\lambda^v}{v!}\\
&= \frac{e^{-(\theta+\lambda)}}{u!}\sum_{v=0}^{u}\frac{u!}{v!(u-v)!}\theta^{(u-v)}\lambda^v\\
&= \frac{e^{-(\theta+\lambda)}}{u!}\sum_{v=0}^{u}\binom{u}{v}\theta^{(u-v)}\lambda^v\\
&= \frac{e^{-(\theta+\lambda)}}{u!}(\theta + \lambda)^u
\end{align*}$$
ovvero una Poisson di parametro $(\theta + \lambda)$.
Possiamo concludere che $$U \sim \text{Poisson}(\theta + \lambda)$$ da cui il seguente teorema.

## Teorema somme di Poisson indipendenti
Siano $X \sim \text{Poisson}(\theta)$ e $Y \sim \text{Poisson}(\lambda)$ **indipendenti**.
Allora 
$$X+Y \sim \text{Poisson}(\theta + \lambda)$$

-----------------
## Jacobiano di una trasformazione
Sia $(X,Y)$ è una v.a. bivariata **continua** con [[Distribuzioni Multivariate#Joint PDF|densità congiunta]] $f_{X,Y}(x,y)$.
Sia $(U,V)$ una **trasformazione** di $(X,Y)$ tale che $U \sim g_1(X,Y)$ e $V \sim g_2(X,Y)$.
È possibile esprimere $f_{U,V}(g_1(x,y),g_2(x,y))$ in termini di $f_{X,Y}(x,y)$.

Siano i supporti 
$$A := \{(x,y) : f_{X,Y}(x,y) > 0\}$$
$$B := \{ (u,v) : u = g_1(x,y) \land v = g_2(x,y)\}$$
Osserviamo che in questo caso abbiamo che $$B \equiv \mathbf{g}(A)$$ dove $\mathbf{g} = (g_1, g_2)$.

Per semplicità assumiamo che per ogni $(x,y) \in A$ esiste un **unico** $(u,v) \in B$ tale che $(u,v) = (g_1(x,y), g_2(x,y))$, ovvero che $\mathbf{g}$ sia **biunivoca**.

In questa maniera possiamo definire una funzione invera $\mathbf{h} = (h_1, h_2)$ tale che $$\mathbf{h}(B) \equiv A$$ ovvero
$$x = h_1(u,v) \equiv g_1^{-1}(g_1(x,y))$$
$$y = h_2(u,v) \equiv g_2^{-1}(g_2(x,y))$$
Il ruolo che giocava la derivata nelle [[Trasformazioni Univariate]], nel caso bivariato (o multiariato in generale) è giocato dal **Jacobiano della trasformazione**, ovvero dal **determinante del gradiente**.
$$
J = \left\vert \begin{array}{cc}
\dfrac{\partial x}{\partial u} & \dfrac{\partial x}{\partial v}\\
\dfrac{\partial y}{\partial u} & \dfrac{\partial y}{\partial v}
\end{array} \right\vert
= \dfrac{\partial x}{\partial u} \cdot \dfrac{\partial y}{\partial v} - \dfrac{\partial x}{\partial v} \cdot \dfrac{\partial y}{\partial u}
$$
Andando a sostituire opportunamente avremo che
$$\begin{align*}
\dfrac{\partial x}{\partial u} &= \dfrac{\partial}{\partial u} h_1(u, v)\\
\dfrac{\partial x}{\partial v} &= \dfrac{\partial}{\partial v} h_1(u, v)\\
\dfrac{\partial y}{\partial u} &= \dfrac{\partial}{\partial u} h_2(u, v)\\
\dfrac{\partial y}{\partial v} &= \dfrac{\partial}{\partial v} h_2(u, v)
\end{align*}$$
Perciò avremo che la [[Distribuzioni Multivariate#Joint PDF|densità marginale]] di $(U,V)$ sarà $0$ al di fuori del suo support $B \equiv \mathbf{g}(A)$, mentre dentro sarà
$$f_{U,V}(u,v) = f_{X,Y}\left( h_1(u,v), h_2(u,v) \right) \cdot \vert J \vert$$

### Prodotto di variabili Beta
Siano $X \sim \text{Beta}(\alpha, \beta)$ e $Y \sim \text{Beta}(\alpha + \beta, \gamma)$ v.a. **indipendenti** con [[pdf]]
$$f_X(x) = \frac{\Gamma(\alpha+\beta)}{\Gamma(\alpha)\Gamma(\beta)}x^{\alpha - 1}(1-x)^{\beta - 1}$$
$$f_Y(y) = \frac{\Gamma(\alpha+\beta+\gamma)}{\Gamma(\alpha+\beta)\Gamma(\gamma)}y^{\alpha + \beta - 1}(1-y)^{\gamma - 1}$$
Per indipendenza la loro [[Distribuzioni Multivariate#Joint PDF|densità congiunta]] sarà quindi
$$f_{X,Y}(x,y) = \frac{\Gamma(\alpha+\beta)}{\Gamma(\alpha)\Gamma(\beta)}x^{\alpha - 1}(1-x)^{\beta - 1}\frac{\Gamma(\alpha+\beta+\gamma)}{\Gamma(\alpha+\beta)\Gamma(\gamma)}y^{\alpha + \beta - 1}(1-y)^{\gamma - 1}$$
per ogni $x,y \in (0,1)$.

Consideriamo le strasformazioni $U = XY$ e $V=X$.
Avremo quindi $g_1(x,y) = xy = u$ e $g_2(x,y) = x = v$

Osservare che dato che $y < 0$ allora $xy < x$, ovvero $u < v$.
Perciò il supporto di $(U,V)$ sarà
$$\mathbf{g}(A) = B = \{ (u,v) : 0 < u < v < 1\}$$
Osservare che la funzione $\mathbf{g} = (g_1, g_2)$ è **biettiva** in $A$, perciò invertibile, con 
$$x = g^{-1}_2(u, v) = v$$
$$y = g^{-1}_1(u, v) = \frac{u}{x} = \frac{u}{v}$$
Perciò il Jacobiano risulta essere
$$
J = \left\vert \begin{array}{cc}
\dfrac{\partial x}{\partial u} & \dfrac{\partial x}{\partial v}\\
\dfrac{\partial y}{\partial u} & \dfrac{\partial y}{\partial v}
\end{array} \right\vert
= \left\vert \begin{array}{cc}
\dfrac{\partial v}{\partial u} & \dfrac{\partial v}{\partial v}\\
\dfrac{\partial (u/v)}{\partial u} & \dfrac{\partial (u/v)}{\partial v}
\end{array} \right\vert
= \left\vert \begin{array}{cc}
0 & 1\\
\dfrac{1}{v} & -\dfrac{u}{v^2}
\end{array} \right\vert
= -\frac{1}{v}
$$

Pericò la densità congiunta di $(U,V)$ risulta essere
$$f_{U,V}(u,v) = f_{X,Y}(v, u/v) = \frac{\Gamma(\alpha + \beta + \gamma)}{\Gamma(\alpha)\Gamma(\beta)\Gamma(\gamma)}v^{\alpha-1}(1-v)^{\beta - 1}\left(\frac{u}{v}\right)^{\alpha + \beta - 1}\left( 1 - \frac{u}{v}\right)^{\gamma - 1} \frac{1}{v}$$
Certamente la distribuzione marginale di $V$ sarà $\text{Beta}(\alpha, \beta)$.
Però anche la distribuzione di $U$ risulterà essere una Beta, infatti
$$\begin{align*}
f_U(u)
&= \int_{u}^{1} f_{U,V}(u, v) \, dv\\
&= \int_{u}^{1} \frac{\Gamma(\alpha + \beta + \gamma)}{\Gamma(\alpha)\Gamma(\beta)\Gamma(\gamma)}v^{\alpha-1}(1-v)^{\beta - 1}\left(\frac{u}{v}\right)^{\alpha + \beta - 1}\left( 1 - \frac{u}{v}\right)^{\gamma - 1} \frac{1}{v} \, dv\\
&= \frac{\Gamma(\alpha + \beta + \gamma)}{\Gamma(\alpha)\Gamma(\beta)\Gamma(\gamma)}\int_{u}^{1} (1-v)^{\beta - 1} u^{\alpha - 1} \left(\frac{u}{v}\right)^{\beta}\left( 1 - \frac{u}{v}\right)^{\gamma - 1} \frac{1}{v} \, dv\\
&= \frac{\Gamma(\alpha + \beta + \gamma)}{\Gamma(\alpha)\Gamma(\beta)\Gamma(\gamma)}u^{\alpha - 1}\int_{u}^{1} (1-v)^{\beta - 1} \left(\frac{u}{v}\right)^{\beta-1}\left( 1 - \frac{u}{v}\right)^{\gamma - 1} \left(\frac{u}{v^2}\right) \, dv\\
&= \frac{\Gamma(\alpha + \beta + \gamma)}{\Gamma(\alpha)\Gamma(\beta)\Gamma(\gamma)}u^{\alpha - 1}\int_{u}^{1} \left(\frac{u}{v} -u \right)^{\beta - 1}\left( 1 - \frac{u}{v}\right)^{\gamma - 1} \left(\frac{u}{v^2}\right) \, dv\\
\end{align*}$$
Facendo un cambio di variabile, e ponendo $w = \dfrac{u}{v}\frac{1}{1-u}$ otterremo che $dw = -\dfrac{u}{v^2(1-u)}$ e con le opportune sostituzioni
$$\begin{align*}
f_U(u)
&= \frac{\Gamma(\alpha + \beta + \gamma)}{\Gamma(\alpha)\Gamma(\beta)\Gamma(\gamma)}u^{\alpha - 1}(1-u)^{\beta + \gamma - 1}\int_{0}^{1} w^{\beta - 1} (1 - \beta)^{\gamma - 1} \, dw\\
&= \frac{\Gamma(\alpha + \beta + \gamma)}{\Gamma(\alpha)\Gamma(\beta)\Gamma(\gamma)}u^{\alpha - 1}(1-u)^{\beta + \gamma - 1} \frac{\Gamma(\beta)\Gamma(\alpha)}{\Gamma(\beta + \gamma)}\\
&= \frac{\Gamma(\alpha + \beta + \gamma)}{\Gamma(\alpha)\Gamma(\beta + \gamma)}u^{\alpha - 1}(1-u)^{\beta + \gamma - 1}
\end{align*}$$
ovvero $U = XY \sim \text{Beta}(\alpha, \beta + \gamma)$.


-------------------------------

### Somma e differenza di Normali
Siano le v.a. $X,Y \sim N(0, 1)$ **indipendenti**, con pdf
$$f_X(x) = \frac{1}{\sqrt{2\pi}}e^{-x^2/2}$$
$$f_Y(y) = \frac{1}{\sqrt{2\pi}}e^{-y^2/2}$$
La loro densità congiunta risulta quindi essere
$$f_{X,Y}(x,y) = \frac{1}{2\pi} e^{- (x^2+y^2)/2}$$
Il supplemento di $(X,Y)$ risulta quindi essere $A \equiv \mathbb{R}^2$.

Consideriamo $U = g_1(X,Y) = X+Y$ e $V = g_2(X,Y) = X-Y$.
Osserviamo che il supporto di $(U,V)$ è uguale a $\mathbb{R}^2$, infatti
$$B := \{ (u,v) : u=x+y, v=x-y\} \equiv \mathbb{R}^2$$
Per foruna esiste una unica soluzine per il sistema
$$\begin{cases}
u = x+y\\
v = x-y
\end{cases}$$
ovvero
$$
\left(\begin{array}{cc|c}
1 & 1 & u\\
1 & -1 & v
\end{array}\right)
\rightarrow
\left(\begin{array}{cc|c}
1 & 1 & u\\
0 & -2 & v -u
\end{array}\right)
\rightarrow
\left(\begin{array}{cc|c}
1 & 1 & u\\
0 & 1 & \dfrac{u-v}{2}
\end{array}\right)
\rightarrow
\left(\begin{array}{cc|c}
1 & 0 & \dfrac{u+v}{2}\\
0 & 1 & \dfrac{u-v}{2}
\end{array}\right)
$$
Ciò implica che le due trasformazioni $g_1, g_2$ sono **invertibili**, ovvero
$$x = g_1^{-1}(u, v) = \frac{u+v}{2}$$
$$y = g_2^{-1}(u, v) = \frac{u-v}{2}$$
Possiamo quindi calcoalre la [[Distribuzioni Multivariate#Joint PDF|densità congiunta]] di $f_{U,V}(u,v)$ come
$$f_{U,V}(u,v) = f_{X,Y}(g^{-1}_1(u,v),g^{-2}_1(u,v)) \cdot \vert J \vert$$
dove
$$
J = \left\vert
\begin{array}{cc}
\dfrac{\partial}{\partial u}\dfrac{u+v}{2} & \dfrac{\partial}{\partial v}\dfrac{u+v}{2}\\
\dfrac{\partial}{\partial u}\dfrac{u-v}{2} & \dfrac{\partial}{\partial v}\dfrac{u-v}{2}
\end{array}
\right\vert
= \left\vert
\begin{array}{cc}
\dfrac{1}{2} & \dfrac{1}{2}\\
\dfrac{1}{2} & -\dfrac{1}{2}
\end{array}
\right\vert
= -\frac{1}{2}
$$
perciò
$$\begin{align*}
f_{U,V}(u,v)
&= \frac{1}{4\pi}\exp{\left( -\frac{1}{2} \left[ \frac{(u+v)^2}{4} + \frac{(u-v)^2}{4} \right]\right)}\\
&= \frac{1}{4\pi}\exp{\left( -\frac{1}{2} \left[ \frac{u^2+v^2+2uv}{4} + \frac{u^2 + v^2 - 2uv}{4} \right]\right)}\\
&= \frac{1}{4\pi}\exp{\left( -\frac{1}{2} \left[ \frac{u^2}{2} + \frac{v^2}{2}  \right]\right)}
\end{align*}$$
Scoponendo tale risultato avremo che
$$f_{U,V}(u,v) = \left( \frac{1}{\sqrt{2\pi}\sqrt{2}}e^{-u^2/4} \right) \cdot \left( \frac{1}{\sqrt{2\pi}\sqrt{2}}e^{-v^2/4} \right)$$
Ovvero $U = X+Y \sim N(0, 2)$ e $V = X-Y \sim N(0,2)$, infatti
$$N(\mu, \sigma^2) = \frac{1}{\sigma\sqrt{2\pi}}\exp{\left[-\frac{1}{2}\left(\frac{x-\mu}{\sigma}\right)^2\right]}$$