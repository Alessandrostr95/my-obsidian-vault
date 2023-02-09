Esistono varie [[Scoring, term weighting & the vector space model|rappresentazioni vettoriali]] delle parole, tutte purtroppo con una **dimensionalità** pari alla grandezza del **vocabolario** del nostro corpus, oppure del numero dei **documenti della collezione**.
Oltre alla dimensionalità enorme, tali rappresentazioni risultano anche estremamente **sparse**.

Il **word embeddig** invece è una rappresentazione molto più **densa** con dimensionalità $d$ molto **ridotte** (circa da 50 a 1000).
Generalmente le rappresentazioni **dense** funzionano meglio delle rappresentazioni sparese in tutti i task di NLP.

Non abbiamo delle motivazioni del perché le rappresentazioni  dense funzionano meglio, però abbiamo delle alcune intuizioni.
Innanzitutto osserviamo che avere un vettore di 300 dimensioni richiede molti meno **parametri** da **apprendere** in un **classificare**, piuttosto di un vettore con 50.000 dimensioni, e avere uno **spazio di parametri** molto più piccolo aiuta alla **generalizzazione** ed evita l'**overfitting**.
Infatti, con più parametri è più facile ottenere una funzione che si avvicina a quella **reale** che si desidera ottenere.

I vettori densi possono anche essere utili per catturare la [[Word Senses#Sinonimia|sinonimia]].
Infatti con una rappresentazione **sparsa** le dimensioni in cui il vettore `car` e `automobile` sono <u>non nulle</u> sono *statisticamente* molto scorrelate, perciò non catturano la loro relazione di [[Word Senses#Sinonimia|sinonimia]].

# Skip-Gram with Negative Sampling
In realtà Word2Vec è un **pacchetto softoware** contente due algoritmi, uno dei quali è noto come **skip-gram with negative sampling** (in breve **SGNS**).

```ad-info
In realtà comunemente ci si riferisce all'algoritmo **SGNS** semplicemente con **word2vec**.
```


