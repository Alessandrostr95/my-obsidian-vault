---
date: 07-03-2023
draft: true
---

Non esistono metologie formali per l'analisi della sicurezza dei sistemi informativi.
È difficile definire la sicurezza in un sistema informativo.

Definire un sistema di sicurezza di una casa è più semplice, perché:
- **conoscenza totale** dell'ambiente che vogliamo difendere e di cosa può impedire i mal intenzionati.
- **conosciamo i punti deboli** dell'ambiente che vogliamo difendere, e dove quindi andare a intervenire (sappiamo i possibili attacchi).
- **conosco** chi si trova davanti a casa mia, e posso decidere se può entrare o no.

Nei sistemi informativi si lavora su vari livelli/astrazioni.
Per esempio il primo livello è l'**architettura fisica**.
Sopra di esso vengono costruite altri livelli, i quali sfruttano reciprocamente quelli inferiori.
Quindi gli attacchi possono essere fatti a diversi livelli, perciò dobbiamo definire la nostra "sicurezza" su più **dimensioni**.

Un sistema informativo è un'archittettura multistrato, conettualmente concepito per essere esguita strato per strato, ma a livello di sicurezza un'operazione può attraversa e saltare gli strati, rendendo invisibili i salti di strato intermedi, rendendo di fatto possibili dei jump profondi nel sistema.

Un'altra differenza è che con le case sappiamo esattamente separare chi conosciamo da chi non conosciamo.
Sappiamo anche distinguere oggetti comuni da oggetti potenzialmente pericolosi.

Quindi riassumendo le problematiche principali sono:
- abbiamo tanti **strati** che interagiscono tra di loro.
- abbiamo la possibilità di effettuare tanti **salti diretti** tra gli strati.
- problema della **separazione tra dati e codice** (cosa posso eseguire e cosa no?)

Un'altra grande problematica è il fatto che le macchine potrebbero non funzionare sempre bene, anche se il nostro sistema informativo è correttamente funzionante.
Basta pensare allo **stack overflow**: posso caricare eccessivamente lo stack dei processi (anche se con operazione del tutto legali), finché non va giù la macchina.

## DIFENSORE vs ATTACCANTE
Il **difensore** come l'**attaccante** hanno una conoscenza totalmente incompleta di un sistema informativo.
La differenza è che c'è un **delay time** a favore dell'attaccante che fa si che l'attacco sia efficace per un **tempo breve** e **limitato**.
Infatti nel momento se l'attccante crea un attacco vuol dire che ha una conoscenza in più rispetto al difensore.
Nel momento in cui viene effettuato l'attacco e poi successivamente scoperto, aggiunge al difensore l'informazione mancante che gli consenta di difendersi e neutralizzare l'attacco.

L'obiettivo è quindi:
- (attaccante) come rendere il delay time più **lungo** possibile?
- (difensore) come rendere il delay time più **breve** possibile?

