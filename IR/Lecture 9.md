---
date: 2022-11-15
draft: true
---
Fin ora abbiamo visto una serie di tecniche per la costruzione di un motore di IR.
Una domanda lecita è:

> Quale combinazione di tecniche è meglio usare, possibilmente rispetto alle mie esigenze? Quale modello usare?

Si vuole quindi **misurare** la qualità del modello.
Per esempio mediante una serie di **banchmark**.

Come primo approccio prendo il mio **user need** "*la mia piscina è sporca e la voglio pulire*".
Costruisco una query sulla base del mio user need "*clear pool*".

Prendiamo poi una collezione di documenti target, per sempio un centinaio, definiamo un insieme di **query rappresentative**, dopodiché vedo come si comporta il mio motore di IR. Quante pagine target "*azzecca*"?

# Precision & Recall
Abbiamo un insieme $A$ di documenti target, di documenti **rilevanti**.
Poi abbiamo l'insieme dei documenti restituiti $B$ dal mio motore di ricerca.

- $$\text{recall} = \frac{\vert A \cap B \vert}{A}$$
- $$\text{precision} = \frac{\vert A \cap B \vert}{B}$$

# Accuracy & Error

x | **retrieved** | **not retrieved** 
 ---|---|---
**relevant** | true-positive = $A \cap B$ = tp | false-negative = $A \setminus B$ = fn
**irrelevant** | false-positive = $B \setminus A$ = fp| true-negative = $\lnot (A \cap B)$ = tn

- $$acc = \frac{tp + tn}{tp+tn+fn+fp}$$
- $$err = \frac{fp + fn}{tp+tn+fn+fp} = 1 - acc$$

# Recall vs Precision
Maggiore è la recall minore è la precision, e viceversa.
[VEDI GRAFICO]
[VEDI ESEMPIO]

-> Maggiore recall = + tp + fn (?)
-> Meggiore precision =  + fp + tn (?)

# F-Measure
In genere si preferisce usare una sorta di **media** tra precisione recall.
La **F-measure** usa una **media armonica**
$$F = \frac{1}{\frac{1}{2}(\frac{1}{R} + \frac{1}{P})} = \frac{2RP}{R + P}$$

Esercizio, Caclolare la F-measure:
- $p,q = 0.8, 0.4$
- $p,q = 0.65, 0.55$

-----
# Rank-Based Measures
[VEDI]

## Recall-Precision Graph
## Breakeven Point
Definisco una grafico precision e racall, dati due risultati di una query.

Dopodiche le vado a interpolare, scegliendo sempre il valore massimo valore.

Sommando i differenti valori, su un numero elevato di query, dei differenti grafici interpolati.

-----
# Mean Avarage Precision
Metodo che funziona quando non ho modo di calcolare la precision e la recal in maniera diretta.


-----
# Discounted Cumulative Gain
Prima abbiamo visto delle misure di rilevanza **binarie**.
Noi vorremo invece delle misure a più **dimensione** di rilevanza.

Due assunzioni:
- La rilevanza dipende dalla posizione in rank.
- Però in genere le persone sono pigre, e quindi man mano che si scende di rank si perde interesse.

Siano le pagine $1,2, ..., n$ con rilevanza $r_1, ..., r_n$.
La rilevanza del mio rank è: $r_1 + r_2 + ... + r_n$

Visto che man mano che si va avanti nel rank le persone perdono interesse, possiamo pesare in maniera inversa i valori.
$$\sum_{i=1}^{n}\frac{r_i}{\log_2{i+1}}$$
