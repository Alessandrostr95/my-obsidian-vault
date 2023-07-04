L'idea della **Linear Discriminant Analysis** (**LDA**) è quella di cercare una **proiezione lineare** degli elementi del dataset $\mathbf{X} \in \mathbb{R}^D$ in un **sottospazio** che sia il più **linearmente separabile possibile**.

L'approccio più base è fornito dal **FIsher Linear Discriminant** (**Discriminante lineare di Fisher**) in cui gli elementi del dataset vengono proiettati su una **retta** (o spazio di dimensione 1) per mezzo di una **trasformazione lineare** del tipo $$y = \mathbf{w} \cdot \mathbf{x} = \mathbf{w}^T \mathbf{x}$$ dove $\mathbf{w}$ è un vettore $D$-dimensionale che corrisponde alla **direzione** della proiezione.

![[ML/img/ML_06_2.png]]

![[ML/img/ML_06_3.png]]

Per esempio con due classi $C_1, C_2$, fissiamo una certa soglia $\tilde{y}$ e assegnamo ogni punto $\mathbf{x}$ alla classe $C_1$ se e solo se la sua proiezione supera la soglia $$\mathbf{x} \in C_1 \iff \mathbf{w}^T \mathbf{x}= y > \tilde{y}$$ altrimenti lo assegnamo alla classe $C_2$.

## Deriving $\mathbf{w}$ in binary case
I punti **medi** delle due classi sono
$$\mathbf{m}_1 = \frac{1}{\vert{C_1\vert}}\sum_{\mathbf{x} \in C_1} \mathbf{x}$$
$$\mathbf{m}_2 = \frac{1}{\vert{C_2\vert}}\sum_{\mathbf{x} \in C_2} \mathbf{x}$$


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


## Refinement
