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

