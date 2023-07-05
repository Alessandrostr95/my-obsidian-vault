L'idea della **Linear Discriminant Analysis** (**LDA**) è quella di cercare una **proiezione lineare** degli elementi del dataset $\mathbf{X} \in \mathbb{R}^D$ in un **sottospazio** che sia il più **linearmente separabile possibile**.

L'approccio più base è fornito dal **Fisher Linear Discriminant** (**Discriminante lineare di Fisher**) in cui gli elementi del dataset vengono proiettati su una **retta** (o spazio di dimensione 1) per mezzo di una **trasformazione lineare** del tipo $$y = \mathbf{w} \cdot \mathbf{x} = \mathbf{w}^T \mathbf{x}$$ dove $\mathbf{w}$ è un vettore $D$-dimensionale che corrisponde alla **direzione** della retta in cui si fa la proiezione. 

![[ML/img/ML_06_2.png]]

![[ML/img/ML_06_3.png]]

Per esempio con due classi $C_1, C_2$, fissiamo una certa soglia $\tilde{y}$ e assegnamo ogni punto $\mathbf{x}$ alla classe $C_1$ se e solo se la sua proiezione supera la soglia $$\mathbf{x} \in C_1 \iff \mathbf{w}^T \mathbf{x}= y > \tilde{y}$$ altrimenti lo assegnamo alla classe $C_2$.

## Deriving $\mathbf{w}$ in binary case
I punti **medi** delle due classi sono
$$\mathbf{m}_1 = \frac{1}{\vert{C_1\vert}}\sum_{\mathbf{x} \in C_1} \mathbf{x}$$
$$\mathbf{m}_2 = \frac{1}{\vert{C_2\vert}}\sum_{\mathbf{x} \in C_2} \mathbf{x}$$ ^2868fa


Una misura della **separabilità** delle di una proiezione può essere la differenza tra le proiezioni dei due punti medi, ovvero $$m_2 - m_1 = \mathbf{w}^T\mathbf{m}_2 - \mathbf{w}^T\mathbf{m}_1 = \mathbf{w}^T(\mathbf{m}_2 - \mathbf{m}_1)$$

Secondo questo criterio di separabilità, è desiderabile trovare quel vettore di parametri $\mathbf{w}$ che **massimizza** la distanza $m_2 - m_1$.

```ad-attention
Perché consideriamo $m_2 - m_1$ e non $\vert m_2 - m_1 \vert$?
```


Osserviamo che la **lunghezza** del vottere $\mathbf{w}$ non cambia la differenza $m_2 - m_1$, perciò esistono **infinite soluzioni** lungo la retta perpendicolare a quella in cui si fa la proiezione.
Quindi, dato che ci interessa trovare una soluzione, consideriamo i soli **vettori unitari**, ovvero tali che $$\Vert \mathbf{w} \Vert_2 = \mathbf{w}^T \mathbf{w} = 1$$

Perciò, il nostro problema di ottimizzazione può essere espresso come un problema di **linear programming**
$$\begin{align}
\max_{\mathbf{w} \in \mathbb{R}^D} \;\;&\mathbf{w}^T(\mathbf{m}_2 - \mathbf{m}_1)\\
s.t. \;\;&\mathbf{w}^T\mathbf{w} = 1\\
\end{align}$$

È possibile trasformare il problema di massimizzazione, nella ricerca del **punto stazionario** della funzione $$\mathcal{L}(\mathbf{w},\lambda) = \mathbf{w}^T(\mathbf{m}_2 - \mathbf{m}_1)+ \lambda(1 - \mathbf{w}^T\mathbf{w})$$Il parametro $\lambda$ è anche noto come **moltiplicatore lagrangiano**.

A questo punto è più semplice risolvere il problema in maniera **analitica**, tramite il calcolo delle derivate parziali.
$$\frac{\partial}{\partial \mathbf{w}}\mathcal{L}(\mathbf{w}, \lambda) = \mathbf{m}_2 - \mathbf{m}_1 - 2\lambda\mathbf{w}$$
$$\frac{\partial}{\partial \lambda}\mathcal{L}(\mathbf{w}, \lambda) = 1-\mathbf{w}^T\mathbf{w}$$

E questi si annullano quando 
$$\frac{\partial}{\partial \mathbf{w}}\mathcal{L}(\mathbf{w}, \lambda) = \mathbf{0} \iff \mathbf{w} = \frac{\mathbf{m}_2 - \mathbf{m}_1}{2\lambda}$$

Per quanto riguarda $$\frac{\partial}{\partial \lambda}\mathcal{L}(\mathbf{w}, \lambda) = 1-\mathbf{w}^T\mathbf{w} =  0$$ dobbiamo sostituire $\mathbf{w}$ col valore precedente, ottenendo quindi 
$$1 - \frac{(\mathbf{m}_2 - \mathbf{m}_1)^T(\mathbf{m}_2 - \mathbf{m}_1)}{4\lambda^2} = 0$$
$$\lambda = \frac{\sqrt{(\mathbf{m}_2 - \mathbf{m}_1)^T(\mathbf{m}_2 - \mathbf{m}_1)}}{2} = \frac{\Vert \mathbf{m}_2 - \mathbf{m}_1 \Vert_2}{2}$$

