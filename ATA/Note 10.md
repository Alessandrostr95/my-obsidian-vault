---
date: 2023-04-19
draft: true
content:
    - algorithm for big data
    - 
---

# Finding Frequent Items on Stream
- **Input**: abbiamo uno **stream** molto grande di oggetti, che non possiamo manetere in memoria per intero.
- **Obiettivo**: vogliamo conservare una struttura dati che ci consenta di estrapolare gli oggetti sono tra i più frequenti.

**Data Base**
```sql
-- iceberg query
SELECT City, COUNT(*)  
FROM Irish_Pubs  
GROUP BY City
HAVING COUNT(*) >= T
```

**Data Minimg**
Abbiamo un database di **transiazioni**.
- $I$: un insieme di prodotti/item
- $D$: un set di **transazioni** dove ogni transazione è $T \subseteq I$.

Definiamo
- un insieme di **association rule** $X \implies Y$  che sta ad indicare che se compro $X$ allora compor $Y$
- la regola $X \implies Y$ accade con **confidenza** $c\%$ se una frazione di transazioni $c$ di $D$ che contengon $X$ hanno anche $Y$.
- la regola $X \implies Y$ ha **supporto** $s\%$ se una frazione di transazioni $s$ di $D$ hanno sia $X$ che $Y$.

**Goal**: trovare assotiation rule di $D$ che hanno supporto e supporto oltre una certa soglia

**Network Monitoring**
Abbiamo un **flusso** di pacchetti con stessa sorgente e destinazione.


### Definizione formale
Dato un parametro $0 < \varepsilon < \phi < 1$, ed uno stream $x_1,...,x_n$ di $n$ elementi.
Vogliamo trovare:
- tutti gli elementi che appaiono con frequenza $\phi n$ volte. Non vogliamo **falsi negativi**!
- non vogli avere gli elementi con frequenza minore di $(\phi - \varepsilon)n$.


**Sticky Sampling Algorithm**
- è un algoritmo **randomizzato** per risolvere il problema.
- ottiene entrabe i due obiettivi con probabilità $\geq 1 - \delta$.
- mantiene un **sampling** molto piccolo dei dati, di dimensione $2\varepsilon^{-1}\log{(\phi^{-1}\delta^{-1})}$
- il parametro $0 < \delta < 1$ può essere scelto.


**OSS**: lo spazio del sample è **indipendente** da $n$.

- Sia $t = \varepsilon^{-1}\log{(\phi^{-1}\delta^{-1})}$.
- Sia $S$ il sample, composto da coppi $(x,f_e(x))$, ovvero
	- $f_e(x)$ è una **stima** della reale frequenza $f(x)$ dell'elemento $x$.

Dato che lo stream è potenzialmente infinito, l'algoritmo lavora per **finestre temporali**:
- ogni finestra ha una **size** e un **samplig rate** $r$.
	- la finstre successiva sarà grande il **doppio** della precedente.
	- all'inizio $S = \emptyset$ e $r = 1$, e $size = 2t$.
	- al secondo step $r = 2$ e $size = 2t$.
	- dal terzo step in poi, $r$ e $size$ raddoppiano ogni volta.

In ogni *finestra*, ogni volta che arriva un nuovo elemento $x$, allora:
- se $x \in S$, allora incremento $f_e(x)$ di 1.
- altrimenti inserisco $(x,1)$ in $S$ con probabilit $1/r$.

Quando passo da $r=1,size=2t$ alla finestra $r=2,size=2t$, voglio modificare $S$ in modo tale che è come se avessi sempre campionato (fin dall'inizio) con $r=2, size=2t$. 
E così via...
Questo è detto **adjusting step**

**query**: ritorna tutti gli elementi di $S$ che hanno frequenza stimata almeno $(\phi - \varepsilon)n$.

### Adjusting step
Per ogni elemento $x \in S$:
- lancio un moneta.
- se esce **testa**, non faccio nulla e vado avanti.
- se esce **croce**:
	- calcolo una **geometrica** con parametro $1/(2r)$.
	- sia $k$ il valore del samplig geometrico.
	- decremento $f_e(x)$ di $k$.
	- se $f_e(x) \leq 0$ rimuovo $x$ da $S$.

**OBS** il rate $r$ è sempre una potenza di $2$.
Perciò assumiamo $r = 2^i$ per qualche $i$.


Quando trovo un elemento $x$, incremento con probabilità $1/r = 2^{-i}$.
Questo equivale a lanciare $i$ monete, ed incrementare $f_e(x)$ quando escono $i$ teste.
==vedi slides==

> **Lemma**: Sia $n$ il numero di elementi visti fin ora. Sia $r$ il sampling rate corrente. Allora $1/r \geq t/n$.
> 
> **Proof**: $$\frac{1}{r} \geq \frac{t}{n} \iff n \geq tr$$
> ==vedi immagine==


> **THM**: Per ogni $\varepsilon < \phi, \delta \in (0,1)$. Allora lo Sticky Alg. risolve il problema con probabilità almeno $1 - \delta$, usando una struttura dati grande in media $2\varepsilon^{-1}\log{(\phi^{-1}\delta^{-1})}$.
> 
> **Proof**:
> Osservare che $f_e(x) \leq f(x)$.
> Dato che l'algoritmo no ritrna mail elementi con $f_e(x) < (\phi - \varepsilon)n$. ==da finire==
