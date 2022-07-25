# Knapsack Auctions
## Problem Definition
Supponiamo di essere un'emittente televisiva, e di voler vendere uno *spazio pubblicitario* durante la finale del [Super Bowl](https://en.wikipedia.org/wiki/Super_Bowl) a diverse ditte interessate.
Per fare ciò avviamo un'asta.

Abbiamo una quantità $W$ di tempo disponibile, e vogliamo decidere come spartirlo tra i partecipanti.
Ogni partecipante $i$ possiede una quantità $w_i$ che indica la **durata** del proprio spot pubblicitario, e un una propria **valuitazione privata** $v_i$ che indica quanto secondo loro vale mostrare il proprio spot plubbicitario durante la finale del Super Bowl.

L'insieme delle soluzioni ammissibili $X$ è composto da quei vettori $n$-dimensionali $\underline{x} = (x_1, ..., x_n)$ composti da soli 0-1 tali che
$$
\sum_{i=1}^{n} w_i \cdot x_i \leq W
$$

^adc6eb

Ciò significa che l'insieme delle pubblicità scelte per essere mostrate non deve superare il tempo a disposizione $W$.

Osserviamo che un'asta di $k$ identici oggetti è un caso particolare di quest'asta dove $w_i = 1$ per ogni partecipante $i$, e con $W = k$.

Quello che si vuole massimizzare è il guadagno ottenuto dall'asta, cercando però di rispettare il [[#^adc6eb|vincolo d'integrità]] (non posso vendere 13 minuti di pubblicità se ho solo 10 minuti disponibili, sarebbe una truffa...).

