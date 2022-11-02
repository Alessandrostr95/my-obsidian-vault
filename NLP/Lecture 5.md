---
date: 2022-10-25
draft: true
---

# Valutare un modello e un sistema (di NLP)
- Si vuole realizzare un **modello per un linguaggio**.
- Si vuole realizzare un **sistema per l'analisi di un linguaggio**.

Data una frase $f$, costruiamo una interpretazione $I_M(f)$ secondo il modello $M$.
Analogamente, data una frase $f$, costruiamo una interpretazione $I_S(f)$ secondo il sistema $S$.
Sarebbe desiderabile che $I_M(f)$ e $I_S(f)$ siano correlate.

### Computational Linguistic
Il primo approccio per costruire un sistema NLP è quello di prendere un modello, per esempio una **grammatica formale**, e la implemento.
Il problema è che non mi pongo il problema del **significato** di ciò che costruisco.
La mente unama però cerca un significato in ciò che esprime.

Per esempio, definisco la **regex** per definire le date, del tipo `num num - num num - num num num num`.
In questo caso, potrei generare una frase che rappresenta una data italiana ma non americana, oppure peggio ancora qualcosa che non è una data (e.g. `00-00-0101`).
Peggio ancora, potrei riconoscere come data qualcosa che non lo è, per esempio una strozzaione `99-10-1000`.

Questo è un **modello deduttivo**, in quanto dico alla macchina come e cosa fare per costruire un linguaggio.


```ad-important
Noi diamo per scontato l'esistenza di un **modello linguistico**.
Il modello linguistico è un **artefatto**, più del linguaggio stesso.

Infatti, a un bambino gli si insegna il modello grammaticale dopo che esso sa già parlare.

Perciò qualsiasi modello grammaticale è **opinabile**.
```

-----
Per vedere se un sistema linguistico funziona correttamente, abbiamo bisogno di chiedere a delle **persone** di generare frasi utili per verificare se il nosto sistema è corretto.

Per fare ciò, dobbiamo definire dell **guide lines** che definiscano la struttira del nostro sistema.
Purtroppo però, ogni persona ha a sua volta una propria interpretazione delle GL.

Date tutte le differenti interpretazioni, si vuole trovare quella che più è *"corretta"* rispetta al modello.
Purtroppo non esiste un concetto di **correttezza delle interpretazioni**.
Perciò è necessario ricorrere in un **accordo**.

------
### Come costruire un DataSet
Supponiamo di avere una frase $f$, e di voler avere una sola interpretazione, per esempio una **etichetta** di *positività/negatività* del sentimento di una frase.
Supponiamo per semplicità di avere solo 2 annotatori.

Sia $C$ l'insieme di frasi a disposizione, misuriamo l'accordo tra le due persone calcolando il numero di frasi per le quali hanno le stesse interpretazioni
$$A_E = \frac{\sum_{f \in C} \left[ I_1(f) \equiv I_2(f) \right]}{\vert C \vert}$$
$A_E = \text{accorod effettivo}$

```ad-attention
title: Problema
Potrebbe capitare però che gli annotatori siano d'accordo **per caso**, e non oggettivamente.
```

Per *"pulire"* l'accordo effettivo $A_E$, possiamo sottrarre un **accordo** $A_C$ e normalizzarlo.
Questo è detto **k inter annotator agreement** ^d49e53

$$A_C = P(I_1(f) \equiv I_2(f))$$
$$\kappa = \frac{A_E - A_C}{1 - A_C}$$


Se $\kappa$ è alto, allora vuol dire che le **guide lines** sono definite bene, a patto che gli annotatori si comportino bene.

```ad-seealso
- $\kappa$-statistics, formula generale con più annotatori.
```

Se $\kappa > 0.4$ allora definiamo il modello come **buono**.
Se invece $\kappa \leq 0.4$ allora dobbiamo pensare a rivedere il modello.

- definisco delle GL.
- faccio annotare un sottoinsieme (ragionevolmente piccolo) di frasi ai miei annotatori.
- calcolo $\kappa$.
- e vedo se $\kappa$ è buono possiamo far annotare tutto il corpus $C$ agli annotatori.

```ad-failure
title: Problema
Prutroppo, noi stiamo assumendo che il modello del linguaggio è **unico**, ovvero esiste una singola interpretazione di una frase $f$.

Purtroppo non è così, in quanto esistono differenti interpretazioni di una stessa frase, ed entrambe **corrette**.
```

Perciò, finito il task dell'etichettamento, otterremo un "**oracolo**" $O$ (o **gold standard**) che possiamo usare per fare testing o training.
Nel nostro caso, possiamo usare l'oracolo $O$ come **fonte di verità** per misurare la qualità del nostro sistema $S$.

Per esempio otteniamo come qualità $85\%$.
Come facciamo a dire che è buono?
Ci servirebbe un **metro di paragone** sulla qualità, che ovviamente dipende dal **problema in questione**.

Per prima cosa prendo un **sistema base line**, ovvero uno semplice da implementare.
Per esempio:
- uno aleaotirio, che classifica a caso le frasi del corpus
- partiziono le parole della frase, le uso come **features** e poi clusterizzo le frasi

Se per esempio il mio sistema base line ha una precisione dell'$82\%$, posso dire che il mio sistema è buono?
