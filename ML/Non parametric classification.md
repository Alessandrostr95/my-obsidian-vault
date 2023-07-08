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

## Kernel density estimation: Parzen windows
Supponiamo che la regione $\mathcal{R}(x)$ sia un **ipercubo** $d$-dimensionale di lato $h$ e centrato in $x$.
Tale regione avrà quindi volume $V = h^d$.
Consideriamo come [[Nonparametric Regression#^0e14e6|funzione kernel]] la **Parzend window** definita come
$$\forall \mathbf{z} = (z_1, ..., z_d) \in \mathbb{R}^d\;\;  \kappa(\mathbf{z}) = \begin{cases}
1 &\text{if } \vert z_i \vert \leq \frac{1}{2}\\\\
0 &\text{otherwise}
\end{cases}$$

Come conseguenza, per ogni punto $x' \in \mathbb{R}^d$ avremo che $$\kappa\left(\frac{x-x'}{h}\right) = 1 \iff x' \in \mathcal{R}(x)$$
Indichiamo con $K$ il numero di elementi presenti nell'ipercubo $\mathcal{R}(x)$, ovvero $$K = \sum_{i = 1}^{n} \kappa\left(\frac{x-x_i}{h}\right)$$

Perciò possiamo calcolare la [[#^e8d899|densità]] $p(x)$ come $$p(x) \approx \frac{1}{nV}\sum_{i=1}^{n}\kappa\left(\frac{x-x_i}{h}\right) = \frac{K}{nh^d}$$

![[ML_09_2.png]]


Ci sono però due contro derivanti dall'utilizzo di questo metodo:
1. per valori di $h$ troppo piccoli possono esserci delle **discontinuità** della distribuzione $p$.
2. tutti gli elementi $x_i \in \mathbf{X}$ presenti nella regione $\mathcal{R}(x)$ hanno esattamente lo **stesso peso**, sarebbe ideale che punti più marginali rispetto al centro $x$ abbiano un peso minore.

Per risolvere questo problema possiamo usare una funzione kernel **smoothed**.
È desiderabile avere una funzione kernel $\kappa(x)$ tale che
1. sia una **densità** $$\int\kappa(x) \,dx = 1$$
2. abbia **media 0** $$\int x\kappa(x) \,dx = 0$$

 ![[ML/img/ML_09_3.png]]

Tra queste possiamo considerare per esempio la **gaussiana**, ovvero $$\kappa_h(x) = \frac{1}{\sqrt{2\pi}h} e^{-\tfrac{x^2}{2h^2}}$$

Perciò calcoliamo la nostra densità $p$ come
$$p(x) \approx \frac{1}{nV}\sum_{i=1}^{n}\kappa\left(\frac{x-x_i}{h}\right) = \frac{1}{nh^d}\sum_{i=1}^{n}\kappa_h\left(x-x_i\right)$$

![[ML/img/ML_09_4.png]]

![[ML/img/ML_09_5.png]]

![[ML/img/ML_09_6.png]]

![[ML/img/ML_09_7.png]]



