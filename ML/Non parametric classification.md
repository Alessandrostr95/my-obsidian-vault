## Introduzione #todo

-----
# Approccio non parametrico
Nell'approccio non parametrico si cerca di derivare la probabilità $p(C_k \vert x)$ **direttamente dai dati**, senza stimare alcun parametro.

## Histogram
Questo è uno degli approcci più semplici.
Partizioniamo il **dominio** $\mathcal{X}$ in $m$ **intervalli** $m$-dimensionali tutti di dimensione $\Delta$, detti **bins** o **bucket**.
Dato un qualsiasi elemento $x$ e il bin a cui appartiene $B(x)$, la probabilità $P_x$ di campionare dal trainig set $\mathbf{X}$ un elemento dal bin $B(x)$, sarà $$P_x = \frac{\vert B(x) \vert}{\vert \mathbf{X} \vert} = \frac{n(x)}{n}$$ dove $n(x) = \vert B(x) \vert$.
Questa probabilità è **discreta**, se invece volessimo stimare una **densità** per un qualsiasi punto continuo, allora possiamo farlo nel seguente modo
$$f_\mathbf{x}(x) = \frac{P_x}{\Delta}$$

![[ML/img/ML_09_1.png]]


# Density Estimators
Sia il nostro dataset $\mathbf{X} = \lbrace x_1, ...,x_n \rbrace$ i cui elementi sono distribuiti secondo una **densità ignota** $p$.

Sia un qualsiasi elemento $x$, e sia $\mathcal{R}(x)$ una regione di $\mathbb{R}^d$ che contiene $x$.
Allora la probabilità che un item sia nella regione $\mathcal{R}(x)$ può esse calcolato come $$P_x = \int_{\mathcal{R}(x)}p(z) \,dz$$
Per regioni **sufficientemente piccole** possiamo approssimare $$P_x \approx V \cdot p(x)$$dove $V$ è il **volume** della regione $\mathcal{R}(x)$.

Invece, la probabilità che $k$ elementi del dataset siano presenti nella regione $\mathcal{R}(x)$ può essere calcolata con la [[Distribuzioni#Binomiale|binomiale]] $$\binom{n}{k}P_x^k(1-P_x)^{n-k}$$
La probabilità $P_x$ può anche essere vista come il valore atteso della **frazione di punti del dataset** presenti nella regione $\mathcal{R}(x)$.
Perciò possiamo approssimare anche $$P_x \approx \frac{k}{n}$$

**Uguagliando** le due approssimazioni di $P_x$ avremo che $$\frac{k}{n} \approx P_x \approx V \cdot p(x)$$
$$\implies p(x) \approx \frac{k}{V n}$$
Abbiamo quindi ottenuto un modo per **stimare** la densità ignota $p(x)$ senza alcun parametro. ^e8d899

Esistono due approcci per stimare $p$
1. **Kernel density estimation**: fissare un valore di $V$ e derivare $k$ dai dati.
2. **K-nearest neighbor**: fisso $k$ e derivo il valore di $V$.
Può essere dimostrato per entrambi i metodi, sotto condizioni ragionevoli, la stima tende alla reale densità $p$, al crescere di $n \to \infty$.

# [[Kernel density estimation - Parzen windows]]

# [[K-nearest neighbor]]
