Le metriche viste in [[Rank-Based Measures]] erano tutte metriche basate su un concetto **binario** di rilevanza: si teneva in considerazione se un termine era rilevante o meno.
Più precisamente abbiamo visto misure basate solamente sulla [[Set Based Measures|precision e recall]].

Per molte applicazioni reali, in particolare per il web, per un utente è più importante che ci siano buoni risultati sulla **prima pagina**, anziché sulle prime 3 o 10.
Infatti nella realtà esistono varie sfumature di rilevanza, spesso associate alla semantica della query.

-----
# DCG - Discounted Cumulative Gain
Il **DCG** (**Discounted Cumulative Gain**) è una misura molto importante nella valutazione di sistemi di IR per il web.

L'idea è che gli utenti sono generalmente **pigri**, e che quindi faranno uno **sforzo** maggiore nell'andare a usufruire docuementi che sono infondo alla prima pagina (o peggio ancora nella seconda).
Infatti generalmente se un utente non trova ciò che gli interessa tra i primi 3 o 4 documenti del risultato, rinuncia alla ricerca e assume che non c'è ciò che gli serve.

Perciò è ragionevole assumere che:
1. i documenti più **rilevanti** sono i più **utili** all'utente.
2. più un documento rilevante è posto in basso alla classifica, meno sarà utile (in quanto l'utente potrebbe non arrivare oltre i primi 3 o 4).

Perciò, mentre prima nel [[Evaluation of IR systems#^f2d730|gold standard]] avevamo una **rilevanza booleana**, adesso abbiamo una valutazione continua in un intervallo $\left[ 0, r \right]$, con $r > 2$.

Un primo approccio (banale) è quello di valutare la qualità di una query in maniera **cumulativa** rispetto all'utilità dei primi $n$ documenti ritornati dal mio sistema.
Siano quindi $r_1, ..., r_n$ i valori di utilità dei miei primi $n$ documenti ritornati dopo una query.
Definiamo quindi il **guadagno cumulativo** (o **Cumulative Gain** o **CG**) la somma $$CG = r_1 + ... +r_n$$

Dato che abbiamo detto che più un documento è in fondo al rank e minore sarà la sua utilità, possiamo **scalare** il contributo dei documenti rispetto alla loro posizione nel rank.
Definiamo quindi il **guadano cumulativo scontato** (o **Discounted Cumulative Gai** o **DCG**) come $$DCG = r_1 + \frac{r_2}{\log{2}} + \frac{r_3}{\log{3}} + ... + \frac{r_n}{\log{n}} = r_1 + \sum_{i = 2}^{n}\frac{r_i}{\log{i}}$$

```ad-info
Un'altra formula usata da alcuni motori di ricerca è la seguente $$DCG = \sum_{i=1}^{n} \frac{2^{r_i} - 1}{\log{(i+1)}}$$
```
