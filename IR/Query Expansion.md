La **Query Expansion** è un altro metodo per aumentare la recall di un sistema di IR.

L'idea è quella di **modificare** la query fatta dall'utente utilizzando dei **[[Relevance feedback & Query expansion#^dcdf9e|tesauri]]** per migliorare la recall.

Un tesauro può essere visto come un **database** che per ogni termine associa una collezione di altri termini (**quasi-**)**sinonimi** rispetto al domini applicativo.

Più precisamente, per ogni *query-term* $t$ **espandiamo** la query con tutti quei termini che secondo il nostro tesauro sono **semanticamente** correlati a $t$.

Generalmente questo metodo aumenta la recall, in quanto si prendono in considerazione tutti quei termini non esplicitamente presenti nella query.
Purtroppo però c'è sempre il rischi di **diminuire la precision**, soprattutto nel caso in cui si utilizzano **termini ambigui**.

# Automatic thesaurus generation
Un tesauro può essere costruito e mantenuto **manualmente** o **automaticamente**.

Anche se un tesauro mantenuto manualmente è l'ideale, purtroppo risulta essere un processo troppo dispendioso.

Perciò è preferibile farlo automaticamente, analizzando statisticamente le distribuzioni dei termini nella collezione.

Esistono generalmente due definizione di **similitudine** tra due parole:
1. **Due parole sono simili quando co-occorrono con parole simili.** Infatti le parole `macchina` e `moto` sono simili perché appaiono spesso insieme alle parole `strada`, `benzina`, `patente`, ecc.. perciò devono essere simili.
2. **Due parole sono simili quando sono grammaticalmente correlate con parole simili**. Infatti una persona può `raccogliere`, `pelare`, `mangiare` sia una `mela` che una `pera`, perciò esse devono essere simili.

# Query log
Un altro metodo per espandere le query è quello di tenere traccia delle azioni fatte dall'utente a seguito di una query.

Per esempio, se un utente $X$ a seguito della query $\text{"piante"}$ cerca spesso la query $\text{"rimedi naturali"}$ si potrebbe inferire che `rimedi naturali` è una potenziale espansione del termine `piante` per l'utente $X$.

Un altro esempio è se a seguito di due query $q_1,q_2$ un utente $X$ clicca molto spesso sullo **stesso url**, possiamo ben pensare che $q_1$ e $q_2$ sono due potenziali reciproche espansioni per l'utente $X$.