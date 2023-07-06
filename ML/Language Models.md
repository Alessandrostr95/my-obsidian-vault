Un **Language Model** è una **distribuzione di probabilità categorica** su un vocabolario di termini (per esempio tutte le parole che appaiono in una collezione di documenti).
Sia quindi $\mathcal{T} = \lbrace t_1, ..., t_n \rbrace$ l'insieme di **termini** (parole) che occorrono in una collezione di documenti $\mathcal{C}$.
Perciò, data la sequenza di parole $w_1, ..., w_k \in \mathcal{T}$, il language model è utilizzato per stimare la probabilità $$P(w_1, ..., w_k \vert \mathcal{C})$$

Un language model può essere derivato a partire da una **distribuzione categorica** dei singoli termini
$$\phi_i = P(t_i \vert \mathcal{C})$$
$$\sum_{i=1}^{n} \phi_i = 1$$

Sia $m_i$ la [[Scoring, term weighting & the vector space model#^433f1d|collection frequency]] del termine $t_i$, ovvero il numero totale di occorrenze di $t_i$ nella collezione $\mathcal{C}$.
Applicando la [[Stimatore di Massima Verosimiglianza|massima verosimiglianza]] possiamo stimare le probabilità $\phi_i$ come $$\phi_i\approx \frac{m_i}{\sum_{k=1}^{n}m_k} = \frac{m_i}{N}$$
Dove $N$ è il numero totale di occorrenze di termini nella collezione $\mathcal{C}$.

```ad-warning
title: Smoothing
Secondo questo modello, la probabilità di un termine $t$ che non appare in $\mathcal{C}$ è *nulla*.
Siccome questa caratteristica è troppo forte e dipende troppo da $\mathcal{C}$ (un termine che non appare mai non significa che non possa apparire in futuro) si applica lo **smoothing**, ovvero si assegna una probabilità piccolissima non nulla a tutti i termini possibili.
```

## Bayesian learning of Language Models
Supponiamo che inizialmente (*a priori*) le probabilità dei termini siano distribuiti secondo una [[Distribuzioni#Distribuzione di Dirichlet|distribuzione di Dirichlet]]
$$p(\phi_1, ..., \phi_n \vert \alpha_1, ..., \alpha_n) = \frac{1}{\Delta(\alpha_1, ..., \alpha_n)} \prod_{i=1}^{n}\phi_i^{\alpha_i - 1}$$

Una volta osservato il corpus $\mathcal{C}$, e avendo ricavato tutte le [[Scoring, term weighting & the vector space model#^433f1d|collection frequency]] $m_1, ..., m_n$, possiamo definire la [[Maximum a Posteriori - Bayesian Approach#^7ee03b|probabilità a posteriori]] $\text{Dir}(\phi_1, ..., \phi_n \vert \pmb{\alpha}')$ dove 
$$\pmb{\alpha}' = (\alpha_1 + m_1, ..., \alpha_n + m_n)$$
Ottenendo quindi $$p(\pmb{\phi} \vert \mathcal{C}, \pmb{\alpha}) = p(\phi_1, ..., \phi_n \vert \pmb{\alpha}') = \frac{1}{\Delta(\alpha_1 + m_1, ..., \alpha_n + m_n)} \prod_{i=1}^{n}\phi_i^{\alpha_i + m_i - 1} = \text{Dir}(\pmb{\phi} \vert \pmb{\alpha}')$$ dove $\pmb{\phi} = (\phi_1, ..., \phi_n)$.

Per ricavare la singola probabilità a posteriori $\hat{\phi}_i$ basterà calcolare
$$\begin{align}
\hat{\phi}_i
&= p(t_i \vert \mathcal{C}, \pmb{\alpha})\\
%&= p(t_i \vert \pmb{\phi}) \cdot p(\pmb{\phi} \vert \mathcal{C}, \pmb{\alpha})\\
&= \int p(t_i \vert\pmb{\phi}) \cdot p(\pmb{\phi} \vert \mathcal{C}, \pmb{\alpha}) \,d\pmb{\phi}\\
&= \int \phi_i \cdot \text{Dir}(\pmb{\phi} \vert \pmb{\alpha}') \,d\pmb{\phi}\\
&= \mathbb{E}_{\pmb{\alpha}'}\left[ \phi_i \right]
\end{align}$$
Questa media può esse calcolata in maniera diretta come $$\hat{\phi}_i = \frac{\alpha_i'}{\sum_{k=1}^{n}\alpha_k'} = \frac{\alpha_i + m_i}{\sum_{k=1}^{n}\alpha_k + m_k} = \frac{\alpha_i + m_i}{\alpha + N}$$
Dato che i parametri $\alpha_i > 0$ nelle distribuzioni di Dirichlet, è quindi impossibile ottenere una probabilità nulla.
Questo è anche noto come **smoothing di Dirichlet**.

```ad-note
title: Non informative prior
Nel caso in cui inizialmente $\alpha_i = \alpha^*$ per ogni $i$, non avremo alcuna informazione a priori.
In questo caso la probabilità a posteriori sarà quindi $$\hat{\phi}_i = \frac{m_i + \alpha^*}{\alpha^* n + N}$$
```

## Classification with Language Model
Il language model può essere esteso anche nel caso di $k \geq 2$ classi.
Consideriamo per semplicità due classi $C_1, C_2$.

Possiamo usare i language model per **classificare** un documento $d$.
%% Come [[#Bayesian learning of Language Models|visto prima]] possiamo stimare la probabilità $\phi_i(C_k) = p(t_i \vert C_k)$, ovvero la probabilità di un termine $t_i$ all'interno della collezione $C_k$.%%

Usando la **formula di Bayes** posso calcola la probabilità a posteriori $$p(C_k \vert d) \propto p(d \vert C_k)p(C_k) \;\;\; \forall k=1,2$$

Assegnamo quindi il documento $d$ alla classe $C_k$ che massimizza la precedente probabilità.

[[Naive Bayes Classifier|Vedremo dopo]] come stimare le probabilità $p(d \vert C_k)$ e $p(C_k)$.
