# Online Paging and Resource Augmentation

# Online Paging
L'**Online Paging** (o **[[1 - Introduction#Caching problem LRU vs FIFO|Online Caching]]**) è un comune problema del mondo reale, abbastanza "semplice" però per dimostrare alternative all'analisi *worst-case*.

Richiamando la struttura del problema vista in [[1 - Introduction]], abbiamo:
1. Una memoria di archivio lenta, contenente $N$ **pagine**.
2. Una memoria molto veloce (la **cache**) che può contenere al più $k < N$ pagine nello stesso momento.
3. Le richieste delle pagine arrivano **online**, ovvero consecutivamente nel tempo. Indichiamo con $\sigma$ la sequenza di richieste.
4. Se la pagina $p_t$ richiesta al tempo $t$ è già presente, allora nessun costo aggiuntivo è richiesto.
5. Se invece $p_t$ non è presente nella cache al momento della sua richiesta, occorrerà un **page fault**.
	1. Nel caso in cui c'è spazio nella cache, nessun problema, basterà caricare la pagina $p_t$.
	2. Se invece la cache risulta satura, occorrerà adottare una **politica di sostituzione** de decidere quale pagina rimuovere per fare spazio a $p_t$.

In questo caso, per **efficienza** di un algoritmo (o *running time*) intederemo il numero di **page fault** che occorreranno.