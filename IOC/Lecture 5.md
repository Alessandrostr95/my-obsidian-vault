---
date: 2024-05-30
draft: true
desc: indici di centralità di tipo esponenziale
---


Sia $A$ la matrice di adiacenza di una rete. Allora $(A^k)_{i,j}$ è il numero di cammini (non pesati e semplici) di lunghezza al più $k$ tra $i$ e $j$.

## Network communicability
La **network communicability** tra due nodi $i$ e $j$ è definita come
$$(e^A)_{i,j} := \sum_{k=0}^{\infty}\frac{1}{k!}(A^k)_{i,j}$$
Il vantaggio di questo metodo è che **penalizza** il peso dei cammini più lunghi (in maniera esponenziale per via del $k!$) e **premia** quelli più corti.

Possiamo aggiungere un parametro $\beta > 0$, avendo
$$(e^{\beta A})_{i,j} := \sum_{k=0}^{\infty}\frac{\beta^k}{k!}(A^k)_{i,j}$$

```ad-info
Il peso dei cammini di lunghezza $k$ sarà $\beta^k/k!$, perciò:
- per $\beta > 1$ penalizziamo quelli più corti
- per $\beta \leq 1$ penalizziamo quelli più lunghi
```

Il parametro $\beta$ viene anche chiamato **inverse temperature**. ^290b54

## Subgraph centrality (basato su esponenziale di matrice)
Sia $G$ un grafo **non diretto**.
La subgraph centrality $SC(i)$ di un nodo $i$ come il numero di cammini di lunghezza $k$ che iniziano e che finiscono in $i$ (pesato rispetto a $k!$).
Più precisamente
$$SC(i) := (e^A)_{i,i} = \left(I+A+\frac{A^2}{2}+\frac{A^3}{3!}+ \dots + \frac{A^k}{k!} + \dots\right)_{i,i}$$

```ad-note
- la **diagonale** di $e^A$ è la subgraph centrality
- gli elementi **extra-diagonali** di $e^A$ sono la network communication centrality
```

Anche in questo caso possiamo aggiungere la [[#^290b54|inverse temperature]] $\beta$
$$SC(i,\beta) := (e^{\beta A})_{i,i} = \left(I+\beta A+\frac{(\beta A)^2}{2}+\frac{(\beta A)^3}{3!}+ \dots + \frac{(\beta A)^k}{k!} + \dots\right)_{i,i}$$

### Subgraph centrality (basato su operatore risolvente)
Possiamo definire una variante della subgraph centrality, però basata su **operatore risolvente** $f(A)$

$$SC_{res}(i,\alpha) := (f(A))_{i,i}$$
 L'**operatore risolvete** $f(A)$ è definito come 
$$f(A) = (I-\alpha A)^{-1}$$
## Indice di Estrada
$$EE(G,\beta) = \sum_{i \in V} SC(i,\beta) = \sum_{i \in V} (e^{\beta A})_{i,i} = \text{Tr}(e^{\beta A})$$



## Entropia del cammino
Definiamo una distribuzione discreta di probabilità sui nodi del grafo (basata su [[#Subgraph centrality]] e [[#Indice di Estrada]])
$$p(i) =\frac{SC(i,\beta)}{EE(G,\beta)}$$
che sarebbe la probabilità di scegliere un ciclo che passa su $i$ su tutti i cicli di $G$.


$$S^V(G,\beta) = - \sum_{i \in V}p(i) \log_2 p(i)$$
$$H(G,\beta) = - \sum_{i \in V}\lambda_i p(i)$$
## Helmholtz free energy
$$F(G,\beta) = -\beta^{-1}log_2(EE(G,\beta))$$

## Katz Centrality
Sia un grafo **non diretto**. Sia l'operatore risolvente $$f(A) = (I-\alpha A)^{-1}$$

La **Katz** centrality di un nodo $i$ è definita come
$$K(i,\alpha) = (f(A) \mathbf{1})_{i}$$

La matrice $I-\alpha A$ deve essere **invertibile**, perciò è necessario scegliere $0 < \alpha < 1/\rho(A)$.

```ad-note
Nota che per grafo $G$ **diretto** avremo che:
- $f(A)\mathbf{1}$ corrisponde alla [[IOC/Lecture 4#Eigenvector centrality per grafi diretti (HITS)|hub score]] 
- $f(A)^T\mathbf{1}$ corrisponde alla [[IOC/Lecture 4#Eigenvector centrality per grafi diretti (HITS)|authority score]]
```



## Total communicability
$$TC(i,\beta) := (e^{\beta A} \mathbf{1})_i$$

## Teorema
Sia $G$ un grafo connesso e **non diretto**. Sia $A$ la sua matrice di adiacenza.
1. per $\beta \to 0^+$ allora il ranking prodotto da $SC(i, \beta)$ [[#Subgraph centrality]] e $TC(i, \beta)$ [[#Total communicability]] **convergono** alla [[IOC/Lecture 4#Degree centrality|degree centrality]];
2. per $\alpha \to 0^+$ allora i ranking $SC_{res}(i,\alpha)$ [[#Operatore risolvente]] e $K(i,\alpha)$ [[#Katz Centrality]] **convergono** alla [[IOC/Lecture 4#Degree centrality|degree centrality]];
3. per $\beta \to 0^+$ allora il ranking prodotto da $SC(i, \beta)$ [[#Subgraph centrality]] e $TC(i, \beta)$ [[#Total communicability]] **convergono** alla [[IOC/Lecture 4#Eigenvector centrality|eigenvector centrality]];
4. per $\alpha \to (1/\rho(A))^-$ allora i ranking $SC_{res}(i,\alpha)$ [[#Operatore risolvente]] e $K(i,\alpha)$ [[#Katz Centrality]] **convergono** alla [[IOC/Lecture 4#Eigenvector centrality|eigenvector centrality]];
5. i valori limite di $SC(i, \beta)$ e $K(i, \beta)$ rimangono **validi** se si usa un **qualsiasi** vettore ==preference== $\mathbf{v}$ positivo, anziché $\mathbf{1}$.


-----

## Cammini non-backtracking
Un cammino è detto **non-backtracking** se **non** contiene sotto-sequenze del tipo $\langle i,j,i \rangle$.

## Eigenvector Communicability
La comunicabilità dei nodi $i,j$ è la cella $(i,j)$ della matrice $\mathbf{x}_1 \mathbf{x}_1^T$, dove $\mathbf{x}_1$ è l'autovettore di grado massimo.
Avremo che

$$(\mathbf{x}_1\mathbf{x}_1^T)_{i,j} = \lim_{\beta \to \infty} \frac{(e^{\beta A})_{i,j}}{e^{\beta \rho(A)}}$$
$$(\mathbf{x}_1\mathbf{x}_1^T)_{i,i} = \lim_{\alpha \to (1/\rho(A))^-} K(i,\alpha)$$

## Total network comunicability
La **total network communicability** è la media di tutte le [[#Total communicability]] dei nodi della rete $G$.
$$TC(G) = \frac{1}{n}\sum_{i \in V}\sum_{j \in V} (e^{\beta A})_{i,j} = \frac{1}{n} \mathbf{1}^T e^{\beta A} \mathbf{1}$$
Questa quantità si **massimizza** quando $G$ è completo.
