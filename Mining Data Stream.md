Abbiamo un'unica unità di calcolo, però l'input non può essere acceduto nella sua interezza.
L'input arriva come uno **stream** di dati.
Il computer non riesce a tenere in ram tutto l'input.
Diciamo che riesce a tenere delle piccole quantità **sublineari** di input.

Abbimo uno strem di dati, e vogliamo fare delle **query** su questo stream (estrarre infomrazione, magari mediante statistiche).

Non possiamo definire una **distribuzione stazionaria** (implicita o esplicita) sullo stream in input.
Lo stream è **avversariale**.

- **Tupla**: un **elemento** dello stream, un **record omogeneo**.
- Possiamo avere in ram solo degli **sketches** dello stream, una piccola parte.
- Possiamo avere più stream, e possiamo anche **combinarli**.

- Abbiamo una **ram** **esponenzialmente più piccola** rispetto allo stream
- Un disco più grande rispetto alla ram, ma più lenta.

Domande:
- Come è fatto un **campione rappresentativo** rispetto allo stream. (Oggi)
- Riusciamo a **filtrare velocemente** lo stream.
- Tenere traccia di ciò che è già arrivato in maniera efficiente con poco spazio, a patto di perdere un fattore di precisione.
- Stimare il **surprising number**: preserviamo un indicatore campionario che stimi come possono arrivare gli elementi.

--------
# Sampling from data stream

## Sampling fixed proportion
Vogliamo un campio il più rappresentativo possibile di tutti gli elementi già visti, mantenendo al più una **frazione** costante.
Ovvero al crescere dello stream cresce il campione come una frazione (e.g. 10%).

> Se ho uno stream infinito, posso invece preservare un sample di dimensione costante, il quale preserva determinate proprietà.
>
> Proprietà:
>- voglio avere una stima della frequenza campionaria degli elementi visti.
>- voglio aggiornare in maniera efficiente tali stime.


**Input**:
- $U$ sequenza di tuple $(\text{userID}, \text{query}, \text{time})$.

**Task**:
trovare un sample $S \subset U$ tale che per l'**utente tipico** $u$, data una query $q$, mi dica (approssimativamente) in media il frazione di volte in cui è stata chiesta $q$.

## Approccio semplice
1. $S = \emptyset$
2. per ogni tupla $t \in U$
	1. prendo una funzione hash u.a.r. $z \in \left[ 10 \right]$

L'approccio "campiono 10% query" non funziona

## Approccio User-Sample
Campiono gli utenti.

1. Campiono 1/10 degli utenti.
2. Faccio statistiche su quegli utenti.

```ad-warning
Caso peggiore:
1. utente 1: 90% delle query
2. utente 2: 10% delle query

Se campiono l'utente 1 rischio di preservare **troppe** informazioni in ram.
```

- $x(v,q)$ = numero di $q$-occorrenze di $id = v$ nell'input.
- $X_S(V,q)$ = numero di $q$-occorrenze di tutti gli id in $V$ dato il sample $S \subset U$

In **media**, campionando il 10% degli user avremo il 10% delle query.

**Problema**: varianza troppo alta!

## Dynamic reduction of Sample Size
Al crescere di $U$ il 10% potrebbe essere troppo grande.

[VEDI SLIDES]

-------
# Problema 2: FIXED size sample
Sia $s$ la **dimensione** del sample che voglia mantenere.
Non abbiamo alcuna ipotesi su $|U|$: <u>potenzialmente infinitp</u>

Qualunque item osservato fino al tempo $t=n$, vogliamo che ogni item già visto finisca nel sample $S$ con probabilità $s/n$.
> vogliamo un **campionamento uniforme** (e **indipendente**) dello stream

^3f454e

**RESERVOIR ALG**
Al tempo $n$ con probabilità $s/n$ lo inserisco.
1. se il nuovo item è già in $S$, allora non faccio niente.
2. se invece non è presente, ne rimuovo uno u.a.r. (con $p=1/s$).

Questo algoritmo preserva questa [[#^3f454e|proprietà]]. (si dimostra per induzione su $n$)

----
# Part 2:  Query over (long) sliding window

Abbiamo uno stream $U$ di dati e vogliamo fare delle query riguardo gli ultimi $N$ elementi arrivati.

Nelle query posso specificare una sottofinestra lunga $k \leq N$ sul quale fare una query.

$$f1(U,N,k) = \# \text{di bits pari a 1 negli ultimi }k \leq N \text{ items}$$

```ad-important
La struttura dati che voglio costruire dipende da $N$, e deve funzionare bene per ogni valore di $k$.
```


> **THM (worst case lower bound)** Per vare una risposta esatta per $f1(U,N,k)$ mi servono almento $\geq N$ bits, per ogni $k \leq N$.

Quindi dobbiamo introdurre un fattore di **approssimazione**, perdendo così un po' di informazione.


**Trivial**
Faccio una **media campionaria** del numero di 1 osservati, usando quindi $\Theta(\log{N})$ spazio.

- $S$ = numeri di 1
- $Z$ = numeri di 0
- $N = S + Z$

$$f1(U,N,k) \approx k \cdot \frac{S}{S+Z}$$

**DGIM ALG** occuma $O(\log^2(N))$ con un errore del 50% ($1.5$-approssimante).
