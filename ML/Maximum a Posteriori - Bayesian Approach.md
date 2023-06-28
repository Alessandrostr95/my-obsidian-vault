Abbiamo visto [[Probabilistic Learning|due approcci frequentisti]] per definire un modello probabilistico.
In particolare abbiamo visto approcci volti al massimizzare la [[Verosimiglianza]].

Un altro approccio è invece quello **Bayesiano**.
Ovvero noi supponiamo di avere una **conoscenza a priori** sui parametri $\pmb{\theta}$, e in particolare supponiamo che essi siano distribuiti secondo una **distribuzione** $p(\pmb{\theta})$. ^8ea705

Data quindi questa conoscenza a priori, e date le osservazioni del training set $\mathcal{T}$, possiamo cercare di *migliorare* la nostra conoscenza cercando i valori del parametro $\pmb{\theta}$ che massimizza la probabilità a **posteriori** del parametro stesso. ^7ee03b

In altre parole, sfruttando la regola di Bayes $$p(\pmb{\theta} \vert \mathcal{T}) = \frac{p(\mathcal{T} \vert \pmb{\theta})p(\pmb{\theta})}{p(\mathcal{T})}$$ possiamo definire uno **Maximum a Posteriori (MAP) Estimator** come segue  ^fa0dbd
$$\begin{align}
\pmb{\theta}_{MAP}
&= \arg\max_{\theta \in \Theta} p(\theta \vert \mathcal{T})\\\\
&= \arg\max_{\theta \in \Theta} p(\mathcal{T} \vert \theta)  \cdot p(\theta)\\\\
&= \arg\max_{\theta \in \Theta} L(\theta \vert \mathcal{T})  \cdot p(\theta)\\\\
&= \arg\max_{\theta \in \Theta} (l(\theta \vert \mathcal{T}) + \log{p(\theta)})\\\\
&= \arg\max_{\theta \in \Theta} \left(\sum_{(x,t) \in \mathcal{T}} \log{p(t \vert x, \theta)} + \log{p(\theta)}\right)
\end{align}$$

## Esempio Gaussiana
Assumiamo che $\pmb{\theta} \in \mathbb{R}^d$ sia distribuito come una [[CLT - Central Limit Theorem#Densità normale multivariata|gaussiana multivariata]] con **varianza uniforme** (ovvero $\sigma_i = \sigma$ per ogni $i = 1, ..., d$), **covarianza nulla** (ovvero $COV(\theta_i, \theta_j) = 0$ per ogni $i \neq j$) e **centrata** nell'origine.

Avremo quindi $$\mathbb{E} \left[ \pmb{\theta} \right] = \mathbf{0}$$ e $$\Sigma = \sigma^2 \cdot I_d$$

Ovvero la nostra **distribuzione a priori** di $\pmb{\theta}$ sarà $$p(\pmb{\theta}) \sim \mathcal{N}(\pmb{\theta} \vert \mathbf{0}, \sigma^2I_d) = \frac{1}{(2\pi)^{d/2} \cdot \sigma^d} e^{-\tfrac{\Vert \pmb{\theta} \Vert^2}{2\sigma^2}} \propto e^{-\tfrac{\Vert \pmb{\theta} \Vert^2}{2\sigma^2}}$$

Date queste **iptesi** calcoliamo lo stimatore MAP
$$\pmb{\theta}_{MAP} = \arg \max_{\theta \in \Theta} \left( l(\theta \vert \mathcal{T}) + \ln{\left(e^{-\tfrac{\Vert \pmb{\theta} \Vert^2}{2\sigma^2}}\right)} \right) = \max_{\theta \in \Theta} \left( l(\theta \vert \mathcal{T}) - \frac{\Vert \pmb{\theta} \Vert^2}{2\sigma^2} \right)$$

Osservare che tale quantità è equivalente alla [[Probabilistic Learning#Misura di qualità $q$|misura di qualità]] con [[Probabilistic Learning#ML and Overfitting|funzione di penalità]] $P(\pmb{\theta})$ con **parametro di tuning** $\gamma = \dfrac{1}{\sigma^2}$.


------
## Esempio: Beta-Bernoulli
Supponiamo di avere il campione $X$ di $n$ eventi binari distribuiti secondo una [[Distribuzioni#Bernoulli|bernoulliana]] di **parametro sconosciuto** $\phi$.
Ovvero $$p(x = \vert \phi) = \phi^x(1-\phi)^{1-x}$$

Come conoscenza iniziale abbiamo che $\phi$ è distribuito seguendo una [[Distribuzioni#Beta|beta]] $$p(\phi) \sim B(\phi \vert \alpha, \beta) = \frac{\Gamma{(\alpha+\beta)}}{\Gamma{(\alpha)} \cdot \Gamma{(\beta)}} \cdot\phi^{\alpha-1}(1-\phi)^{\beta-1}$$
Applicando la regola di Bayes avremo che $$p(\phi \vert X) = \frac{p(X \vert \phi)p(\phi)}{p(X)} \propto \frac{\Gamma{(\alpha+\beta)}}{\Gamma{(\alpha)} \cdot \Gamma{(\beta)}} \cdot\phi^{\alpha-1}(1-\phi)^{\beta-1} \prod_{i=1}^{n} \phi^x (1-\phi)^{1-x}$$
Il [[#^fa0dbd|MAP]] sarà quel valore di $\phi$ per il quale si **annulla** la derivata

$$\frac{d}{d\phi}\left[ l(\phi \vert \mathcal{T})+\ln{B(\phi \vert \alpha,\beta)} \right] = \frac{n_1}{\phi}-\frac{n_0}{1 -\phi} + \frac{\alpha - 1}{\phi} - \frac{\beta - 1}{1 - \phi} = 0$$

Ovvero quando $$\phi_{MAP}=\frac{n_1+\alpha-1}{n+\alpha+\beta-2}$$