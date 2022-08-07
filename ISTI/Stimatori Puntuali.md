# Stimatori puntuali
Uno **stimatore puntuale** è una qualsiasi statistica $T(X_1, ..., X_n)$ usata per stimare il valore di uno o più parametri **sconosciuti**.

Più in generale, supponiamo di avere una distribuzione $$f(\mathbf{X} \vert \theta_1, ...,  \theta_k)$$ con $\theta_1, ..., \theta_k \in \Theta$ dei parametri dai quali la distribuzione dipende.

Esistono due filosofie di pensiero su come poter stimare i parametri:
- **Approccio Frequentista** [da fare]
- **Approccio Bayesiano** [da fare]

Quello che in generale si vuole è che uno stimatore $T(\mathbf{X})$ sia il più *"simile"* possibile al parametro da stimare $\theta$.
In base ai valori che il campione $\mathbf{X}$ può assumere, la stima $T$ a sua volta assume valori differenti.
Perciò per *"simile"* si intende che a prescindere dal campione, $T$ assume dei valori concentrati in un intorno di $\theta$ con alta probabilità.

Abbiamo quindi bisogno di *quantificare* la qualità di uno stimatore, per decidere quale è meglio di altri.

Una prima proprietà desiderabile per uno stimatore è che esso sia **non distorto** o **un-biased**, ovvero che la sua media corrisponda al parametro desiderato $$\mathbb{E}_{\theta} \left[ T(\mathbf{X}) \right] = \theta, \; \forall \theta \in \Theta$$
Perciò a prescindere dal valore del parametro, uno stiamtore non distorto in media assumerà sempre il valore di $\theta$ (ovviamente calcolando la media tenendo conto del valore che $\theta$ assume se necessario, ovvero calcolando $\mathbb{E}_{\theta}$).

Un'altra proprietà desiderata è che la stiame $T(\mathbf{X})$ sia il più possibile **concentrata** attorno al suo valore atteso.
Perciò si vuole minimizzare la sua varianza $$\text{minimize Var}_{\theta}(T(\mathbf{X}))$$

Più in generale uno stiamtore è detto **UMVUE** (**uniformly minimum variance unbiased estimator**) se è **un-biased** e se per ogni $\theta$ possibile abbia una varianza più piccola rispetto a tutti gli altri stimatori (ovvero la minimizza).

Altri termini utili sono:
- il **bias** o **distorsione** $$\text{bias}(T) = \mathbb{E}_{\theta} \left[ T(\mathbf{X}) \right] - \theta$$ Ovviamente se $\text{bias}(T) = 0$ allora $T$ è non distorto.
- L'**errore quadratico medio** $$\text{MSE}(T) = \mathbb{E}\left[ \left(T(\mathbf{X}) - \theta \right)^2 \right] = \text{Var}_{\theta}(T(\mathbf{X})) + \text{bias}(T)^2$$
### Esempio
Richiamando il [[Random Sample#Teorema 5 2 6 - Proprietà importanti]], si può dimostrare che $\overline{X}$ e $S^2$ sono entrambi **stimatori non distorti** per $\mu$ e $\sigma^2$, ovvero

$$\text{bias}(\overline{X}) = 0$$

$$\text{bias}(S^2) = 0$$

Invece sappiamo che la v.a. $(n-1)S^2/\sigma^2 \sim \chi^2_{n-1}$ ([[Distribuzioni#Chi Quadro#Proprietà|vedi]]) con media $n-1$ e varianza $2(n-1)$.
Perciò $$\text{Var}(S^2) = \text{Var}(\sigma^2 \chi^2_{n-1}/(n-1)) = \left(\frac{\sigma^2}{n-1}\right)^2 \cdot \text{Var}(\chi^2_{n-1}) = 2\frac{\sigma^4}{n-1}$$ e quindi $$\text{MSE}(S^2) = \text{Var}(S^2) + \underbrace{\text{bias}(S^2)}_{=0} = 2\frac{\sigma^4}{n-1}$$

Consideriamo ora un nuovo stimatore della varianza, un pochino più distorto $$\hat{\sigma}^2 = \frac{\sum_{i=1}^{n}(X_i - \overline{X})^2}{n} = \frac{n-1}{n}S^2$$

Calcoliamo la sua media $$\mathbb{E}\left[ \hat{\sigma}^2 \right] = \mathbb{E}\left[ \frac{n-1}{n}S^2 \right] = \frac{n-1}{n}\sigma^2$$ e il suo bias $$\text{bias}(\hat{\sigma}^2) = \sigma^2 - \frac{n-1}{n}\sigma^2 = -\frac{\sigma^2}{n}$$
Calcoliamo ora l'errore di tale stimatore
$$\begin{align*}
\text{MSE}(\hat{\sigma}^2)
&= \text{Var}(\hat{\sigma}^2) - \frac{\sigma^2}{n}\\
&= \left( \frac{n-1}{n} \right)^2 \text{Var}(S^2) - \frac{\sigma^2}{n}\\
&= 2\sigma^4\frac{n-1}{n^2} - \frac{\sigma^2}{n}\\
&\leq 2\sigma^4\frac{n-1}{n^2}\\
&= 2\frac{\sigma^4}{n}\frac{n-1}{n}\\
&< 2\frac{\sigma^4}{n-1}\underbrace{\frac{n-1}{n}}_{<1}\\
&< 2\frac{\sigma^4}{n-1} = \text{MSE}(S^2)
\end{align*}$$ ovvero abbiamo dimostrato che benché $\hat{\sigma}^2$ sia distorto, esso ha un errore minore di quello precedente, ovvero $\text{MSE}(\hat{\sigma}^2) < \text{MSE}(S^2)$.
Ciò significa che $\hat{\sigma}^2$ è mediamente più concentrato attorno al valore del parametro da stimare.

Per fortuna entrambi gli errori tendono a $0$ al crescere di $n$, perciò entrambi tendono asintoticamente al $\sigma^2$.

Tale proprietà di convergenza asintotica è detta anche **consistenza**

## Consistenza di uno stimatore
Uno stimatore è detto **consistente** se [[Convergenza#Convergenza in probabilità|tende in probabilità]] al parametro interessato $T(\mathbf{X}) \xrightarrow{p} \theta$, ovvero se $$\lim_{n \to \infty}P(\vert T(\mathbf{X}) - \theta \vert < \varepsilon) = 1$$ per ogni $\varepsilon > 0$.

Perciò uno stimatore consistente non è per forza non distorto, ma lo si può considerare come **"asintoticamente non distorto"**.

-------------------
# Metodi frequentisti per trovare stimatori
Vedi:
- [[Metodo dei Momenti]]
- [[Massima Verosimiglianza]]