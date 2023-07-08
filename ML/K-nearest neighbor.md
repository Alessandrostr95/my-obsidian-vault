In questo caso vogliamo approssimare la [[Non parametric classification#^e8d899|densità]] $p(x)$ fissando il numero di vicini a $k$ ed espandendo la regione $\mathcal{R}(x)$ centrata in $x$ finché non ricopre almeno $k$ elementi del dataset $\mathbf{X}$.

L'approssimazione può quindi essere fatta come 
$$p(x) \approx \frac{k}{nV} = \frac{k}{n \cdot c_d \cdot r_k(x)}$$
dove
- $c_d$ è il volume di una **sfera unitaria** $d$-dimensionale.
- $r_k(x)$ è la distanza tra $x$ e il suo $k$-esimo elemento di $\mathbf{X}$ più vicino. Questa distanza può essere vista come il **raggio** della più piccola sferza centrata in $x$ che ricopre $k$ elementi del dataset.

```ad-note
Mentre nel [[Kernel density estimation - Parzen windows|kernel density estimation]] si considera come regione $\mathcal{R}(x)$ un **ipercubo** $d$-dimensionale nel **KNN** si considera invece una **sfera** $d$-dimensionale, la quale la si fa espandere finché non ricopre $k$ elementi di $\mathbf{X}$. 
```



