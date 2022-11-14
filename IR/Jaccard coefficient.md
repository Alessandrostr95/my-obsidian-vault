Siano $A,B$ due **insiemi**.
Il **coefficente di Jaccard** è definito come $$J(A,B) = \frac{\vert A \cap B \vert}{\vert A \cup B \vert}$$
Il coefficiente di Jaccard restituisce sempre un numero compreso tra 0 ed 1, perciò possiamo usarlo come funzione di ordinamento per il risultato della nostra query.

Infatti abbiamo che:
- $J(A,B) = 0 \implies$ sono totalmente differenti.
- $J(A,B) = 1 \implies$ sono uguali.

Se poniamo $A = \text{query}$ e $B = \text{document}$, possiamo misurare la "*rilevanza*" del documento $B$ rispetto alla query $A$.

```ad-important
La misura di *Jaccard* non è una **metrica**, in quanto non vale che:
- $J(A,A) = 0$
- $J(A,B) = J(A,X) + J(X,B)$ (**disugagliaza triangolare**)

Abbiamo come proprietà solo la **simmetria**.
Non possiamo quindi definire una **spazio metrico**.
```

### Problematiche
Questo coefficiente purtroppo non tiene conto della **frequenza** in cui i termini appaiono in un documento: tiene solo conto di quali termini della query appaiono nel documento.

Generalmente la frequenza di un documento in un documento è una sorta di **indice di rilevanza** del documento.

> **Esempio**
> Abbiamo due documenti $D_1$ e $D_2$.
> La query è $B = \{ \texttt{ termaX } \}$.
> Supponiamo che il termine `termX` sia presente in entrambi i documenti, con frequenze relativamente $3$ volte e $88$ volte.
> Se la dimensione è lastessa, allora avremo che $$J(B,D_1) = J(B,D_2)$$ anche se in teoria il documento $D_2$ è più significativo.

Un altro problema è la **dimensione** del documento.
Più un documento è grande, più esso verrà **penalizato** nello scoring.

Un accorgimento è quindi quello di normalizzare per la **radice** dell'unione degli insiemi.

$$J'(A,B) = \frac{\vert A \cap B \vert}{\sqrt{\vert A \cup B \vert}}$$
