## Convergenza in probabilità
Una sequenza di v.a. $X_1, X_2, ...$ **converga in probabilità** alla a.v. $X$ se per ogni $\varepsilon > 0$ $$\lim_{n \to \infty}P(\vert X_n - X\vert < \varepsilon) = 1$$
Per convenzione scriveremo che $X_n \xrightarrow{p} X$.

### Teorema Debole dei Grandi Numeri - Weak LLN
Siano $X_1, X_2, ...$ v.a. **i.i.d.** con media $\mu$ e varianza **finita** $\sigma^2 < \infty$.
Sia $$\overline{X}_n = \dfrac{\sum_{i=1}^n X_i}{n}$$ la [[Random Sample#Media campionaria|media campionaria]] dei primi $n$ elementi.
Allora per ogni $\varepsilon > 0$ $$\lim_{n \to \infty}P(\vert \overline{X}_n - \mu \vert < \varepsilon) = 1$$ ovvero  $\overline{X}_n$ **converge in probabilità** a $\mu$.

#### Proof
Basta applicare la [[Disuguaglianza di Chebyschev]].
$$P(\vert \overline{X}_n - \mu \vert \geq \varepsilon) \leq \frac{\text{Var}(\overline{X}_n - \mu)}{\varepsilon^2} = \frac{\text{Var}(\overline{X}_n)}{\varepsilon^2} = \frac{\sigma^2}{n\varepsilon^2}$$
Perciò, fissato un qualsiasi $\varepsilon > 0$ avremo che
$$P(\vert \overline{X}_n - \mu \vert < \varepsilon) = 1 - P(\vert \overline{X}_n - \mu \vert \geq \varepsilon) \geq 1 - \frac{\sigma^2}{n\varepsilon^2} \xrightarrow{n \to \infty} 1 \;\; \square$$

### Teorema 5.5.4
Siano $X_1, X_2, ...$ v.a. **tendono in probabilità** ad $X$, e sia $h$ una funzione continua.
Allora $h(X_1), h(X_2), ...$ convergono il probabilità $h(X)$.

---------------------
## Almost Sure Convergence
Una sequenza di v.a. $X_1, X_2, ...$ **converga almost surely** alla a.v. $X$ se per ogni $\varepsilon > 0$  $$P\left( \lim_{n \to \infty}\vert X_n - X\vert < \varepsilon \right) = 1$$
### Esempio
Consideriamo uno spazio $S = \left[ 0,1 \right]$ con distribuzione unifrome.
Siano le v.a. $X_n(s) = s + s^n$ e $X(s) = s$, con $s \sim US$.
Osserviamo che per ogni $s \in \left[0,1\right)$ $X_n(s)$ converge **quasi sicuramente** a $X$.
Infatti per $n \to \infty$ avremo che $s^n \to 0$ e che quindi $X_n(s) \to s$.

Non è vero però per $s = 1$, infatti per $n \to \infty$ $X_n(1) = 2 \neq 1 = X(1)$.

Però dato che la convergenza avviene in $\left[ 0,1 \right)$ e dato che $P(s \in \left[ 0,1 \right)) = 1$ allora possiamo dire che $X_n$ converge quasi sicuramente a $X$. 


### Teorema Forte dei Grandi Numeri - Strong LLN
Siano $X_1, X_2, ...$ v.a. **i.i.d.** con media $\mu$ e varianza **finita** $\sigma^2 < \infty$.
Sia $\overline{X}_n$ la [[Random Sample#Media campionaria|media campionaria]] dei primi $n$ elementi della serie di v.a.

Allora per ogni $\varepsilon > 0$
$$P\left( \lim_{n \to \infty} \vert \overline{X}_n - \mu \vert < \varepsilon \right) = 1$$
ovvero $\overline{X}_n$ converge *quasi certamente* a $\mu$.

----------------------------
## Convergenza in Distribuzione
Una sequenza di v.a. $X_1, X_2, ...$ **converge in distribuzione** a una v.a. $X$ se $$\lim_{n \to \infty}F_{X_n}(x) = F_X(x)$$ per ogni punto $x$ di continuità di $F_X(x)$.

Come notazione, per dire che *"$X_n$ tende in distribuzione a $X$*" (sempre per $n$ che tende ad infinito) scriveremo $X_n \xrightarrow{d} X$.

### Esempio - Il massimo di Uniformi
Siano $X_1, X_2, ...$ v.a. **i.i.d.** uniformi $U\left(0,1\right)$, e sia $X_{(n)} = \max_{1 \leq i \leq n} X_i$ il massimo valora dei primi $n$ elementi.
Vogliamo studiare a cosa tende in distribuzione $X_{(n)}$.

Per $n \to \infty$ ci aspettiamo che $X_{(n)}$ si avvicina sempre di più ad 1, e siccome $X_{(n)}$ è sempre minore di 1, avremo che per ogni $\varepsilon > 0$
$$P(\vert X_{(n)} - 1 \vert \geq \varepsilon) = P(X_{(n)} \geq 1 + \varepsilon) + P(X_{(n)} \leq 1 - \varepsilon) = 0 + P(X_{(n)} \leq 1 - \varepsilon)$$
Sfruttando ora l'indipendenza avremo che $$P(X_{(n)} < 1 - \varepsilon) = P(\{X_i \leq 1 - \varepsilon : i=1,...,n\}) = (1-\varepsilon)^n$$ il quale tende a $0$ per $n \to \infty$.
Abbiamo quindi dimostrato $X_{(n)}$ tende in **probabilità** ad 1, ovvero $X_{(n)} \xrightarrow{p} 1$.

Poniamo ora $\varepsilon = t/n$
$$P(X_{(n)} \leq 1 - t/n) = (1-t/n)^n \xrightarrow{n \to \infty} e^{-t}$$

Riarrangiando i termini otteremo che $$P(n \cdot (1 - X_{(n)}) \leq t) \rightarrow 1 - e^{-t}$$
ovvero abbiamo dimostrato che la v.a. $n \cdot (1 - X_{(n)})$ tende in distribuzione ad una **[[Distribuzioni#Esponenziale|esponenziale]]** di parametro 1.

### Teorema 5.5.12 (importante)
Se una sequenza $X_1, X_2, ...$ tende in **probabilità** ad una variabile $X$ allora tende anche in **distribuzione**.

### Teorema 5.5.13 (importante)
Una sequenza $X_1, X_2, ...$ tende in **probabilità** ad una **cosante** $\mu$ <u>se e solo se</u> tende a $\mu$ anche in **distribuzione**.

Più formalmente, l'affermazione che $$P(\vert X_n - \mu \vert > \varepsilon) \xrightarrow{n \to \infty} 0 \;\; \forall \varepsilon > 0$$ è **equivalente** a
$$F_{X_n}(x) =
P(X_n \leq x) \xrightarrow{n \to \infty}
\begin{cases}
0 &x < \mu\\
1 &x \geq \mu
\end{cases}$$