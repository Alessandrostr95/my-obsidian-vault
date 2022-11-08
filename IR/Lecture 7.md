---
date: 2022-11-08
draft: true
---
# Ranked Retrieval
Abbiamo visto fin ora:
- delle strutture dati che ci consentano di rispondere a delle [[IR/Lecture 3|query booleane]] semplici su un insieme enorme di documenti
- come tali strutture dati possono essere [[IR/Lecture 6|comprimere]] al minimo.

Una seconda parte del problema è che dei sistemi di IR è che:
- i documenti restituiti da una query sono generalmente tantissimi, e una persona non può processarli in maniera efficiente per trovare quello che gli serve
- usando male gli **operatori** di ricerca booleana potrebbe indurre in un risultato vuoto.

Infatti consideriamo la query `world climate change merkle`, espressa in **linguaggio naturale**.
Possiamo processarla come:
	- `world climate change` AND `merkle` -> $\emptyset$
	- `world climate change` OR `merkle` -> **very big result**

Però capire come usare gli operatori booleani in maniera autmatica è computazionalmente difficile.

Generalmente, possiamo definire le proprietà che si voglia che un motore di IR abbia:
1. Deve mostrare solo un **piccolo sottoinsieme** di pagine più rilevante (per esempio 10). ^b46390
2. L'utente non dovrebbe esprimere le query in maniera troppo complessa, è preferibile che usi il **linguaggio naturale**.

Per il [[#^b46390|punto 1]] vogliamo che il motore dia uno **score** *di rilevanza* ai documenti, generalmente **normalizzato** in $\left[ 0,1 \right]$, in modo così da ordinarli in modo **non crescente**.

Tale *score* deve essere un **indice di rilevanza** del documento rispetto alla query.

Per esempio, se serco un termine `X` mi aspetto che più in documento appare il termine

## Jaccard Coefficient
Siano $A,B$ due **insiemi**.
Il **coefficente di Jaccard** è definito come $$J(A,B) = \frac{\vert A \cap B \vert}{\vert A \cup B \vert}$$

- $J(A,B) = 0 \implies$ sono totalmente differenti.
- $J(A,B) = 1 \implies$ sono uguali.

```ad-important
La misura di *Jaccard* non è una metrica, in quanto non vale che:
- $J(A,A) = 0$
- $J(A,B) = J(A,X) + J(X,B)$ (**disugagliaza triangolare**)

Abbiamo come proprietà solo la **simmetria**.
```


Se poniamo $A = \text{query}$ e $B = \text{document}$, possiamo misurare la "*rilevanza*" del documento $B$ rispetto alla query $A$.


Possiamo quindi usare il coefficiente di Jaccard come **funzione di ordinamento** per il risultato della nostra query.

### Problema
Questo coefficiente purtroppo non tiene conto della **frequenza** in cui i termini appaiono in un documento: tiene solo conto di quali termini della query appaiono nel documento.

Esempio: ho due documenti $D_1$ e $D_2$. La mia query è $B = \brace \texttt{ termaX } \rbrace$. Supponiamo che il termine `termX` in entrambi i documenti, con frequenze relativamente $3$ volte e $88$ volte.
Se la dimensione è lastessa, allora avremo che $$J(B,D_1) = J(B,D_2)$$.

Un altro problema è la **dimensione** del documento.
Più un documento è grande, più esso verrà **penalizato** nello scoring.

Un accorgimento è quindi quello di normalizzare per la **radice** dell'unione degli insiemi.

$$J'(A,B) = \frac{\vert A \cap B \vert}{\sqrt{\vert A \cup B \vert}}$$

## Count matrix
Per dare maggiore enfasi ai documenti con maggiore frequnza dei termini della query, usiamo una rappresentazione di tipo **bag of words**.

Abbiamo una matrice `term x doc`, dove nella cella $i,j$ ho il numero di volte in cui il termine $i$ appare nel documenti $j$.



