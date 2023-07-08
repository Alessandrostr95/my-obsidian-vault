In questo caso vogliamo approssimare la [[Non parametric classification#^e8d899|densità]] $p(x)$ fissando il numero di vicini a $k$ ed espandendo la regione $\mathcal{R}(x)$ centrata in $x$ finché non ricopre almeno $k$ elementi del dataset $\mathbf{X}$.

L'approssimazione può quindi essere fatta come 
$$p(x) \approx \frac{k}{nV} = \frac{k}{n \cdot c_d \cdot r_k(x)}$$
dove
- $c_d$ è il volume di una **sfera unitaria** $d$-dimensionale.
- $r_k(x)$ è la distanza tra $x$ e il suo $k$-esimo elemento di $\mathbf{X}$ più vicino. Questa distanza può essere vista come il **raggio** della più piccola sferza centrata in $x$ che ricopre $k$ elementi del dataset.

```ad-note
Mentre nel [[Kernel density estimation - Parzen windows|kernel density estimation]] si considera come regione $\mathcal{R}(x)$ un **ipercubo** $d$-dimensionale nel **kNN** si considera invece una **sfera** $d$-dimensionale, la quale la si fa espandere finché non ricopre $k$ elementi di $\mathbf{X}$. 
```

# Classification through kNN
Consideriamo una ipersfera $d$-dimensionale di volume $V_k(x)$ centrata in $x$ e che contiene $k$ elementi del dataset $\mathbf{X}$.

Indichiamo con $k_i$ il numero di elementi di questa sfera che appartengono alla classe $C_i$, e con $n_i$ il numero di elementi del dataset che appartengono a $C_i$.

Possiamo quindi approssimare la probabilità $p(x \vert C_i)$ come segue
$$p(x \vert C_i) \approx \frac{k_i}{n_i \cdot V_k(x)}$$

Analogamente possiamo approssimare $p(C_i)$ con la [[Random Sample#Media campionaria|media campionaria]] $$p(C_i) \approx \frac{n_i}{n}$$

Applicando bayes, avremo quindi
$$\begin{align}
p(C_i \vert x) = \frac{p(x \vert C_i)p(C_i)}{p(x)} \approx \frac{\dfrac{k_i}{n_i \cdot V_k(x)} \cdot \dfrac{n_i}{n}}{\dfrac{k}{n \cdot V_k(x)}} = \frac{k_i}{k}
\end{align}$$

In altri termini, basta assegnare la classe di **maggioranza** tra i $k$ elementi del dataset più vicini al nostro input.

![[ML/img/ML_09_8.png]]

Questo metodo è molto semplice e funziona bene se vengono forniti molti elementi e data una buona metrica di distanza.

Purtroppo funziona male quando la dimensionalità $d$ dello spazio è elevata.
Infatti, quando le dimensioni sono molte i punti risultano sempre essere parecchio **sparsi** e quindi i $k$ punti più vicini all'input potrebbero essere parecchio lontani.
