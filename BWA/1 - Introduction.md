# Beyond Worst-Case Analysis
Confrontare algoritmi diversi è difficile.
Per quasi tutte le coppie di algoritmi, e per qualsiasi misura di prestazioni dell'algoritmo come il **tempo di esecuzione** o la **qualità della soluzione**, ogni algoritmo funzionerà meglio dell'altro su alcuni input.
Ad esempio, l'algoritmo di ordinamento **insertion sort** è più veloce del **merge sort** su array già ordinati, ma più lento su molti altri input.

Quando due algoritmi hanno prestazioni incomparabili?
Come possiamo ritenere uno di loro "**migliore**" dell'altro?

L'analisi **worst-case** è uno dei modelli più usato per l'analisi della qualità di un algoritmo.
Nel modello *worst-case*, le prestazioni complessive di un algoritmo sono riassunte dalle sue **peggiori prestazioni** su qualsiasi input: si analizzano le prestazioni nel **caso peggiore**.
L'algoritmo *"migliore"* è quindi quello che ha le prestazioni migliori nel caso peggiore.

Il *merge sort*, con il suo tempo di esecuzione (**asintotico**) nel caso peggiore di $\Theta(n \log{n})$ per gli array di lunghezza $n$, è migliore in questo senso rispetto all'*insertion sort*, che ha un tempo di esecuzione peggiore di $\Theta(n^2)$.

L'analisi *worst-case* risulta estremamente utile, ed è il paradigma dominante per l'analisi degli algoritmi nell'informatica teorica.
Un algoritmo con buone prestazioni nel caso peggiore garantisce una buona qualità per qualsiasi input possibile.


