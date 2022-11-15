Un modo alternativo al [[Jaccard coefficient]] che tiene anche conto della **frequenza dei termini** per determinare la rilevanza dei documenti, è il semplice modello a **bag of words**.

Per rappresentare la collezione usiamo una semplice [[Binary Term-Document Incidence Matrix]] dove però invece di avere 0-1, abbiamo la **frequenza** del termine nel documento.

Questa matrice è anche detta **Count Matrix**.

![](./img/IR_bag_of_word_1.png)

I documenti sono quindi rappresentati come un insieme di **vettori** in $\mathbb{N}^{\vert V \vert}$, dove $V$ è l'insieme dei termini (o vocaboli).

```ad-attention
Questo metodo è **naive**, in quanto non tiene conto della **semantica** della query.

Infatti se cerco:
- `il gatto morde il cane`
- `il cane morde il gatto`

tale modello restituisce lo stesso risultato, attribuendo lo stesso voto agli stessi documenti.
```
