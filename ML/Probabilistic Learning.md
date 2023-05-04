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

Per esempio, consideriamo la *classificazione binaria*.
Possiamo modellare $p(t \vert x)$, con $t \in \lbrace 0,1 \rbrace$, come una [[Distribuzioni#Bernoulli|bernoulliana]] di parametro $\pi(x)$.
$$p(t \vert x) = \pi(x)^t(1-\pi(x))^{1-t}$$
Modelliziamo la probabilità di successo $\pi(x)$ come $$\pi(x) = \frac{1}{1+e^{-\sum_{i=1}^d w_ix_i + b}}$$
In questo caso quindi i parametri sono i coefficienti $(b,w_1, ..., w_d)$, rappresentati quindi come un punto nello spazio $\mathbb{R}^{d+1}$.
Definito il modello, si potrebbero inferire i migliori parametri con metodi come il [[Gradient Descent]].

