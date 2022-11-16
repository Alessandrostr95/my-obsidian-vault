Abbiamo visto in precedenza che è utile rappresentare i documenti come:
- un [[Binary Term-Document Incidence Matrix|binary term vector]] in $\lbrace 0,1 \rbrace^{\vert V \vert}$
- un [[Bag of words model - Term Frequency tf#^6f3674|count term vector]] in $\mathbb{N}^{\vert V \vert}$

Alla luce di quanto visto fin ora, è più ragionevole definire una matrice di valori **reali**, dove alla cella $(i,j)$ è presente il [[TF-IDF weight|peso tf-idf]] $w_{i,j}$.

![](./img/IR_vector_space_model_1.png)

Tale matrice è anche detta **score matrix**, e ogni documento è ora rappresentato com un vettore in $\mathbb{R}^{\vert V \vert}$.

Grazie a questa rappresentazione, abbiamo che i nostri documenti ora sono dei **punti** o **vettori** in uno spazio $\vert V \vert$-dimensionale, dove i termini rappresentano le **dimensioni** o **assi** di questo spazio.

La lunghezza di ogni coordinata di un vettore che rappresenta un documento è quindi il peso tf-idf rispetto alla coordinata.

```ad-note
Osservare che tale spazio può avere un numero di dimensioni veramente esorbitante: anche decine di milioni se pensiamo al web.
In ogni caso, quando le dimensioni sono tante, i vettori sono per lo più **sparsi** (con molte entries nulle).
```

Secondo questo modello, possiamo rappresentare anche le **query come vettori**.
A questo punto potremmo definire un **rank** dei documenti in base alla loro "*prossibità*" al vettore-query.

Ma quale metrica di **distanza** conviene usare?
Per esempio la classica **distanza euclidea** potrebbe funzionare bene se i vettori avessero **magnitudo** simili.

Purtroppo però i documenti hanno migliaia di termini in più rispetto alle query (sono **molto meno sparsi**) perciò potrebbero essere davvero parecchio distanti dal vettore-query, anche se rilevanti alla fine del ranking.

![](./img/IR_vector_space_model_2.png)