Più formalmente, ogni partecipante all'asta propone un'*offerta* $b_i$ e la *regola di derivazione* che massimizza il **social surplus** è la seguente
$$
\mathbf{x}(\mathbf{b}) = \max_{x \in X} \sum_{i = 1}^{n} b_i \cdot x_i
$$
Questa appena descritta è una istanza del problema combinatorico di ottimizzazione [Knapsack problem](https://en.wikipedia.org/wiki/Knapsack_problem).

## Soglia critica di pagamento
Il [[3 - Myerson’s Lemma#Theorem Mayerson's Lemma|lemma di Myerson]] ci garantisce l'**esistenza** di una configurazione di pagamenti $\mathbf{p}$ tale che il meccanismo $(\mathbf{x}, \mathbf{p})$ è [[2 - Mechanism Design Basics#^a63f67|DSIC]] (grazie al fatto che $\mathbf{x}$ è [[3 - Myerson’s Lemma#^3c29c4|monotona]]).

Fissiamo un player $i$ e le offerte $\mathbf{b}_{-i}$.
Dato che la regola di allocazione è *binaria* ($x_i$ può essere solo $0$ o $1$) allora l'andamento di $x_i(\cdot, \mathbf{b}_{-i})$ è facile da intuire: vale sempre $0$ finché non si raggiunge una **soglia critica** $\theta_i$ dalla quale in poi è sempre $1$.

Ricordando la [[3 - Myerson’s Lemma#^232507|formula]] avremo che il pagamento sarà pari a $0$ per ogni offerta $b_i < \theta_i$, e pari a $\theta_i \cdot (1 - 0) = \theta_i$ per ogni $b_i \geq \theta_i$, un po' come accadeva nella [[2 - Mechanism Design Basics#Vickrey's Second-Price Auctions|Vickrey's second auction]].

## Intrattabilità del problema
È ben noto che il problema Knapsack è un problema NP-Hard.
Perciò, a meno che non sia $P = NP$, non c'è speranza di soddisfare *entrambe le proprietà* [[2 - Mechanism Design Basics#^db3c63|social surplus maximization]] e [[2 - Mechanism Design Basics#^92a5ec|polynomial running time]].

Perciò l'idea è quella di **rilassare** una delle due proprietà, rinunciando quindi ad *efficacia* o *efficienza*.

Ma cosa è possibile rinuciare per garantire la terza proprietà necessaria, ovvero [[2 - Mechanism Design Basics#^a63f67|DSIC]]?

Per esempio, è ben noto un algoritmo [pseudo-polinomiale](https://it.wikipedia.org/wiki/Tempo_pseudopolinomiale) di [programmazione dinamica](https://en.wikipedia.org/wiki/Dynamic_programming) che risolve il problema del Knapsack all'*ottimo* in tempo $O(nW)$.
Questo algoritmo però rinuncia all'efficienza perché $W$ rappresenta un **valore numerico** e non la grandezza dell'istanza, perciò sono necessari $O(\log{W})$ bit per rappresentarlo.
Quindi $W$ è **esponenziale** nella sua *"grandezza"*.

Un altro approccio invece è quello di preservare l'efficienza perdendo in ottimalità, ma **non troppo**.
Algoritmi che appliccano questo approccio sono detti [algoritmi approssimanti](https://it.wikipedia.org/wiki/Algoritmo_di_approssimazione).

Per esempio un algoritmo approssimante per il knapsack segue la seguente euristica greedy:

Assumiamo che tutti i pesi siano $w_i \leq W$.
Se ce ne dovessero essere di più pesanti basta rimuoverli.
A questo punto:
1. Ordianiamo (e indiciziamo) gli elementi nel seguente ordine $$\frac{b_1}{w_1} \geq \frac{b_2}{w_2} \geq ... \geq \frac{b_n}{w_n}$$
2. Iterativamente, seguendo l'ordinamento, inserisci un player all'interno della soluizione a meno che non *"entri nello zaino"*.
3. Ritorna il massimo tra il miglior offerente e ciò che si è ricavato dal punto 2.

```julia
function greedy_knapsack(bids::Vector{Float64}, weights::Vector{Float64}, W::Float64)
    rates = Tuple[]
    for i = 1:length(bids)
        append!(rates, [(bids[i] / weights[i], i)])
    end

    sort!(rates, by = x -> x[1], rev = true)

    result = Float64[]
    current_load = 0.0

    for t ∈ rates
        i = t[2]
        b = bids[i]
        w = weights[i]

        if current_load + w ≤ W
            append!(result, b)
            current_load += w
        end
    end

    max(bids...) > sum(result) ? [max(bids...)] : result
end
```

^adfe61

Quello appena descritto è un algoritmo $\frac{1}{2}$-approssimante per il problema del Knapsack, ovvero garantisce che (nel caso peggiore) il risultato trovato è almeno la metà del valore del risultato ottimo.

> **Theorem** Assumendo che le offerte sia sincere, ovvero che $b_i = v_i$ per ogni partecipante, allora il *social surplus* ottenuto tramite l'algoritmo greedy è *almeno* il 50% del *massimo social surplus*.

> **Proof**  **TODO**

Questo approccio greedy funziona anche meglio sotto certe assunzioni.
Per esempio, se per ogni partecipante $i$ abbiamo che $w_i \leq \alpha W$, con $0 < \alpha \leq \frac12$, allora l'algoritmo garantisce un'approssimazione di $1-\alpha$ (anche omettendo il terzo punto).

Si può dimostrare che massimizzare il social surplus induce in un algoritmo $\mathbf{x}$ [[3 - Myerson’s Lemma#^3c29c4|monotono]] (vedi [[Exercise Set 2#Exercise 14|esercizio 14]]), il quale a sua volta implica l'**esistenza** di uno schema di pagamento $\mathbf{p}$ tale che il meccanismo $(\mathbf{x},\mathbf{p})$ è DSIC.
$$
\text{exact surplus-maximization} \implies \text{DSIC/monotone "for free"}
$$
Cosa possiamo dire invece riguardo l'**approssimazione** del social surplus? Si può garantire la *monotonia*? (vedi [[Exercise Set 2#Exercise 18|esercizio 18]])

## Black-Box Reductions
Verrebbe da pensare che qualsiasi regola di allocazione ragionevole è necessariamente anche *monotona*.
In realtà non è sempre detto.

Per esempio, per knapsack, esiste un [FPTAS](https://en.wikipedia.org/wiki/Fully_polynomial-time_approximation_scheme) che dato un $\varepsilon > 0$ calcola una $(1-\varepsilon)$-approssimazione in tempo polinomiale nell'istanza e in $1/\varepsilon$, inducendo però in una regola di allocazione **non**-monotona.

È però possibile **riarrangiare** tale regola ottenuta tramite l'FPTAS in modo tale da garantire la monotonia, <u>senza degradare l'efficienza</u>.

Quello che si può chiedere è quindi:
> Esiste un problema a **singolo parametro** per il quale la migliore approssimazione garantita ottenuta tramite un algoritmo polinomiale è **strettamente migliore** della migliore approssimazione garantita ottenuta tramite un algoritmo polinomiale e *monotono*?

O più semplicemente:
> esiste un problema a singolo parametro per il quale il miglior risultato che si può ottenere in tempo polinomiale è **strettamente migliore** del miglior risultato che si può ottenere sfruttando entrambi tra tempo polinomiale e monotonia?

Un modo per dimostrare una risposta negativa sarebbe attraverso una "**black-box reduction**":
un modo **generico** che dato un algoritmo polinomiale non-monotono genera un algoritmo polinomiale monotono senza *degradare* il fattore di apporssimazione.
Questo risultato sarebbe fantastico in quanto significherebbe che se un problema a singolo parametro è apporssimante allora si può garantire *"gratis"* anche le proprietà di monotonia e DSIC!

Tale riduzione black-box sarebbe interessante anche se la trasformazione genererebbe un algoritmo polinomiale e monotono con un fattore di approssimazione degenrato di un fattore **costante**.

Purtroppo nel 2012 è stato dimostrato che non è possibile definire un metodo **generale**  di riduzione black-box per i tipi di porblemi a singolo parametro appena citati (vedi [qua](https://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.307.8197&rep=rep1&type=pdf)).
D'altro canto esistono larghe classi di problemi a singolo parametro per i quali è possibile definrie una riduzione black-box (vedi Exercise 16).

-------------------
# The Revelation Principle
Fin ora ci siamo interessati di meccanismi [[2 - Mechanism Design Basics#^a63f67|DSIC]], e questo è ragionevole per alcuni validi motivi:
1. Per ogni player è facile definire quale strategia adottre: basta adottare la strategia dominante (ovvero dichiarere il vero).
2. Il designer invece può facilmente predire il risultato dell'asta sotto la (forte) assunzione che tutti i partecipanti adottino la strategia dominante.

Ciò che si può chiedere è:
> Possono i meccanismo non-DSIC garantire proprietà che i meccanismi DSIC non possono?
> Ovvero, sono anche utili da studiare i meccanismi che non sono DSIC?

Per cercare di dare una risposta, iniziamo col osservera un paio di assunzione che abbiamo dato per scontato nei meccanismi DSIC:
- (a) Ogni partecipante ha **sempre** una strategia dominante, indipendentemente dalla sua valutazione privata. ^4a68e4
- (b) Questa strategia dominante è la **direct revelation** (ovvero dichiarare il vero). ^c7388e

Per esempio, esistono meccanismi nei quali non esiste una strategia dominante, come la [[2 - Mechanism Design Basics#First-Price Auctions|first-price auction]] oppure la [morra cinese](https://it.wikipedia.org/wiki/Morra_cinese).

Oppure ancora esistono meccanismi per i quali vale [[#^4a68e4|(a)]] ma non [[#^c7388e|(b)]].
Per esempio, supponiamo di avere un'[[2 - Mechanism Design Basics#Single item auction|asta a singolo oggetto]], dove i partecipanti dichiarano $\mathbf{b}$ e di eseguire la [[2 - Mechanism Design Basics#Vickrey's Second-Price Auctions|Vickrey's auction]] con offerte $2\mathbf{b}$.
In tal caso la strategia dominante non sarebbe pià quella di dichiarare la propria valutazione privata $v_i$, bensì la sua metà $v_i/2$.

### Beyond Dominant-Strategy Equilibria
In pratica spesso accade di trovarsi in contesti in cui non esistono *strategie dominanti*.
Perciò è ragionevole chiedersi che succede se rilassiamo il punto [[#^4a68e4|(a)]].

In tal caso è necessario assumere delle proprietà più forti, ovvero bisogna assumere la presenza di [equilibri di Nash](https://en.wikipedia.org/wiki/Nash_equilibrium#Nash_Equilibrium) o [equilibri di Bayes-Nash](https://en.wikipedia.org/wiki/Bayesian_game).

Dato quindi che assumiamo la presenza di equilibri, ci si può chiedere se si possono progettare meccanismi migliori di quelli [[2 - Mechanism Design Basics#^a63f67|DSIC]].
La risposto a questa domanda è *"a volte"*.

Per questa ragione (e anche perché i meccanismo non-DSIC sono abbastanza comuni in pratica) è importante nello studio del mechanism design tenere in considerazione e progettare meccanismi al di la di quelli DSIC.

### The (dominant-strategy) Revelation Principle and the Irrelevance of Truthfulness
Il **Revelation Principle** afferma che se è presente la proprietà [[#^4a68e4|(a)]] allora la proprietà [[#^c7388e|(b)]] è automaticamente grantita (*"for free"*).

Più formalmente

> **Theorem 3.1 (Revelation Principle)** For every mechanism $M = \langle \mathbf{x}, \mathbf{p} \rangle$ in which every participant has a **dominant strategy** (<u>no matter what its private information</u>), there is an equivalent direct-revelation DSIC mechanism $M'$.

^ec0935

In breve ciò che dice il *pricipio di rivalazione* è che se riesci a progettare un meccanismo che ha delle strategie dominanti allora può progettare un meccanismo la cua strategia dominante è la diretta rivelazione delle proprie informazioni private (ovvero conviene sempre dichiarare il vero).

> **Proof:** Supponiamo che per ogni partecipante $i$, e per ogni possibile informazione privata $v_i$ che esso può avere, esiste una *strategia dominante* $s_i(v_i)$ che esso può adottare nel meccanismo $M$.
> 
> Costruiamo ora un meccanismo $M'$.
> $M'$ prende in input le offerte $b_1, ..., b_n$ dei partecipanti $1, ..., n$.
> Dopodiché esso sottopone al meccanismo $M$ i valori $s_1(b_1), ..., s_n(b_n)$.
> Infine l'outcome di $M$ sarà dato come outcome di $M'$.
> ![|500](lecture04-1.png)
> Si può dimostrare che $M'$ è DSIC.
> 
> Infatti se un player $i$ ha come valutazione privata $v_i$, se esso propone $b_i=v_i$ allora il meccanismo $M'$ sottometterà ad $M$ il valore $s_i(v_i)$, ovvero la strategia *migliore* che può applicare.
> Perciò offrendo un valore differente da $v_i$ l'utilità di $i$ non può migliorare $\square$.
