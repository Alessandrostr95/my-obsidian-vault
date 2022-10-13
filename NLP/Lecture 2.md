---
date: 2022-10-13
draft: true
---

Linugaggio:
- **Naturale**: creato dall'umanità.
- **Artificiale**: create dell'uomo.

La lingua è in **continua evoluzione**: si creano nuove parole, scompaiono, o cambiano di significato col tempo.


- **NLP**: derivante dall'**informatica** (problema far parlare le macchine)
- **IR** (dalla biblioteconomia) (grandi moli di dati)
- **TextMining** -> Web Mining and Retrieval (dalla statistica) (trovare informazioni da un'enorme quantita di dati)
- **Computational Linguistic** (dalla linguistica) (portare i modelli linguistici nelle macchine)

Il linguaggio nasce dall'esigenza di scambiare informazioni più articolate. (**capacità interna**).

La **linguistica interna** è composto da
- **Compotenza** le capacità (*cognitive*) che consentono di sviluppare e comprendere un linugaggio. *Ciò che l'individuo sa* (quali sono le frasi grammaticali)
- **Performance** il linguaggio stesso. *Ciò che l'individuo fa, a partire dalla sua competenza*.

Chomsky ha cercato di individuare quale **competenza innata** ogni individuo ha, che gli consente di sviluppare un linguaggio.
Esso ha provato tramite l'uso di **macchine** (o **automi**) che descrivevano la struttura dei linguaggi.
Questi sono detti **linguaggi artificiali**.


Bisogna anche mettersi d'*accordo* sul dare il significato al linguaggio: questa è una **convenzione**, **capacità esterna**.

**Ferdinand De Soussure** diceva che esiste una lingua che deriva dall'accordo delle persone, nota come **Langue** [fr].
Il contrattare della *Langue* è la **Parole** [fr], ovvero la produzione linguistica in funzione di una Langue.

Se facciamo ML (apprendimento **deduttivo**) prendiamo un dataset di **Parol**.
Però non conosciamo la convenzione che ci sta dietro, perciò non conosciamo la **Langue**.
Perché una *parole* può nascondere al suo interno molteplici *lange*.


Le modalità attraverso cui la lingua si evolve sono:
- il **luogo** (ovvero sottoinsieme d'individui) $\rightarrow$ cambiamento **diatopico**
- il **tempo** $\rightarrow$ cambiamento **diamesico**
- il **mezzo** di comunicazione $\rightarrow$ cambiamento **diacronico**

```ad-important
Se faccio una macchina che ha la **competence** di padroneggiare il linguaggio, senza tener conto del **tempo**, dopo poco tempo sarà inutile.

Se faccio una macchina che ha la **competence** di padroneggiare il linguaggio, senza tener conto del **luogo** (sottoinsieme di persone) in cui verrà usato, risulterà mal funzionante o inutile.
```

> Noi ci scambiamo **informazioni** sulla **realtà** tramite le **parole**.

### Teoria 1
La **realtà** è un oggetto da rappresentare, detto anche **significiato**.
Per rappresentarlo si usano dei **simboli** (o **parole**), dette **significanti**.

```ad-example
Quando programmiamo avremo che

`print("Hello World")`

è un **significante**.
Mentre l'operazione di scrivere su stdout è il **significato**.
```

Nel significato ci sono 2 elementi:
- il **referente**, ovvero l'oggetto effettivo nel mondo.
- il **senso**, ovvero il **concetto** di tale oggetto.

Il simbolo è anche noto come **veicolo segnico**.

Il triangolo `senso-referente-veicolo segnico` è detto **triangolo semiotico di Pierce**, o in beve **segno**.

I **veicoli segnici** sono composti da una versione:
- **analogica**, ovvero qualcosa che è in *analogia* con la realtà che vuole rappresentare (per esempio il disegno di un fiore è un simbolo analogico perché rappresenta il suo referente fiore) (esempio lingua cinese)
- **simbolica** (la nostra lingua è simbolica, perché la lettera `a` non è in analogia con la realtò, e nemmeno le parole)
	- **arbitrarietà** dare il significato che si vuole alle parole 

## Fenomeni del linguaggio naturale
### Fenomeno ambiguità
Quando un **significante** ha più **significati**.

- **ambiguità lessicale**: quando una parola ha più significato
- **ambiguità strutturale**: quando una frase può essere letta/interpretata in più modi. `La borsetta di leopardo` può essere interpretato come "*una borsa di pelle di leopardo*" oppure "*la borsa di un leopardo*"

Un linguaggio artificiale è tale quando non ha abiguità.

### Ricchezza espressiva
Quando un **significato** può essere espresso da molteplici **significanti**.
Ovvero i **sinonimi** o **modi di dire**.

### Conclusione
In un linguaggio artificiale si preserva la **ricchezza espressiva** e si azzera l'**ambiguità**.

