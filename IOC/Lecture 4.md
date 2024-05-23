---
Date: 2024-05-23
draft: true
---
# Degree centrality

# Betweeness centrality
Sia un nodo $i$ di una rete $G$, definiamo la between centrality di $i$ come
$$c_B(i) = \sum_{j \neq i}\sum_{k \neq j}\delta_{jk}(i)$$
dove
$$\delta_{jk}(i) = \frac{\text{\# cammini minimi tra } j,k \text{ che passano per il nodo }i}{\text{\# cammini minimi tra } j,k}$$

Purtroppo il numero di cammini tra due nodi $j,k$ cresce **esponenzialmente**, perciò sorge un problema di calcolabilità (efficienza).
In pratica esistono degli algoritmi **polinomiali** per il calcolo della betweeness, però purtroppo di grado troppo alto.

# Closeness centrality
La closeness centrality di un nodo $i$ è definita come
$$c_{C}(i) = \frac{1}{\sum_{j \in V}d_G(i,j)}$$

```ad-note
Ne betweeness ne closeness sono buone misure su reti complesse. Infatti in molte reti reali abbiamo un diametro molto piccolo, e quindi **molti** nodi centrali. E' come fare un ranking in una classe di studenti molto bravi, se pur con caratteristiche differenti.
```


## Eigenvector centrality
Sia $\mathbf{x}$ l'[[#Teorema di Perron-Frobenius|autovettore dominante]] di una matrice di adiacenza $A$. Allora esso equivale al **limite** di questa *iterazione a punto fisso*
$$\mathbf{x} := \lim_{n \to \infty} \mathbf{x}^{(n)}$$ dove
$$\mathbf{x}^{(n)} = A \mathbf{x}^{(n-1)}$$

L'**eigenvector** centrality di un nodo $i$ è pari alla componente $i$-esima dell'autovettore dominante $\mathbf{x}$ (opportunamente normalizzato).

```ad-note
L'iterazione a punto fisso 
$$\mathbf{x}^{(n)} = A \mathbf{x}^{(n-1)}$$
scleto un qualsiasi $\mathbf{x}^{(0)}$ **non negativo** e **non nullo** è detto **metodo delle potenze**.
```

Questo vale per $A$ [[#Matrice primitiva|primitiva]].

Se invece $A$ è non-primitiva allora sostituiamo $A$ con
$$\hat{A} = (1-\alpha)I + \alpha A, \; \alpha \in (0,1)$$

```ad-note
Questo metodo converge velocemente solamente quando l'autovalore dominante è ben distaccato dal secondo autovalore.
```

```ad-note
Il **page-rank** è una variante dell'Eigenvector centrality in modo tale da avere sempre un autovalore dominante positivo.
```


## Eigenvector centrality per grafi diretti (HITS)
Una estensione del [[#Eigenvector centrality]] su grafi diretti è l'**Hyperlinks Induced Topic Search** (**HITS**)
In questo caso avremo **due** autovettori dominanti
- autovettore dominante destro - hub score $$\mathbf{h}^{(k)} = A^TA \mathbf{h}^{(k-1)}$$
- autovettore dominante sinistro - autority score $$\mathbf{a}^{(k)} = A^TA \mathbf{a}^{(k-1)}$$

Un modo alternativo è il seguente
$$\begin{cases}
\mathbf{a}^{(k)} = A^T\mathbf{h}^{(k-1)}\\
\mathbf{h}^{(k)} = A\mathbf{a}^{(k)}
\end{cases}$$

Vedi [[18 - Web Search - Part 2]].



-------
## Appendice
###  Diametro di un grafo
Il diametro di un grafo $G = (V,E)$ **connesso** è la massima distanza tra due nodi qualsiasi
$$\text{diam}(G) = \max_{(u,v) \in \binom{V}{2}} d_G(u,v)$$
### Teorema di Perron-Frobenius
Sia $G$ un grafo semplice e connesso con [[|matrice di adiacenza]] $A$, per il [teorema di Perron-Frobenius](https://it.wikipedia.org/wiki/Teorema_di_Perron-Frobenius) allora ha un autovettore $\mathbf{x} > \mathbf{0}$ positivo e il suo autovalore **dominante** (modulo massimo, reale, unico).

### Matrice primitiva
Una matrice non negativa è detta primitiva se esiste un $k$ tale $A^k$ è positiva.

