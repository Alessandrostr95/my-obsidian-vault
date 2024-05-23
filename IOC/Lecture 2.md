---
date: 2024-05-09
draft: true
---

Data una matrice $A \in \mathbb{C}^{n \times m}$ 
Esempi di funzioni su matrici
- funzione **element-wise** $$f(A) = f(a_{ij})$$
- funzione verso **scalare** $f: \mathbb{C}^{n \times n} \to \mathbb{R}$, per esempio $$k(A) = \Vert A \Vert \cdot \Vert A^{-1} \Vert$$
- funzione verso **scalare** $f: \mathbb{C} \to \mathbb{C}^{n \times n}$, per esempio $$f(z) = (zI - A)^{-1}$$
- funzioni da matrice a matrice $f: \mathbb{C}^{n \times n} \to \mathbb{C}^{n \times n}$ $$f(A) = (I-A)^{-1}(I + A^2)$$
- se $f$ ha una rappresentazione di potenze convergente, (esempio $\log{(1+t)} = t - t^2/2 + t^3/3 - \dots$, per $|t| < 1$), allora abbiamo la seguente convergenza $\log{(I - A)} = A - A^2/2 + A^3/3 - \dots$ per $\rho(A) < 1$.


Esistono almeno 3 definizioni possibili di funzioni tra matrici, tutte **equivalenti tra loro**.

## Prima definizione - JCF

> **Def.**
> Per ogni $A \in \mathbb{C}^{n\times n}$ $\exists Z, J_1,...,J_\phi$ tali che $Z^{-1} A Z = J = \text{diag}(J_1,...,J_\phi)$ , e $$J_k = J_k(\lambda) = \begin{pmatrix}
> \lambda &1 &0 &\cdots &\cdots &\cdots &0\\
> 0& \lambda &1 &0 &\cdots &\cdots &0\\
> 0& 0& \lambda &1 &0 &\cdots &0\\
> \vdots &\ddots &\ddots &&\cdots &1& \vdots\\
> 0 &\cdots &\cdots&&\cdots&\lambda &1\\
> 0 &\cdots &\cdots&&\cdots &0 &\lambda
> \end{pmatrix} \in \mathbb{C}^{m_k \times m_k}$$
> Vale anche che $\sum_{k=1}^{\phi} m_k = n$.
> Siano $\lambda_1, ..., \lambda_s$ gli autovalori di $A$, $n_i$ è il più grande blocco di Jordan in cui appare $\lambda_i$, ed è detto **indice** di $\lambda_i$.

^290301

```ad-warning
La **molteplicità** di un autovalore $\lambda_i$ è pari alla somma delle dimensioni di **tutti** i [[#^290301|blocchi di Jordan]] in cui esso appare.
```

^95a382


```ad-todo
Vedere **matrice nilpotente di Jordan**.
$$J_k = \lambda_k I + N_k$$
```


> **Def.**
> Una funzione tra matrici $f$ è detta **definita sullo spettro** di una matrice $A$, se i seguenti valori esistono:
> $$f^{(j)}(\lambda_i), \; j=0,...,n_i-1, \; i=1, \dots, s$$
> dove $f^{(j)}$ è la derivata $j$-esima di $f$.
> In altri termini, la funzione $f$ deve poter essere **derivata** $n_i-1$ per ogni autovalore $\lambda_i \in \lambda(A)$.

> **Def. JCF (Jordan Normal Form)**
> Sia $f$ definita sullo spettro di $A \in \mathbb{C}^{n\times n}$.
> Definiamo $$f(A) := Z \cdot f(J) \cdot Z^{-1} = Z \cdot \text{diag}(f(J_1), \dots, f(J_\phi)) \cdot Z^{-1}$$
> dove
> $$f(J_k) := \begin{pmatrix}
> f(\lambda_k) &f'(\lambda_k) &\dots & \dfrac{f^{(m_k - 1)}(\lambda_k)}{(m_k-1)!}\\
> 0 &f(\lambda_k) &\dots &\vdots\\
> \vdots & \ddots &\ddots &\vdots\\
> 0 &\cdots &\cdots &f(\lambda_k)
> \end{pmatrix}$$

> Supponiamo che $f$ sia sviluppabile in seri di Taylor, ovvero $$f(x_0) = f(x_0) + f'(x_0)(x-x_0)+ ... + \frac{f^{(j)}(x_0)}{j!}(x-x_0)^j + ...$$
> Allora $f(J_k)$ può essere scritta come una serie **finita** $$f(J_k) = f(\lambda_k)I + f'(\lambda_k)N_k + ... + \frac{f^{(m_k-1)}(\lambda_k)}{(m_k-1)!}N_k^{m_k-1}$$

