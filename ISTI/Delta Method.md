# Teorema - Metodo Delta
Sia la v.a. $X_n$ tale che $X_n \xrightarrow{p} \mu$ e $\sqrt{n}(X_n - \mu) \xrightarrow{d} N(0, \sigma^2)$.
Sia inoltre la funzione $g$ *differenziabile* in un intorno di $\mu$.
Allora avremo che
$$g(X_n) \xrightarrow{p} g(\mu)$$ $$\sqrt{n}(g(X_n) - g(\mu)) \xrightarrow{d} N(0, \sigma^2 \left[ g'(\mu) \right]^2  )$$

### Dimostrazione
Per il primo sviluppo di Taylor avremo per un $x$ sufficientemente *"vicino"* a $\mu$
$$g(x) = g(\mu) + g'(\mu)(x-\mu) + o(\vert x - \mu \vert)$$
$$\implies g(x) - g(\mu) = g'(\mu)(x-\mu) + o(\vert x - \mu \vert)$$
Per per $n \to \infty$ abbiamo che $X_n \to \mu$, perciò
$$\sqrt{n}(g(X_n) - g(\mu)) = g'(\mu)\sqrt{n}(X_n - \mu) + o(\vert \sqrt{n}(X_n - \mu )\vert)$$

Se $X_n$ tende a $\mu$ in probabilità allora anche $(X_n - \mu)$ tende a 0 in probabilità, perciò $$\sqrt{n}(g(X_n) - g(\mu)) = g'(\mu)\sqrt{n}(X_n - \mu)$$
$$\implies \frac{\sqrt{n}(g(X_n) - g(\mu))}{g'(\mu) \sigma} = \frac{\sqrt{n}(X_n - \mu)}{\sigma} \xrightarrow{d} N(0,1)$$
Per l'ultimo passaggio è stato applicato il [[CLT - Central Limit Theorem]].

Possiamo quindi concludere che $\sqrt{n}(g(X_n) - g(\mu))$ [[Convergenza#Convergenza in Distribuzione|converge in distribuzione]] ad una normale $N(0, \sigma^2 \left[ g'(\mu) \right]^2)$ $\square$.

----------------
### Esempio I
Supponiamo di avere un [[Random Sample#Random Sample|random sample]] $X_1, ..., X_n$ con media $\mu$, varianza $\sigma^2$ e media campionaria $\overline{X}$.
Supponiamo inoltre di voler studiare la distribuzione $\dfrac{1}{\overline{X}}$.

Ponendo $g(x) = 1/x$ avremo che
$$\sqrt{n}\left(\frac{1}{\overline{X}} - \frac{1}{\mu}\right) = \sqrt{n}\left(g(\overline{X}) - g(\mu)\right) \xrightarrow{d} N(0, \sigma^2/\mu^4)$$

------------------------------
## Proprietà utili
Sia $X_n \xrightarrow{p} \mu$.
Allora avremo che
$$g(X_n) \xrightarrow{d} g(\mu) + g'(\mu)(X_n - \mu)$$
Calcolando la media, *asintoticamente* otteremo
$$\begin{align*}
\mathbb{E}\left[ g(X_n) \right]
&= \mathbb{E}\left[ g(\mu) + g'(\mu)(X_n - \mu) \right]\\
&= g(\mu) + g'(\mu)\mathbb{E}\left[(X_n - \mu)\right]\\
&= g(\mu) + g'(\mu)(\underbrace{\mathbb{E}\left[X_n\right]}_{= \mu} - \mu)\\
&= g(\mu)
\end{align*}$$
e
$$\begin{align*}
\text{Var}(g(X_n))
&= \mathbb{E}\left[ \right(g(X_n) - \mathbb{E}\left[ X_n \right] \left)^2 \right]\\
&= \mathbb{E}\left[ (g(X_n) - g(\mu))^2 \right]\\
&= \mathbb{E}\left[ (g'(\mu)(X_n - \mu))^2 \right]\\
&= \left[g'(\mu)\right]^2 \cdot \mathbb{E}\left[ (X_n - \mu)^2 \right]\\
&= \left[g'(\mu)\right]^2 \cdot \text{Var}(X)
\end{align*}$$

-------------------------
### Esempio II
Supponiamo di avere $X_1, ..., X_n$ v.a. **iid** *Bernoulliano* di parametro $p$.
Un interessante parametro che spesso si studia è il rapporto di efficienza $p/(1-p)$.

Per la [[Convergenza#Teorema Debole dei Grandi Numeri - Weak LLN|legge dei grandi numeri]] sappiamo che possiamo *stimare* il parametro $p$ tramite le osservazioni $X_1, ..., X_n$, semplicemente tramite la media campionaria $$\hat{p} = \frac{1}{n}\sum_{i=1}^{n}X_i$$ (questo perché appunto la media di una bernoulliana è appunto $p$).
Ci chiediamo se $\dfrac{\hat{p}}{1-\hat{p}}$ è un buono **stimatore** di $p/(1-p)$.

Per esempio, poniamo la funzione $$g(p) = \frac{p}{1 - p}$$ con derivata prima $$g'(p) = \frac{1}{(1-p)^2}$$

Cerchiamo di calcolare la **varianza del nostro stimatore**
$$\begin{align*}
\text{Var}(g(\hat{p}))
&= \left[ g'(\mathbb{E}\left[ \hat{p} \right]) \right]^2 \cdot \text{Var}(\hat{p})\\
&= \left[ g'(p) \right]^2 \cdot \text{Var}(\hat{p})\\
&= \left[ g'(p) \right]^2 \cdot \frac{\text{Var}(p)}{n}\\
&= \frac{1}{(1-p)^4} \cdot \frac{p(1-p)}{n}\\
&= \frac{p}{n(1-p)^3}
\end{align*}$$

----------------------------
# Metodo Delta del Secondo Ordine
Sia $X_n$ una sequenza di v.a. tale che $\sqrt{n}(X_n - \mu) \xrightarrow{d} N(0, \sigma^2)$.
Per una data funzione $g$ e per uno specifico valore $\mu$, supponiamo che $g'(\mu) = 0$ e che $g''(\mu) \neq 0$.

Allora $$n(g(X_n) - \mu) \xrightarrow{d} \sigma^2 \frac{g''(\mu)}{2} \chi_1^2$$