Quindi il vettore ottimo risulterà essere $$\mathbf{w}^* = \frac{\mathbf{m}_2 - \mathbf{m}_1}{\Vert \mathbf{m}_2 - \mathbf{m}_1 \Vert_2}$$

Come possiamo osservare purtroppo non è detto che con tale proiezione $\mathbf{w}^*$ otteniamo un iperpiano con una buona **sparsifiazione**
![[ML/img/ML_06_2.png]]


## Refinement - Fisher Criterion
Un'altra misura di separabilità (*più ragionevole*) è la **dispersione** delle proiezioni.
Ovvero, sarebbe ideale trovare la retta in cui le proiezioni dei punti di una stessa classe sono **meno dispersi possibili**.

Vogliamo perciò massimare una funzione che:
1. cresce al crescere della "*separazione*" tra le classi, e ^bc1e67
2. decresce al diminuire della dispersione dei delle proiezioni dei punti. ^65347b

Sia la **within-class variance** (o **varianza intra-classe**) delle proiezioni di una classe $C_i$ definita come $$s_i^2 = \sum_{\mathbf{x} \in C_i}(\mathbf{w}^T\mathbf{x} - m_i)^2$$ dove $m_i$ è la proiezione del [[#^2868fa|punto medio]] $\mathbf{m}_i$.

Data una **direzione** $\mathbf{w}$, il **Fisher Criterion** $J(\mathbf{w})$ è definito come il rapporto tra lo scarto quadratico delle proiezioni dei punti medi, e la somma delle varianze intra-classe.
In altri termini $$J(\mathbf{w})=\frac{(m_2-m_1)^2}{s_1^2+s_2^2}$$ ^18dee7

Osserviamo che $J$ rispetta i criteri [[#^bc1e67|1]] e [[#^65347b|2]], infatti:
1. **cresce** la crescere della separazione delle classi ($(m_2-m_1)^2$).
2. **decresce** rispetto alla dispersione delle proiezioni dei punti ($s_1^2+s_2^2$).

Siano $\mathbf{S}_1, \mathbf{S}_2$ le **within-class covariance matrices** (le **matrici di covarianza intra-classe**), definite come $$\mathbf{S}_i = \sum_{\mathbf{x} \in C_i} (\mathbf{x} - \mathbf{m}_i)(\mathbf{x} - \mathbf{m}_i)^T$$

Possiamo quindi rappresentare la varianza intra-classe della classe $i$ nel seguente modo $$s_i^2 = \sum_{\mathbf{x} \in C_i}(\mathbf{w}^T\mathbf{x} - m_i)^2 = \mathbf{w}^T\mathbf{S}_i\mathbf{w}$$

Indichiamo quindi con $\mathbf{S}_W = \mathbf{S}_1 + \mathbf{S}_2$ la **total within-class covariance matrix**, e con $$\mathbf{S}_B = (\mathbf{m}_2 - \mathbf{m}_1)^T(\mathbf{m}_2 - \mathbf{m}_1)$$ la **between-class covariance matrix** (la **matrice di covarianza inter-classe**).

Possiamo quindi esprimere il [[#^18dee7|Fisher Criterion]] $J(\mathbf{w})$ in forma matriciale nel seguente modo $$J(\mathbf{w}) = \frac{(m_2-m_1)^2}{s_1^2+s_2^2} = \frac{\mathbf{w}^T\mathbf{S}_B\mathbf{w}}{\mathbf{w}^T\mathbf{S}_W\mathbf{w}}$$

Tale discriminante è massimizzato quando $$\frac{\partial}{\partial\mathbf{w}}J(\mathbf{w}) = \mathbf{0}$$
Questo gradiente si può risolvere analiticamente, con $$\mathbf{w}^* = (\mathbf{S}_1 + \mathbf{S}_2)^{-1}(\mathbf{m}_2 - \mathbf{m}_1)$$

## Choosing a threshold
Un approccio possibile è il seguente:
1. assumere che $p(y \vert C_i)$ è una [[Distribuzioni#Normale|gaussiana]], e stiamarla per [[Stimatore di Massima Verosimiglianza|verosimiglianza]].
2. con la formula di bayes stimare $p(C_i \vert y)$.
3. identificare la threshold $\tilde{y}$ che minimizza l'**odds** $O(C \vert y)$.

Quindi, per verosimiglianza abbiamo che $$\mu_i = \frac{1}{\vert C_i \vert}\sum_{\mathbf{x} \in C_i}\mathbf{w}^T\mathbf{x}$$ e $$\sigma_i^2 = \frac{1}{\vert C_i \vert - 1}\sum_{\mathbf{x} \in C_i}(\mathbf{w}^T\mathbf{x} - \mu_i)^2$$

Dopodiché, applicando bayes $$p(C_i \vert y) \propto p(y \vert C_i)p(C_i) \approx p(y \vert C_i)\frac{|C_i|}{|C_1| + |C_2|} \propto \vert C_i \vert \cdot e^{- \tfrac{(y-\mu_i)^2}{2\sigma^2_i}}$$

Per finire, definiamo la threshold $$\tilde{y} = \arg\min_{y \in \mathbb{R}}\frac{\vert C_1 \vert \cdot p(y \vert C_1)}{\vert C_2 \vert \cdot p(y \vert C_2)}>1$$
