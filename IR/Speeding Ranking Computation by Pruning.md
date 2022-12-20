Un **approccio generico** è quello di identificare un sottoinsieme $A$ di documenti **contenders**, di dimensione ragionevolmente piccola $k < \vert A \vert \ll N$, sui quali poi calcolare la funzione di [[Scoring, term weighting & the vector space model|scoring]] e restituire i primi $k$.

Se $A$ contiene tutti i top $k$ documenti con score massimo, allora avremo che questo metodo è [[Efficient Scoring#Safe vs non-safe ranking|safe]].
Ciò però non è detto che sia garantito, dipende dal metodo usato per computare $A$.

Possiamo pensare all'insieme $A$ come un **pruning** di documento *non-contenders*.

# Index Elimination
Un primo approccio per identificare l'insieme $A$ è noto come **Index Elimination**.
Questo metodo è molto semplice e consiste nel inserire in $A$ i soli documenti che presentano **almeno** un *query term*.

```ad-warning
Questo metodo non tiene affatto in considerazione il **significato** dei documenti.
Infatti potrei avere un documento che non presenta alcun query term, ma che però utilizza molti sinonimi dei termini della query.
Perciò avremo un documento potenzialmente molto rilevante che non sarà presente in $A$.
```

### High-IDF query terms only
Una prima ottimizzazione di questo metodo consiste nel tenere in considerazioni i soli termini con valore informati maggiore, ovvero quey termini $t$ della query con alto [[TF-IDF weight#^3ca620|IDF]].
Perciò se ho la query $q = \text{"the kaleidoscope"}$ considererò i soli documenti che hanno presente il termine `kaleidoscope`, in quanto il termine `the` ha un basso idf.

Il vantaggio di questo approccio è che verranno scartati parecchi documenti dall'insieme $A$.
Infatti i termini con basso IDF sono quelli che hanno posting list molto grandi.

### Many query terms
Un'altra ottimizzazione consiste nell'includere in $A$ solo documenti che hanno **molti** query term.

Per *"molti"* si intende una frazione tale che:
- non sia troppo grande, altrimenti l'insieme $A$ diventa troppo piccolo. Più $A$ è piccolo, minore è la probabilità che contengra i top $k$ documenti.
- non sia troppo piccola, altrimenti $A$ tende a crescere molto.

L'iportante è scegliere una quantità in modo tale che $A$ non sia troppo grande, ma comunque rimanga significativo rispetto all'insieme dei documenti rilevanti.

Tale intersezione *"morbida"* è anche nota come **soft conjuction**.


# Champion lists
L'idea di questo metodo è quella di precalcolare per ogni entry $t$ dell'indice i primi $r$ documenti con peso maggiore.
Tali $r$ elementi sono anche noti come **champion list**.

```ad-note
Questa operazione va necessariamente fatta in **indexing-time**.
Inoltre è anche possibile che $r < k$.
```

A seguito di una query viene fatta una [[#Many query terms|soft conjunction]] dei soli documenti nelle champion list dei query term più significativi.