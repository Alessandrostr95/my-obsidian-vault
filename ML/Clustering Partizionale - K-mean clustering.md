Dato un insieme di elementi $\mathbf{X} = (\mathbf{x}_1, ..., \mathbf{x}_n) \subseteq \mathbb{R}^d$ e un intero $K$ vogliamo derivare i [[Clustering#^39c993|prototipi]] $\mathbf{m}_1, ..., \mathbf{m}_K$ tali che minimizzino la seguente funzione costo
$$J(\mathbf{r}, \mathbf{m}_1, ..., \mathbf{m}_k) = \sum_{i=1}^{n}\sum_{j=1}^{K}r_{ij} \cdot \Vert \mathbf{x}_i - \mathbf{m}_j \Vert^2$$

Possiamo trovare i migliori prototipi in maniera **iterativa** nel seguente modo:
ad ogni step definiamo
$$r_{ij} = \mathbb{1} \big\lbrace j = \arg \min_{1 \leq \ell \leq K} \Vert \mathbf{x}_i - \mathbf{m}_\ell \Vert^2 \big\rbrace$$

Dopodiché troviamo i migliori valori di $\mathbf{m}_j$ in maniera analitica
$$\frac{\partial J}{\partial \mathbf{m}_j} = 2 \sum_{i=1}^{n} r_{ij} \cdot (\mathbf{x}_i - \mathbf{m}_j) = 0 \implies \mathbf{m_j}^* =\frac{\sum_{i=1}^{n} r_{ij} \cdot \mathbf{x}_i}{\sum_{i=1}^{n} r_{ij}}$$

![[ML/img/ML_14_1.png]]

![[ML/img/ML_14_2.png]]

![[ML/img/ML_14_3.png]]

![[ML/img/ML_14_4.png]]

# How to choose $K$
Un modo intuitivo per scegliere il miglior $K$ è quello di fare **validazione**.
Ovvero eseguo l'algoritmo per diversi valori, e ne valuto la qualità della soluzione.

Esistono due modi per misurare la qualità di un clustering:
1. tramite la **distanza media** dei punti dai rispettivi prototipi.
2. la [[Verosimiglianza|log-verosimiglianza]] degli elementi rispetto alle **misture** risultanti dal modello.

Osservare che al crescere di $K$ ovviamente la qualità del modello aumenta, tendendo per all'**overfitting**.