```ad-example
Sia la matrice
$$A = \begin{pmatrix}
1/2 &1\\
0 &1/2
\end{pmatrix} = J$$
Abbiamo $n_1 = 2, s = 1, \lambda_1 = 1/2$.
Sia la funzione $f(x) = x^3$ definita sullo spettro di $A$.
Allora avremo la seguente uguaglianza.
$$f(J) =\begin{pmatrix}
f(1/2) &f'(1)\\
0 &f(1/2)
\end{pmatrix} = \begin{pmatrix}
1/8 &3/4\\
0 &1/8
\end{pmatrix} = J^3 = A^3 = f(A)$$
Si evince inoltre che $Z = I$.
```


## Seconda definizione - Polinomio interpolante di Hermite

> **Def. polinomio minimo di una matrice**
> Il **polinomio minimo** $\psi$ di una matrice $A$ è l'**unico** polinomio monico (**coefficiente** del grado più alto è 1) di grado più basso tale che $\psi(A) = \mathbf{0}$.
> 
> Il polinomio minimo è **unico** perché divide **tutti** i polinomi $p$ tali che $p(A) = \mathbf{0}$.
> **Proof**: sia $p = \psi \cdot q + r$ tale che $0 < \deg{(r)} < \deg(\psi)$. Allora $$\mathbf{0} = p(A) = \psi(A)q(A) + r(A) = \mathbf{0}q(A) + r(A) = r(A)$$
> Questo è assurdo, perché per definizione $\deg(\psi)$ è minimo tale che $\psi(A) = 0$, però abbiamo un $r$ di grado minore tale che $r(A) = 0$.
> Perciò deve essere per forza vero che $\deg(r) = 0$, perciò $\psi$ divide $p$ $\square$.

^9cd009

> Definiamo $\psi(t)$ come segue
> $$\psi(t) = \prod_{i=1}^{s} (t - \lambda_i)^{n_i}$$

> **Teorema**
> Siano $p,q$ due polinomi ed $A \in \mathbb{C}^{n \times n}$, allora $p(A) = q(A)$ **se e solo se** $p$ e $q$ prendono gli stessi valori sullo spettro di $A$, ovvero
> $$p^{(j)}(\lambda_i) = q^{(j)}(\lambda_i), \; j=0,...,n_i-1, \; i=1, \dots, s$$
> **Nota:** si può dimostrare usando il polinomio minimo $\psi$.
> Come risultato del teorema avremo che la matrice $p(A)$ è **completamente determinata dai valori sullo spettro** $\lambda(A)$.

> **Def.**
> Sia $f$ definita sullo spettro di $A \in \mathbb{C}^{n \times n}$, e $\psi$ il polinomio minimo di $A$.
> Si definisce $f(A) := p(A)$ con grado $$\deg(p) < \sum_{i=1}^{s}n_i = \deg(\psi)$$ che soddisfano le seguenti condizioni di interpolazione
> $$p^{(j)}(\lambda_i) = f^{(j)}(\lambda_i), \; j=0,...,n_i-1, \; i=1,...,s$$
> Il polinomio $p$ **esiste** ed è **unico** de è detto **polinomio interpolante di Hermite**.

```ad-example
Sia $f(t) = \sqrt{t}$, e sia la matrice
$$A = \begin{pmatrix}
2 &2\\
1 &3
\end{pmatrix}$$
con $\lambda_1 = 1, \lambda_2 = 4$, $s = 2$, $n_1=n_2=1$.
$$p(1) = f(1) = 1, \; p(4) = f(4) = 2$$
Usando il [polinomio interpolante di Lagrange](https://it.wikipedia.org/wiki/Interpolazione_di_Lagrange) possiamo riscrivere
$$p(t) = f(1)\frac{t-4}{1-4} + f(4)\frac{t-1}{4-1} = \frac{1}{3}(t+2)$$
perciò
$$p(A) = \frac{1}{3}(A + 2I) = \frac{1}{3} \cdot
\begin{pmatrix}
4 &2\\
1 &5
\end{pmatrix} =
\begin{pmatrix}
4/3 &2/3\\
1/3 &5/3
\end{pmatrix} =: f(A)
$$
Infatti si può verificare che $(f(A))^2 = A$.
```
