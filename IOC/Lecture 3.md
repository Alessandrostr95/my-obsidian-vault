---
date: 2024-05-16
draft: true
---
> **Formula di Lagrange-Hermite**
> $$p(x) = \sum_{i=1}^{s} \left[ \left( \sum_{j = 0}^{n_i - 1} \frac{1}{j!} \phi^{(j)}_i(\lambda_i)(x-\lambda_i)^j \right)\prod_{j \neq i}(x-\lambda_j)^{n_j} \right]$$
> dove
> $$\phi_i(x) = \frac{f(x)}{\prod_{j \neq i} (x-\lambda_j)^{n_j}}$$

> **Polinomio interpolante di Lagrange**
> Se gli autovalori sono **tutti distinti**, ovvero $\lambda_i \neq \lambda_j$ per ogni $i \neq j$, abbiamo la seguente formula più semplice
> $$p(x) = \sum_{i=1}^{n} f(\lambda_i) \prod_{j \neq i}\left( \frac{x-\lambda_j}{\lambda_i - \lambda_j}\right)$$

> **Formula di Newton**
> Possiamo definire
> $$p(x) = f[x_1] + f[x_1,x_2](x-x_1) + f[x_1,x_2,x_3](x-x_1)(x-x_2) + \dots + f[x_1,\dots,x_m
> ](x-x_1) \cdot \dots \cdot (x-x_{m-1})$$
> dove $m = \deg(\psi)$ ([[IOC/Lecture 2#^9cd009|polinomio minimo di A]]) $$\psi(x) = \prod_{i=1}^{s}(x-\lambda_i)^{n_i}$$ e $\{ x_1, ..., x_m\}$ **includono** gli autovalori $\lambda_1, ..., \lambda_s$, ognuno dei quali è presente $n_i$ volte.
> Quindi $m = \sum_{i=1}^{s} n_i$.
> > **Esempio**
> > $$q(x) = f[x_1] + f[\lambda_1,\lambda_2](x-\lambda_1) + f[\lambda_1,\lambda_2,\lambda_3](x-\lambda_1)(x-\lambda_2) + \dots + f[\lambda_1,\dots,\lambda_n](x-\lambda_1) \cdot \dots \cdot (x-\lambda_{n-1})$$
> > Osserviamo che $\deg(q) \geq \deg(p)$ perché in $q$ appaiono gli autovalori un numero di volte pari alla loro [[IOC/Lecture 2#^95a382|molteplicità]].
> 

^feba69

```ad-note
La [[#^feba69|formula di Newton]] che viene usata per costruire $f(A)$, lo fa come **polinomio** in $A$.
```


## Terza definizione - FAC

> **Def. FAC**
> Sia $A \in \mathbb{C}^{n \times n}$, allora definiamo
> $$f(A) = \frac{1}{2\pi i} \int_{\Gamma} f(z) (zI-A)^{-1}dz$$
> Deve essere necessario che $f$ è [[#Funzione analitica|analitica]] sopra $\Gamma$. Inoltre $\Gamma$ è una **curva chiusa** che **include** gli autovalori.

> **Def. (Operatore risolvente)**
> La matrice $(zI-A)^{-1}$ è detto **operatore risolvente**, $\forall z \in \Gamma$.

> **Teorema.**
> Per ogni matrice $A$ **finita** e per ogni funzione $f$ [[#Funzione analitica|analitica]], allora le definizioni [[IOC/Lecture 2#Prima definizione - JCF|prima]], [[IOC/Lecture 2#Seconda definizione - Polinomio interpolante di Hermite|seconda]] e [[#Terza definizione - FAC|terza]] sono **equivalenti**.

```ad-note

- La definizione [[IOC/Lecture 2#Seconda definizione - Polinomio interpolante di Hermite|seconda]] (col polinomio di interpolazione) serve per dimostrare proprietà
- La definizione [[IOC/Lecture 2#Prima definizione - JCF|JCF]] serve per risolvere **equazioni** del tipo $g(X) = A$.
```

### Alcune proprietà
1. $f(A)$ commuta con $A$.
2. $f(A^T) = (f(A))^T$
3. $f(XAX^{-1}) = Xf(A)X^{-1}$ (si dice **invariante per similitudine**).
4. gli autovalori di $f(A)$ sono $f(\lambda_i)$ dove $\lambda_i$ sono gli autovalori di $A$.

-----
## Appendice

### Differenze divise
La **differenza divisa** $f[ \cdot ]$ è definita come
$$f[x_i] = f(x_i)$$
$$f[x_i,x_j] = \frac{f(x_i) - f(x_j)}{x_i - x_j}\;\; ,x_i \neq x_j$$
$$f[x_i,x_j] = f'(x_i) = f'(x_j)\;\; ,x_i \neq x_j$$
$$f[x_i,x_j,x_k] = \frac{f[x_i,x_j]- f[x_j,x_k]}{x_i-x_k}$$
$$f[x_n,\dots, x_{n+k}] = \frac{f[x_n,\dots,x_{n+k-1}]- f[x_{n+1},\dots,x_{n+k}]}{x_n-x_{n+k}}$$

### Funzione analitica
Una funzione $f$ è **analitica** se può essere derivata un numero **infinito** di volte.