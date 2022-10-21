---
date: 21-10-2022
draft: true
keyword:
  - Sintassi
  - Semantica
  - Costituenti
  - Principio di Composizionalità Semantica
---

## Sintassi
Richiamando il [[NLP/Lecture 3#^628640|livello sintattico]], partiamo dalle **unità minime** composto da **parole** e vogliamo definire come sono composte le **unità d'interesse**, le **frasi**.

- **Salva Significatione** $\to$ **significato logico**
- **Salva Grammaticalità** $\to$ **struttura grammatica**

Prendo una frase, provo a sostituire una parola, e se sostituiendola preservo la **grammaticalità**, allora le due parole preservano la **grammaticalità**, anche perdendo il significato

- io **mangio** la pizza $\to$ io **taglio** la pizza
	- `mangio` e `taglio` **appartengono** alla stessa classe
- io **mangio** la pizza $\not\to$ io **la** la pizza
	- `mangio` e `la` non appartengono alla stessa classe

> **part of speach** $\equiv$ **classi grammaticali**

- il libro **bello** è sul tavolo $\to$ il libro **usato** è sul tavolo
	- **usato** è della stessa classe di **bello**. In realtà **usato** è un **participio passato**, però in questo caso appartiene alla classe di **aggettivo**.

- il libro **usato** da Mario è sul tavolo
	- in questo caso, la stessa parola, quasi stesso contesto e quasi stesso significato, assume la classe di **verbo**.

Per generare una grammatica, dobbiamo generare una **sequenza** di **costituenti** (**phrase**):
1. **N**omi
2. **V**erbi
3. **Aag**gettivi
4. **Avv**erbi
5. **Pr**oposizioni

I costituenti **governano** un **blocco grammaticale** quando tali blocchi si basano su di essi, ovvero ne derivano il significato.

I costituenti sono **ininterrompibili**, ovvero non si possono mettere altri elementi all'interno del blocco che governano.

Perciò la **sintassi** si occupa come strutturare le frasi *ben formate*, facendo interagire tra di loro le parole.
Data una sequenza **corretta** di parole, si può dire che tale frase è **sintatticamente corretta**, senza però minimamente preoccuparse della **semantica**.

## Semantica
Target
- **frasi**: voglio ricavare il senso delle frasi a partire dal senso delle parole che le compongono
- **parole**: voglio ricavare il senso delle parole a partire dal senso dei morfemi che le compongono
	- **irragionevole** deriva da **ir** e **ragionevole**, ovvero il senso è `non ragionevole`.

```ad-important
title: Principio di Composizionalità Semantica
Ottenere il senso del tutto partendo dal senso delle sue parti.
```

^246bb5

Dalle parti di una frase, possiamo ricavare il senso della frase stessa, purché esse rispettino la sintassi.

```ad-example
title: Quando il principio funziona

	La sedia rossa

La sedia è **rivestita** di un colore rosso

	La mela rossa

La mela **è** di colore rosso

	L'arancia rossa

Il colore **interno** dell'arancia è rosso.

Se chiedo ad una macchina "*mi prendi un'arancia rossa?*" esso non sarà mai in grado di farlo, perché esternamente sono tutte arancioni.

In questo caso l'interazione di oggetti differenti che implica significati differenti.
```

```ad-example
title: Quando il principio NON funziona

	hard disk
Letteralmente: `disco duro`.
Perché il disco interno alla memoria è solido.

	floppy disk
Letteralmente: `disco floscio`.
Perché il disco interno alla memoria è cos' sottile che se agitato è *floscio*.

-------

Qui il principio è stato **perso** perché ormai nessuno ci pensa più.


	solid state disk
Letteralmente: `disco allo stato solido`

In questo caso non possiamo dire che il principio si sia perso, perché all'interno non è presente alcun disco.
In questo caso il significato di disco è totalmente **cambiato**, in quanto ci si riferisce all'*hard disk*.
```

--------------
# Figure retoriche: Modi di dire
Per dire
	stai facendo una cosa totalmente inutile
possiamo dire
- *stai pettinando le bambole*
- *stai asciugando gli scogli*

## Metafora
È una **similitudine abbreviata**, ovvero una similitudine senza il "*come*".

- **Similitudine**: Mi perdo nei tuoi occhi azzurri come il ghiaccio.
- **Metafora**: Mi perdo nei tuoi occhi di ghiaccio.

### Metafore Nominali
Che hanno come paragone un nome:
- **Sei un coniglio** $\to$ *sei pauroso come un coniglio*
- **Un sacco di roba** $\to$ tanta roba

### Metafore Verbali
- **Ingannare l'attesa**
- **Sono piovute proteste**

### Metafora Enunciativa
- **Bisogna lavare i panni sporchi in casa**
- **Fare di tutte le erbe un fasscio**

### Metafore Morte - Catacresi
- **La linuga di fuoco** $\to$ la lava
- **Il collo della bottiglia** $\to$ la parte stretta della bottiglia
- **I tendti della segna** $\to$ la parte seghettata di una sega

# Metominia
Sostituire un termine con un altro, a patto che abbiamo un nesso logico
- **Hai studiato Gambosi?** $\to$ hai studiato machine learning?
- **Ce l'hai facebook?**
- **Ho comprato la Dyson**
- **Luca va a cento all'ora** $\to$ uso Luca per indicare la sua auto.
- **Andiamoci a fare un bicchiere** $\to$ uso il contenitore per indicarne il contenuto.
- **Sei senza cuore** $\to$ una qualità fisica per indicare una qualità morale.

-------------
Fin'ora abbiamo visto come, data una sequenza di parole **sintatticamente** corretta, e grazie [[#^246bb5|Principio di Composizionalità Semantica]], siamo in grado di ricavare la **semantica** di una frase.

Avvolte siamo in gradi di fare l'opposto, dando più peso al significato delle parole.
Per esempio se dico
	panino io mortadella mangio
Siamo in gradi di comprendere che io mangio un panino con la mortadella.
