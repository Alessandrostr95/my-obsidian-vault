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

Una prima ottimizzazione di questo metodo consiste nel tenere in considerazioni i soli termini con valore informati maggiore, ovvero quey termini $t$ della query con alto [[TF-IDF weight#^3ca620|IDF]].
Perciò se ho la query $q = \text{"the kaleidoscope"}$ considererò i soli documenti che hanno presente il termine `kaleidoscope`, in quanto il termine `the` ha un basso idf.


