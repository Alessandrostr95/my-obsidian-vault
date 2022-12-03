Vogliamo che dato un **infermotion need** espresso sottoforma di **query** ed una **collezione di documenti** un sistema di IR sia in grado di misurare quanto i documenti siano inerenti alla query.

Dato che c'è sempre un livello di **incertezza** sul significato di una query $q$ (dato che sono espresse in **linguaggio naturale**) allora avremo certamente che anche il valore di disgnificatibilità di un documento $d$ avrà una certa **incertezza**.

Infatti l'incertezza può essere data da vari fattori:
- diversi utenti potrebbero avere diverse opinioni sulla rilevanza di $d$ rispetto a $q$.
- uno stesso utente potrebbe definire $d$ rilevante o meno rispetto a $q$ dipendentemente dal contesto.
- la rappresentazione dei documenti nel sistema di IR potrebbe non catturare a pieno il concetto di rilevanza. Infatti per esempio il [[Vector Space Model]] definisce la rilevanza dei documenti solamente rispetto alla similitudine (intesa come insieme di parole) con la query. Però sappiamo che una cosa si può dire in molti modi diversi e con parole differenti.

Possiamo quindi definire un **modello probabilistico** in cui il ranking è basato sulla probabilità che un documento $d$ sia effettivamente rilevante rispetto ad una query $q$.
$$P(d \text{ è rilevante rispetto a }q)$$

- [[Appendice Probabilità]]
- [[Probabilistic Ranking Principle]]
- [[Binary Independent Model]]