L'idea base del **Relevance Feedback** consiste nel far marcare dall'utente che sottopone la query i documenti restituiti come **rilevanti** o **non rilevanti**.
Dopodiché sfruttare questa etichettatura fatta *on-the-fly* per eseguire una nuova query nella speranza di migliorare il risultato.

Volendo questo processo può essere iterato svariate volte finché non si raggiunge il risultato che l'utente desidera.

```ad-tldr
title: Ad hoc retrieval
I sistemi di IR che non sfruttano il **relevance feedback** sono spesso detti **ad hoc retrieval**.
```


I documenti contrassegnati come come rilevanti dall'utente introducono **nuovi termini** i quali possono essere sfruttati per migliorarre il risultato.
L'intuizione dietro è che:
> Se ci sono dei termini usati spesso insieme ai miei *query-term* vuol dire che essi sono in qualche modo **correlati** semanticamente, perciò posso sfruttarli nella nuova query.


# Visualizzazione Geometrica
Il relevance feedback ha dietro una rappresentazione geometrica che può aiutare a spiegare perché dovrebbe funzionare.

Definiamo come **vettore centroide** di un qualsiasi inieme $D$ di documenti come la **media aritmetica** dei vettori che rappresentano i documenti di $D$, ovvero $$\vec{\mu}(D) = \frac{1}{\vert D \vert} \sum_{d \in D}\vec{d}$$

Sia $C$ l'intera nostra collezione di documenti, e assumiamo di conoscere a priori $C_r, C_{nr}$ la partizione di documenti **rilevanti** e **non rilevanti** rispettivamente.

La query ottima **idealmente** è quella che massimizza la **similitudine** col centroide di $C_r$ e minimizza quella col centroide di $C_{nr}$.
$$\vec{q}_{\text{opt}} = \arg \max_{\vec{q}} \lbrace \text{sim}(\vec{q}, \vec{\mu}(C_r))- \text{sim}(\vec{q}, \vec{\mu}(C_{nr})) \rbrace$$

Se come misura di similitudine applichiamo la [[Vector Space Model#^eef545|cosine similarity]] avremo quindi che $\vec{q}_{\text{opt}}$ è il vettore che **massimizza** la quantità $$\vec{q}\cdot\vec{\mu}(C_r) - \vec{q}\cdot\vec{\mu}(C_{nr}) = \vec{q} \cdot (\vec{\mu}(C_r)-\vec{\mu}(C_{nr}))$$
Ovvero in queto framework la query ottima consiste in $$\vec{q}_{\text{opt}} \equiv \vec{\mu}(C_r)-\vec{\mu}(C_{nr}) = \frac{1}{\vert C_r \vert}\sum_{d \in C_r} \vec{d} - \frac{1}{\vert C_{nr} \vert}\sum_{d \in C_{nr}} \vec{d}$$

Tale equivalenza è ragionevole, in quanto teniamo in considerazione tutti i documenti rilevanti grazie al centroide $\vec{\mu}(C_r)$ e **scartiamo** quelli non rilevanti semplicemente **sottraendo** $\vec{\mu}(C_{nr})$.

Purtroppo però in un contesto reale non conosciamo la partizione $C_r, C_{nr}$, perciò sfruttiamo il **relevance feedback** degli utenti.

L'**algoritmo di Rocchio** è un **metodo iterativo** che implementa il relevance feedback nel [[Vector Space Model]].

Sia $\vec{q}_0$ la query **iniziale** dell'utente, e siano $D_r, D_{nr}$ il relevance feedback dell'utente.
**Iterativamente** possiamo migliorare la query con la seguente formula $$\vec{q}_{k+1} = \alpha \cdot \vec{q}_k + \beta \cdot \vec{\mu}(D_r) - \gamma \cdot \vec{\mu}(D_{nr})$$ dove $\alpha, \beta, \gamma$ sono dei **pesi** che possono essere impostati a piacere, a seconda se si desidera dare più peso alla query ($\alpha$), più peso a quelli contrassegnati come rilevanti ($\beta$) o "*allontanarsi*" di più da quelli contrassegnati come non rilevanti ($\gamma$).

# Quando funziona bene?
Innanzitutto ragioniamo sotto quali **assunzioni** il relevance feedback aumenta la [[Set Based Measures#^eed895|recall]].

Certamente tale metodo funziona bene qualora l'utente che espone la query al sistema conosce **bene** il vocabolario dei termini della nostra collezione.
Infatti se nella query ci sono **sinonimi** che però non sono molto presenti nella collezione, potrebbe non migliorare di molto la situazione.

Un'altra assunzione è che **documenti simili hanno molti termini in comune**.
Se così fosse, allora è evidente che il relevance feedback è utile per migliorare la query.
Questo però non è sempre detto, in quanto esistono svariati modi di dire uno stesso concetto, e potrebbe capitare che i documenti nella collezione esprimano concetti simili ma con termini totalmente differenti.

# Problematiche
Una delle principali problematiche è che tale metodo è **computazionalmente dispendioso**.
Infatti, una query semplice ha generalmente una **dimensionalità bassa**, e quindi pochi termini da dover processare.
Invece, le query generate usando il relevance feedback sono molto più dense di termini, perciò molto **dispendiose** da computare: devo processare troppe posting list.

Un'altra problematica molto importante è che generalmente l'utente è riluttante del dare un feedback ai documenti.
Infatti per dare un feedback necessità prima di leggere i documenti in questione, e ciò richiederebbe molto tempo.
Per questo motivo tale metodo non è usato in pratica.

# Pseudo-relevance feedback
Si può quindi **simulare** il meccanismo di feedback umano sfruttando il [[Scoring, term weighting & the vector space model|ranking]].
A seguito della query dell'utente prendiamo i top $k$ risultati nel ranking, e li contrassegnamo come **rilevanti** (come se fosse stato l'utente a farlo).
Dopodiché applichiamo l'algoritmo di Rocchio per migliorare la query, quante volte vogliamo.

In media questo metodo funziona molto bene, però è molto rischioso in quanto se il ranking non è fatto bene si rischia che la query **diverga** verso direzioni errate, aumentando così di fatto i **falsi positivi**.