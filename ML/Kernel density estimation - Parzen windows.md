Supponiamo che la regione $\mathcal{R}(x)$ sia un **ipercubo** $d$-dimensionale di lato $h$ e centrato in $x$.
Tale regione avrà quindi volume $V = h^d$.
Consideriamo come [[Nonparametric Regression#^0e14e6|funzione kernel]] la **Parzend window** definita come
$$\forall \mathbf{z} = (z_1, ..., z_d) \in \mathbb{R}^d\;\;  \kappa(\mathbf{z}) = \begin{cases}
1 &\text{if } \vert z_i \vert \leq \frac{1}{2}\\\\
0 &\text{otherwise}
\end{cases}$$

Come conseguenza, per ogni punto $x' \in \mathbb{R}^d$ avremo che $$\kappa\left(\frac{x-x'}{h}\right) = 1 \iff x' \in \mathcal{R}(x)$$
Indichiamo con $K$ il numero di elementi presenti nell'ipercubo $\mathcal{R}(x)$, ovvero $$K = \sum_{i = 1}^{n} \kappa\left(\frac{x-x_i}{h}\right)$$

Perciò possiamo calcolare la [[Non parametric classification#^e8d899|densità]] $p(x)$ come $$p(x) \approx \frac{1}{nV}\sum_{i=1}^{n}\kappa\left(\frac{x-x_i}{h}\right) = \frac{K}{nh^d}$$

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

