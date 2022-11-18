---
date: 2022-11-18
draft: true
---
Abbiamo due tipologie di regole:
- **preterminali** da non-terminale a terminale $A \to a$
- **binarie** da un non-terminale in due non-terminali $A \to BC$

Sia una matrice quadrata $A \in \mathbb{R}^{n \times n}$.
Per comodità indichiamo con $A^+$ la sua identità e $A^-$ la sua **inversa**.

In ogni cella $i,j$ della matrice del CYK avrò un insieme di non terminali, del tipo $\lbrace A, B, ... \rbrace$.

Vorrei rappresentare questo insieme come una serie di **operazioni matriciali**.
Per esempio, se nella cella $i,j$ ho i simbilo $\lbrace A, B \rbrace$ vorrei rappresentare il suo contenuto della cella con la matrice/vettore $i^+ A^+ j^-$ e $i^+ B^+ j^-$.

Prendiamo la regola $A \to BC$.
Possiamo rappresentare questa regola in formato matriciale all'interno del CYK.
Per esempio, supponiamo che la cella $(i,j)$ è generata dalla combinazione delle celle $(i,s)$ ed $(s,j)$ mediante la regola $A \to BC$.
Ovvero:
- in $(i,s)$ abbiamo $B$.
- in $(s,j)$ abbaimo $C$.
- siccome abbiamo la regola $A \to BC$, possiamo inserire in $(i,j)$ il simbolo $A$.

Per fare ciò possiamo fare il prodotto matriciale
$$\underbrace{i^+ B^+ s^-}_{(i,s)} \cdot (s^- B^- A^+ C^- s^-) \cdot \underbrace{s^+ C^+ j^-}_{(s,j)} \equiv i^+ A^+ j^-$$

Dato che dentro una cella $(i,j)$ sono presenti molteplci simboli, possiamo rappresentarli come $i^+ (A^+ + B^+ + ...) j^-$.

Vorremmo una matrice $M$ che contenga tutte queste informazioni al suo interno, e che in tempi brevi ci consenta di "*accedere*" alle sue informazioni.

$$R_B = \sum_{A \to BC} B^+ A^+ C^-$$
$$\overline{R_B} = \sum_{i=1}^{n} s^+ R_B s^-$$
Dove $s$ rappresenta una posizione possibile di una diagonale.

Per la diagonale $k$ della matriciale avrò la matrice $M_k$.
$$M_k \overline{R_B} M_k = M_{k+1}$$