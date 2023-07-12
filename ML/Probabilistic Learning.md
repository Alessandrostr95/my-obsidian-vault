# Derivare un predittore probabilistico
Fin ora abbiamo assunto che esiste una **funzione** che mette in relazione $\mathcal{X}$ ad $\mathcal{Y}$.
Un altro framework è quello **probabilistico**, dove:
- assumiamo che gli elementi di $\mathcal{X}$ occorrono secondo una distribuzione $p_1(x)$ (generalmente **uniforme**).
- gli elementi di $\mathcal{Y}$ sono distribuiti, rispetto ad ogni elemento $x \in \mathcal{X}$, secondo una **distribuzione condizionata** $p_2(t \vert x)$ (i.e. la probabilità che $t$ sia il target dell'elemento $x$).

Ciò che si vuole è quindi, derivare da $\mathcal{T}$, un algoritmo $A_{\mathcal{T}}$ che dato un input $x \in \mathcal{X}$ computa la distribuzione condizionata $p(t \vert x)$, la quale deve *approssimare* il più possibile la distribuzione $p_2$.

Dato che sarebbe desiderabile ottenere un singolo valore come output (e non una distribuzione), è necessario definire una **strategia di decisione** che sceglie il valore di $t$ più ragionevole dalla distribuzione $p( \;\cdot\; \vert x)$.

# First Approach
Il primo approccio consiste nel definire una **classe** di [[Distribuzioni|distribuzioni]] condizionate $\mathcal{P}$.
Dopodiché cerchiamo di individuare la migliore distribuzione $p^* \in \mathcal{P}$ sfruttando tutta la nostra conoscenza (ovvero $\mathcal{T}$) in accordo ad una **misura** $q$.
Infine, dato un input $x$ applichiamo la distribuzione $p^*( \;\cdot\; \vert x)$ ad ogni valore di $\mathcal{Y}$.

![[ML/img/ML_04_1.png]]


Il problema è quindi quello di definire la classe $\mathcal{P}$.
Come fatto in precedenza, possiamo adottare in [[From Learning to Optimization|approccio parametrico]], ovvero $\mathcal{P}$ è la famiglia di tutte quelle disrtibuzioni condizionate della **stessa forma**, ma che dipendono da un **insieme di parametri** $\pmb{\theta}$.

### Esempio
Per esempio, consideriamo la *classificazione binaria*.
Possiamo modellare $p(t \vert x)$, con $t \in \lbrace 0,1 \rbrace$, come una [[Distribuzioni#Bernoulli|bernoulliana]] di parametro $\pi(x)$.
$$p(t \vert x) = \pi(x)^t(1-\pi(x))^{1-t}$$
Modelliziamo la probabilità di successo $\pi(x)$ come $$\pi(x) = \frac{1}{1+e^{-\sum_{i=1}^d w_ix_i + b}}$$
In questo caso quindi i parametri sono i coefficienti $(b,w_1, ..., w_d)$, rappresentati quindi come un punto nello spazio $\mathbb{R}^{d+1}$.
Definito il modello, si potrebbero inferire i migliori parametri con metodi come il [[Gradient Descent]].


## Misura di qualità $q$
Per inferire il miglior $p \in \mathcal{P}$ ci vuole una **misura** $q(p, \mathcal{T})$ della **qualità** di $p$ rispetto al training set $\mathcal{T}$.
Questa misura $q$ è correlata alla *probabilità* che il nostro dataset $\mathcal{T} = (\mathbf{X}, \mathbf{t})$ è stato generato dalle seguenti ipotesi:
1. $\mathcal{T} = n$, ed ogni elementi $(x_i,t_i)$ è stato campionato in maniera **indipendente**.
2. ogni $x_i \in \mathbf{X}$ è campionato secondo una distribuzione nota $p_1$ (generalmente uniforme).
3. $t_i$ è campionato dalla nostra distribuzione $p( \;\cdot\; \vert x)$.

Perciò sia $$p(\mathcal{T}) = p((\mathbf{X}, \mathbf{t}))$$ la probabilità secondo le **precedenti** assuzioni (indipendenza, $\mathbf{X} \sim p_1$ e $\mathbf{t} \sim p( \;\cdot\; \vert \mathbf{X})$ invece di $\mathbf{t} \sim p_2$).

**Ragionevolmente** la distribuzione migliore $p^*$ di $\mathcal{P}$ è quella che **massimizza la probabilità** $p^*(\mathcal{T})$.
Perciò possiamo usare questa probabilità come misura $q$.

Secondo le nostre assunzioni, possiamo quindi definire $$q(\hat{p}, \mathcal{T}) = \hat{p}(\mathcal{T}) = \prod_{i=1}^{n}\hat{p}(x_i,t_i) = \prod_{i=1}^{n}\hat{p}(t_i \vert x_i) p_1(x_i)$$
Se assumiamo che $p_1$ è **uniforme**, allora $$q(\hat{p},\mathcal{T}) \propto \prod_{i=1}^{n}\hat{p}(t_i \vert x_i)$$
In altri termini, se $\mathcal{T}$ è un **campione indipendente** e **uniforme**, la distribuzione ottima $p^*$ è quella che **massimizza** la [[Distribuzioni Multivariate|distribuzione congiunta]] $$p^*(\mathbf{t} \vert \mathbf{X}) = \prod_{i=1}^{n}p^*(t_i \vert x_i)$$

```ad-note
Trovare il $p^* \in \mathcal{P}$ che massimizza la **distribuzione congiunta** di $\mathcal{T}$ è l'equivalente probabilistico del trovare la funzione $h^* \in \mathcal{H}$ che massimizza il [[Prediction Risk#^9cd1a0|rischio empirico]] $\overline{\mathcal{R}}_{\mathcal{T}}(h^*)$.
```


----------
# Second Approach
Supponiamo di essere in una stanza $\mathcal{P}$, e di chiedere se la nostra squadra di calcio preferita questa serà farà un *pareggio*, *sconfitta* o *vittoria*.

Ogni persona nella stanza fornisce una **distribuzione** $p$ sull'esito $t$ della partita $x$ di questa sera.

Nel [[#First Approach|precedente approccio]] sceglievamo la **persona più affidabile**, ovvero quella che massimizzava la [[#Misura di qualità $q$|misura di qualità]] $q( \;\cdot\;, \mathcal{T})$.

Supponiamo che la persona più affidabile dice che c'è il 100% di probabilità che la squadra vinca, mentre <u>tutti</u> gli altri danno lo 0% di vittoria.
A chi credere? A uno solo molto affidabile, o a tutti quanti?
Se quasi tutti credono in qualcosa, vuol dire che hanno un'informazione mancante al più affidabile. Oppure la persona più affidabile ha un'informazione in più che manca alla massa.

Si può quindi fare una **media pesata** delle diverse distribuzioni, pesandole rispetto alle relative misure di qualità $q$.

$$p^*(\mathbf{t} \vert \mathbf{X}) = \dfrac{\sum_{p \in \mathcal{P}} q(p, \mathcal{T})\cdot p(\mathbf{t} \vert \mathbf{X})}{\sum_{p \in \mathcal{P}} q(p, \mathcal{T})}$$


-----
# [[Verosimiglianza]]
Abbiamo visto che la [[#Misura di qualità $q$|misura di qualità]] $q$ rispetto a una distribuzione condizionata $p$ e un dataset $\mathcal{T}$ può essere definita come la probabilità congiunta $$q(p, \mathcal{T}) = p(\mathcal{T})$$ Ovvero la probabilità di campionare il campione $\mathcal{T}$ assumendo che i campioni siano **uniformi**, **indipendenti** e i target seguono la distribuzione $p$.

Assumiamo che $p$ dipensa dai parametri $\pmb{\theta}$, ovvero $$p(\mathcal{T}) = p_{\pmb{\theta}}(\mathcal{T}) = p(\mathcal{T} \vert \pmb{\theta})$$
Ovvero, **sapendo** che $p$ dipenda dai parametri $\pmb{\theta}$, abbiamo la distribuzione congiunta su $\mathcal{T}$.

La [[Distribuzioni Multivariate|probabilità congiunta]] $p(\mathcal{T} \vert \pmb{\theta})$ è anche nota come [[Verosimiglianza]].
$$\begin{align}
L(\pmb{\theta} \vert \mathcal{T})
&= p(\mathcal{T} \vert \pmb{\theta})\\\\
&= p(\mathbf{X}, \mathbf{t} \vert \pmb{\theta})\\\\
&= \prod_{(x,t) \in \mathcal{T}} p(x,t \vert \pmb{\theta})\\\\
&= \prod_{(x,t) \in \mathcal{T}} p(t \vert x, \pmb{\theta}) \cdot p(x \vert \pmb{\theta})\\\\
&= \left( \prod_{(x,t) \in \mathcal{T}} p(t \vert x, \pmb{\theta}) \right) \cdot \left( \prod_{(x,t) \in \mathcal{T}} p(x \vert \pmb{\theta}) \right)\\\\
&= p(\mathbf{t} \vert \mathbf{X}, \pmb{\theta}) \cdot  p(\mathbf{X} \vert \pmb{\theta})\\\\
(\mathbf{X} \text{ uniforme}) &= p(\mathbf{t} \vert \mathbf{X}, \pmb{\theta}) \cdot  p(\mathbf{X})\\\\
&\propto p(\mathbf{t} \vert \mathbf{X}, \pmb{\theta})
\end{align}$$


Seguendo quindi l'**approccio frequentista** la distribuzione ottima $p^*$ è definita dallo [[Stimatore di Massima Verosimiglianza]] 
$$\begin{align}
\pmb{\theta}^*
&= \arg \max_{\pmb{\theta} \in \Theta} L(\pmb{\theta} \vert \mathcal{T})\\\\
&= \arg \max_{\pmb{\theta} \in \Theta} p( \mathcal{T} \vert \pmb{\theta})\\\\
&\propto \arg \max_{\pmb{\theta} \in \Theta} p(\mathbf{t} \vert \mathbf{X}, \pmb{\theta})\\\\
&= \arg \max_{\pmb{\theta} \in \Theta} \prod_{(x,t) \in \mathcal{T}} p(t \vert x, \pmb{\theta})
\end{align}$$

Generalmente è preferibile considerare la **log-verosimiglianza** $$l(\pmb{\theta} \vert \mathcal{T}) = \log{L(\pmb{\theta} \vert \mathcal{T})}$$
È preferibile perché la produttoria diventa una sommatoria la quale è più semplice da gestire e *derivare*, preservando però lo stesso **stimatore** $\pmb{\theta}^*$.

Infatti, grazie all'applicazione del logaritmo, è più semplice calcolare la derivata della verosimiglianza, rendendo fattibile l'applicazione del [[Gradient Descent]]

$$\begin{align}
\nabla_{\pmb{\theta}} \; l(\pmb{\theta} \vert \mathcal{T}) \rightarrow \dfrac{\partial}{\partial \theta_i} l(\pmb{\theta} \vert \mathcal{T})
&= \dfrac{\partial}{\partial \theta_i} \log{L(\pmb{\theta} \vert \mathcal{T})}\\\\
&= \dfrac{\partial}{\partial \theta_i} \log{ \left( \prod_{(x,t) \in \mathcal{T}} p( t \vert x, \pmb{\theta}) \right)}\\\\
&= \dfrac{\partial}{\partial \theta_i} \log{ \left( \prod_{(x,t) \in \mathcal{T}} p( t \vert x, \pmb{\theta}) \right)}\\\\
&= \dfrac{\partial}{\partial \theta_i} \sum_{(x,t) \in \mathcal{T}} \log{p( t \vert x, \pmb{\theta})}\\\\
&= \sum_{(x,t) \in \mathcal{T}} \dfrac{\partial}{\partial \theta_i} \log{p( t \vert x, \pmb{\theta})}\\\\
\end{align}$$

### Esempio Bernoulli
Supponiamo che di avere un *campione* $X$ di $n$ eventi binari, secondo una [[Distribuzioni#Bernoulli|distribuzione bernoulliana]] di parametro $\phi$ **sconosciuto**.

$$p(x \vert \phi) = \phi^{x}(1-\phi)^{1-x}$$

Avremo
- verosimiglianza $$L(\phi \vert X) = \prod_{i=1}^{n}\phi^{x_i}(1-\phi)^{1-x_i}$$
- log-verosimiglianza $$l(\phi \vert X) = \sum_{i=1}^{n}\left(x_i\ln{\phi} + (1-x_i)\ln{(1-\phi)} \right) = n_1 \ln{\phi} + n_0\ln{(1-\phi)}$$ dove $n_1$ è il numero di successi, ed $n_0 = n - n_1$.

Si può trovare semplicemente il punto di minimo in maniera analitica

$$\frac{d}{d \phi}l(\phi \vert X) = \frac{n_1}{\phi} - \frac{n_0}{1 - \phi} = 0 \implies \phi_{ML} = \frac{n_1}{n_1 + n_0} = \frac{n_1}{n}$$
 

## ML and Overfitting
Osservare che lo stimatore di massima verosimiglianza dipende totalmente dai dati osservati, e quindi tende all'[[From Learning to Optimization#Bias vs Varianza|overfitting]].

Lo stimatore infatti è buono per descrivere il campione osservato, ma troppo specifico per poter utilizzare lo stesso modello su altri dataset.

Perciò in genere quello che si fa è cercare di *limitare* la dipendenza dai dati, introducendo una **funzione di penalità** $P(\pmb{\theta})$ che non dipende dai dati.
Perciò la [[#Misura di qualità $q$|misura di qualità]] $q$ diventa 
$$q(p( \;\cdot\; \vert \pmb{\theta}), \mathcal{T}) = l(\pmb{\theta} \vert \mathbf{X}) - P(\pmb{\theta})$$

Una funzione di penalità comune è $$P(\pmb{\theta}) = \frac{\gamma}{2} \Vert \pmb{\theta} \Vert^2$$ con $\gamma > 0$ detto **parametro di tuning**.
Questo esempio di funzione di penalità penalizza maggiormente i modelli troppo complessi, limitando quindi la possibilità di overfitting.