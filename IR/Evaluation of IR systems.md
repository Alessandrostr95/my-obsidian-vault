Fin ora abbiamo visto una serie di tecniche per la [[Index construction|costruzione]], [[Index Compression|compressione]] e [[Scoring, term weighting & the vector space model|scoring]] di un motore di IR.

Una domanda lecita è:

> Quale combinazione di tecniche è meglio usare, possibilmente rispetto alle mie esigenze?
> Quale modello usare? 
> Il mio motore di IR è **buono**?

Per rispondere a queste domande abbiamo bisogno di **misurare** la qualità del modello.
Per fare ciò abbiamo bisogno di:
1. Un **sottoinsieme** di documenti significativi per il mio **information need** su cui fare test. ^79484d
2. Un insieme di **query** significative per il mio **information need**.
3. Per valutare la qualità delle risposte alle query, abbiamo bisogno di **precomputare** (tramite degli annotatori *umani*) la **rilevanza** dei documenti nel punto [[#^79484d|(1)]] in base all'**user need**. 

Una volta ottenuto quello di cui abbiamo bisogno, possiamo valutare la qualità di un motore di IR tramite una serie di **banchmark**.

```ad-example
title: Esempio
1. Prendo il mio **user need**: "*la mia piscina è sporca e la voglio pulire*".
2. Costruisco una query sulla base del mio user need "*clear pool*".
3. Pongo la query al mio motore di IR sulla collezione composta dal solo **sottoinsieme di documenti** rappresentativi.
4. Confronto il risultato con le **annotazioni** pre-computate a priori.
5. Come faccio a valutare la qualità della risposta? Il mio risultato è abbastanza "*azzeccato*"?
```

-----
- [[Precision, Recall, Accuracy, Error, F-Measure]]