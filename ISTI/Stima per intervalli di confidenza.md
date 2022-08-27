# Stima per intervalli di confidenza
* * *

## Definizione Intervallo di confidenza
Sia un [[Random Sample#Random Sample|campione]] $X_1, ..., X_n$ da una popolazione con [[Distribuzioni Multivariate#Joint PDF|densità]] $f_X(x \vert \theta)$, dipendenti da un parametro $\theta$ <u>sconosciuto</u>.
Siano due [[Random Sample#^46a026|statistiche]] $T_1, T_2$ tali che
1. $T_1(\mathbf{X}) \leq T_2(\mathbf{X})$ per ogni campione $\mathbf{X} \in \mathbb{R}^n$
2. $P(T_1(\mathbf{X}) \leq \theta \leq T_2(\mathbf{X})) = 1 - \alpha$

Allora l'**intervallo** $\left[ T_1(\mathbf{X}), T_2(\mathbf{X}) \right]$ si dice **intervallo di confidenza con grado di confidenza** $1 - \alpha$ per $\theta$.

--------------------------------
## Stima della media di una Normale con varianza nota
Sia un campione [[Distribuzioni#Normale|normale]] $X_1, ..., X_n \sim N(\mu, \sigma^2)$ con $\mu$ **sconosciuto** e $\sigma$ **noto**.

Consideriamo l'[[Hypothesis Testing#^137feb|ipotesi nulla]] $$H_0: \mu = \mu_0$$ e [[Hypothesis Testing#^7a2fae|ipotesi alternatica]] $$H_1: \mu \neq \mu_1$$

Dalla [[CLT - Central Limit Theorem#CLT - Central Limit Theorem|teorema del limite centrale]] sappiamo che **sotto ipotesi nulla** $H_0$ $$Z = \frac{\overline{X}_n - \mu_0}{\sigma/\sqrt{n}} \sim N(0,1)$$
Pericò dato un generico $k>0$ avremo  che $$P(-k \leq Z \leq k) = \int_{-k}^{k}\frac{1}{\sqrt{2\pi}} e^{-x^2/2} \, dx = \Phi(k) - \Phi(-k)$$
Per **simmetria** di una normale standardizzata avremo che $$\Phi(k) - \Phi(-k) = 2\int_{0}^{k}\frac{1}{\sqrt{2\pi}}e^{-x^2/2} \, dx = 2 \Phi^*(k)$$ dove per comodità indichiamo con $$\Phi^*(k) = \int_0^{k}\frac{1}{\sqrt{2\pi}}e^{-x^2/2} \, dx$$
Osserivamo che $$-k \leq Z \leq k \iff -k \leq \frac{\overline{X} - \mu_0}{\sigma/\sqrt{n}} \leq k \iff \overline{X} - \frac{k\sigma}{\sqrt{n}} \leq \mu_0 \leq \overline{X} + \frac{k\sigma}{\sqrt{n}}$$
Ovvero $$P(-k \leq Z \leq k) = P(T_1(\mathbf{X}) \leq \mu \leq T_2(\mathbf{X})) = 2 \Phi^*(k)$$ dove $$T_1(\mathbf{X}) = \overline{X} - \frac{k\sigma}{\sqrt{n}}$$ e $$T_2(\mathbf{X}) = \overline{X} + \frac{k\sigma}{\sqrt{n}}$$

Perciò se volessimo che tale porbabilità sia pari a $1-\alpha$ basterà risolvere l'equazione $$2\Phi^{*}(k) = 1-\alpha$$ $$k = \Phi^{*-1}\left(\frac{1-\alpha}{2}\right)$$
Per esempio, nel nostro caso, ponendo $k$ pari al quantile $\phi_{1 - \alpha/2}$ otterremo un intervallo di confidenza con probablità $$P(-\phi_{1-\alpha/2} \leq Z \leq \phi_{1-\alpha/2}) = (1-\alpha/2) - (1 - (1-\alpha/2)) = 1 - \alpha$$
Se volessimo un grado di confidenza del $95\%$ basta verificare che $$1-\alpha = 0.95 \implies \alpha/2 = 0.025 \implies 1 -\alpha/2 = 0.975$$
Basterà consultare la [[quantili.pdf|tabella dei quantili]] della normale standard, e verificare che $$\phi_{1-\alpha/2} = \phi_{0.975} = 1.96$$ 

In conclusione $$P\left(\overline{X} - \frac{1.96\sigma}{\sqrt{n}} \leq \mu_0 \leq \overline{X} + \frac{1.96\sigma}{\sqrt{n}}\right) \approx 0.95$$ ovvero se $\mu_0$ appartiene all'**intervallo aleatorio** $\left[ T_1, T_2\right]$  allora possiamo dire con una confidenza del $95\%$ che il reale valore della media $\mu$ è $\mu_0$.

### Osservazione importante
Importante osservare che possono esistere svariati intervalli $\left[ a,b\right]$ la cui probabilità $$P(\theta_0 \in \left[a,b \right]) = 1 - \alpha$$
È però preferibile trovare sempre l'intervallo con **ampiezza** $b-a$ più piccolo possibile.

Nel caso di distribuzioni **simmetriche**, come la normale standard, è possibile dimostrare che quello di ampiezza minima è sempre quello simmetrico e centrato in zero, del tipo $\left[ -k, k \right]$ (per $k \in \mathbb{R}^+$).

----------------------------
## Stima della media di una Normale con varianza sconosciuta
Sia un campione [[Distribuzioni#Normale|normale]] $X_1, ..., X_n \sim N(\mu, \sigma^2)$ con $\mu$ e $\sigma$ **sconosciuti**.

Consideriamo le ipotesi $$H_0: \mu = \mu_0; \; H_1: \mu \neq \mu_1$$
Non conoscendo $\sigma$ potremmo pensare di **stimarlo**, per esempio con la [[Random Sample#Varianza campionaria|carianza campionaria]] $S = \sqrt{S^2}$.

Purtroppo però la statistica $$T = \frac{\overline{X} - \mu_0}{S/\sqrt{n}}$$ non segue una [[Distribuzioni#Normale Standard|normale standardizzata]] $N(0,1)$, nemmeno sotto ipotesi nulla $H_0$.

Sappiamo però che sotto ipotesi nulla $H_0$, la statistica $T$ segue una [[Distribuzioni#Distribuzione t di Stundet|distribuzione t di Student]] con $n-1$ *gradi di libertà*.

Perciò, scegliendo due valori $t_1 \leq t_2$, avremo che $$t_1 \leq T \leq t_2 \iff \underbrace{\overline{X} - t_2\frac{S}{\sqrt{n}}}_{T_1} \leq \mu_0 \leq \underbrace{\overline{X} - t_1\frac{S}{\sqrt{n}}}_{T_2}$$ [[quantili.pdf|tabella dei quantili]] della
Per via della **simmetria** della distribuzione *t* di Student sappiamo che l'intervallo di ampiezza minima è quello **simmetrico**, ovvero con $t_1 = - t_2$.
Perciò consultiamo la [[quantili.pdf|tabella dei quantili]] della distribuzion *t* di Student e poniamo $t_2 = \phi_{1-\alpha/2}$.

Quindi $$P(T \in \left[ - \phi_{1-\alpha/2}, \phi_{1-\alpha/2}\right]) = P(\mu_0 \in \left[ T_1, T_2 \right]) = 1 - \alpha$$


Supponiamo di avere $n=10$ e di volere un *grado di confidenza* di $1 - \alpha = 95\%$.
Consultando la [[quantili.pdf|tabella dei quantili]] avremo che
$$\begin{align*}
1-\alpha &= 0.95\\
\implies \alpha/2 &= 0.025\\
\implies 1-\alpha/2 &= 0.975\\
\implies t_2 &= \phi_{1-\alpha/2} = \phi_{0.975} = 2.262
\end{align*}$$

-------------------------
## Stima tramite MLE
Prendiamo un campione $X_1, ..., X_n$ dipendente da un parametro $\theta$ <u>sconosciuto</u>, e consideriamo le ipotesi $$H_0: \theta = \theta_0;\; H_1: \theta \neq \theta_0$$
Sia $\hat\theta_{ML}$ il [[Stimatore di Massima Verosimiglianza#Stimatore di Massima Verosimiglianza - MLE|MLE]] per $\theta$.
Sappiamo che per campioni grandi (asintoticamente) avremo che $$\sqrt{nI(\hat\theta_{ML})} \cdot (\hat\theta_{ML} - \theta_0) \sim N(0,1)$$ (vedi [[Stimatore di Massima Verosimiglianza#Proprietà asintotiche molto importanti|proprietà asintotiche dei MLE]]).

Perciò definendo la statistica $$Z = \sqrt{nI(\hat\theta_{ML})} \cdot (\hat\theta_{ML} - \theta_0)$$ possiamo eseguire la stima per intervalli di confidenza come in precedenza.

Infatti $$P(-\phi_{1-\alpha/2} \leq Z \leq \phi_{1-\alpha/2}) = P\left(\hat\theta_{ML}-\frac{\phi_{1-\alpha/2}}{\sqrt{nI(\hat\theta_{ML})}} \leq \theta_0 \leq \hat\theta_{ML}+\frac{\phi_{1-\alpha/2}}{\sqrt{nI(\hat\theta_{ML})}}\right) = 1-\alpha$$

----------------------------
## Stima di $p$ per una Binomiale
[DA FINIRE...]

