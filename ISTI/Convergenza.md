## Convergenza in probabilità
Una sequenza di v.a. $X_1, X_2, ...$ **converga in probabilità** alla a.v. $X$ se per ogni $\varepsilon > 0$ $$\lim_{n \to \infty}P(\vert X_n - X\vert < \varepsilon) = 1$$
### Teorema Debole dei Grandi Numeri - Weak LLN
Siano $X_1, X_2, ...$ v.a. **i.i.d.** con media $\mu$ e varianza **finita** $\sigma^2 < \infty$.
Sia $$\overline{X}_n = \dfrac{\sum_{i=1}^n X_i}{n}$$ la [[Random Sample#Media campionaria|media campionaria]] dei primi $n$ elementi.
Allora per ogni $\varepsilon > 0$ $$\lim_{n \to \infty}P(\vert \overline{X}_n - \mu \vert < \varepsilon) = 1$$ ovvero  $\overline{X}_n$ **converge in distribuzione** a $\mu$.

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