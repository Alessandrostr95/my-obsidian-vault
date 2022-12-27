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

