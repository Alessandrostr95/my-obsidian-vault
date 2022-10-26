---
date: 2022-10-26
draft: true
---
# Dynamic Indexing
Gli indici costruiti [[IR/Lecture 4|fin ora]] si basano su collezioni di documenti statiche.
Come possiamo fare invece nel caso di collezioni **dinamiche** che cambiano nel tempo? (*insert*, *delete* e *update* dei documenti)

## Simple approach
- Mantengo un *grande* indice costruito inizialmente
- **INSERT**: Periodicamente, creo nuovi piccoli indici con i nuovi indici aggiunti.
- A seguito di una richiesta, effetto la query su tutti gli indici, e poi *fondo* tutti i risultati ottenuti
- **DELETE**: per la rimozione invece contrassegno contrassegno il documento come **rimosso** con un flag. Così a seguito di una query, semplicemente ignoro i documenti contrassegnati come rimossi.
- **UPDATE**: per l'update, combino la rimozione e l'insert 
- Periodicamente, costruisco di nuovo un grande indice

## Svantaggi
- È troppo rischioso costruire sempre un nuovo indice.
- Troppo dispendioso fare sempre merge.
	- Sarebbe efficiente fondere se abbiamo un file per ogni posting list, basta infatti appendere righe su tale file
	- Purtroppo però un OS può tenere un numero limitato di files.


## Logarithmic Merge
Assumiamo per il momento di avere un **unico indice** in un **unico file**.
([[IR/Lecture 4|sappiamo]] che spesso non è così, perché potrei avere un indice partizionato su differenti macchine)


- Ho il mio **indice principale** su disco.
- In ram creo un **indice ausiliare**, e quando la memoria è satura scrivo in memoria.
- Quando gli indici caricati in memoria, tutti insieme, hanno una dimensione **comparabile** all'indice principale attuale, operiamo un operazione di merge.
- Così facendo, otteniamo un **numero logaritmico** di operazione di merge, **ammartizzando** così il numero di operazioni.

