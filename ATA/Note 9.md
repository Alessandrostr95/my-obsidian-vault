---
date: 2023-04-17
draft: true
content:
    - algorithm for big data
    - 
---

# Bloom Filters
Un **Bloom Filter** è una struttura dati **probabilistica** che tiene un insieme $S$ di elementi, soggetta alle seguenti operazioni:
- **insert(x)**: inserisce $x$ in $S$.
- **membership(x)**: restituisce SI se $x \in S$, NO altrimenti.

La struttura è probabilistica perché:
- se $x \in S$, allora restitusce SI con probabilità 1 (ovvero **niente falsi negativi**).
- se $x \not\in S$, allora restitusice NO con probabilità $1 - \delta$.

Dove $0 < \delta < 1$ è un **parametro di errore**.

**Bloom filter**:
- $n$ è la **capacità** (decisa a priori) del filtro.
- utilizza $\Theta(n \log(1/\delta))$ **bits** di spazio per salvare al più $n$ elementi dell'universo $U$.
- gli elementi espliciti non vengono memorizzati al suo interno.

USE CASE:
- un blue filter viene usato come **interfaccia** tra le richieste degli utenti, e la struttura dati reale alla quale si vuole accedere.
- quando un utente vuole usare la struttura dati, la sua richiesta deve prima passare il bloom filter.

Siano $h_1,...,h_k$ delle funzioni hash t.c. $h_i: U \to \lbrace 0,1,...,m-1 \rbrace$.
Dove $m$ e $k$ sono dei **parametri** del filtro in funzione di $n$ e $\delta$.

> **Assunzione**: $h_1,...,h_k$ sono **independenti** e **completamente uniformi**.

```ad-info
title: Completamente Uniformi.
Una funzione $h$ è **completamente uniforme** se mappa ogni elemento $x \in U$ in maniera totalmente **uniforme**, con prob $1/m$.
```

> **Bloom Filter**
> - **Struttura**: un bloom filter è un vettore di bit $B$ grande $m$, inizialmente con tutte le celle poste a 0.
> - **insert(x)**: calcolo $h_1(x),...,h_k(x)$ e pongo $B \left[ h_i(x) \right] = 1$.
> - **membership(x)**: ritorna SI se per ogni $i = 1,...,k$ abbiamo che $B \left[ h_i(x) \right] = 1$.


Come possiamo **minimizzare i falsi positivi**?

## Analisi

Supponiamo di stare all'inizio, con tutti bit 0.
Qual è la probabilità che un data una cella, allora dopo il primo inserimento la cella rimane ancora 0? $$\left(1-\frac{1}{m}\right)^k$$

Dopo $n$ inserimenti qual è la probabilità che la data cella sia ancora 0?
$$\left(1-\frac{1}{m}\right)^{nk} = \left[\left(1-\frac{1}{m}\right)^{m}\right]^{nk/m} \approx e^{-nk/m} = p$$

Qual è la probabilità che invece dopo $n$ inserimenti ho una data cella posta ad 1?
$$1-p$$

Quindi la probabilità di avere un **falso positivo** sarà $$(1-p)^k = \left( 1 - e^{-nk/m} \right)^{k}$$
Tale probabilità è minimizzata quando $k = (n/m) \ln{2}$.
Avremo quindi la probabilità minima sarà $$\left(\frac{1}{2} \right)^{(m/n)\ln{2}} = \delta$$

I parametri in funzione di $\delta$ saranno quindi:
- $$m = n\log_2{e}\log_2{(1/\delta)} \approx 1.44 \log_2{(1/\delta)}$$
- $$k = (m/n)\ln{2} = \log_2{(1/\delta)}$$

> **THM**
> Sia $0 < \delta < 1$ il parametro usato per creare un bloom filter. Usando i parametri $k$ ed $m$ come definito prima, allora il filtro restituirà un **falto positivo** con probabilità $\leq \delta$.

==vedi esempi==

-----
# Counting 1s in a Window
## Problema
Abbiamo uno **streaming** di dati.
Senza perdità di generalità supponiamo di avere uno stream di bits.

Lo stream di dati è talmente grande, che non posso mantenerlo in memoria per intero.
Perciò si vuole avere in memoria una **rappresentazione compatta**, tale che riposnda a determinate query (anche in maniera approssimata), del tipo
- quanti 1 hai visto negli utlimi $n$ istanti di tempo?

Quindi $n$ indica la **grandezza della finestra temporale** che ci interessa.

Formalmente:
- arriva una sequenza di $N$ bit
- **query(n)**: voglio sapere quanti 1 ci sono negli ultimi $n \leq N$ bit ricevuti.
- **update(b)**: processa il prossimo bit $b \in \lbrace 0,1 \rbrace$ e aggiorno la mia struttura dati.

```ad-warning
Per ottenere le risposte **esatte**, i servono necessariamente $\Omega(N)$ bits.
```


Struttura dati **DGIM**:
- **qualità**: $1 + \varepsilon$ approssimante. (con $\varepsilon > 0$)
- **size**: $O(\varepsilon^{-1}\log^2{N})$ bits.
- **update time**: $O(\log{N})$
- **query time**: $O(\varepsilon^{-1}\log{n})$

## DGIM struttura dati
Sia $B = \lceil 1/\varepsilon \rceil$.

Creare dei **gruppi** di sequenze di bit $G_1,..., G_t$, tali che:

1. ogni $G_i$ inizia e termina con un 1.
2. tra ogni gruppo $G_i, G_{i+1}$ ci sono sempre sequenze di 0.
3. Ogni gruppo $G_i$ ha $2^{k}$, per qualche $k \geq 0$.
4. per ogni $1 \leq i < t$ allora se $G_i$ ha $2^k$ bit, allora $G_{i+1}$ ha o $2^{k}$ oppure $2^{k-1}$.
5. per ogni $k$ (**eccetto il massimo**), il numero $Z_k$ che consta il numero di gruppi che contengono $2^k$ bit, allora $B \leq Z_k \leq B+1$. Per quanto riguarda l'ultimo $k$, vale solo che $Z_k \leq B+1$.

Ogni gruppo $G_j$ è rappresentato da una coppia di interi $l,r$, che indica dove inizia e dove finisce il gruppo.

Tutti i gruppi che hanno dimensione $2^i$ sono mantenuti da una lista doppiamente concatenata $\lambda_i$ ==vedi esempio==.

Ogni elemento $\lambda_i$ preserva la **testa**, la **coda** e la **lunghezza** $Z_i$.

A loro volta, tutti i $\lambda_i$ sono mantenuto attraverso un'altra lista doppiamente concatenata $L$. ==vedi esempio==

Dimensione:
- ogni $G_j$ è rappresentato da due interi, di dimensione $O(\log{N})$ bit.
- $\vert L \vert = O(\log{N})$.
- $\lambda_i \leq B+1 \in O(\varepsilon^{-1})$.

La dimensione totale sarà quindi $O(\varepsilon^{-1}\log^2{N})$ bit.

### Update
Se $b = 0$, non fare niente.
Se $b=1$ allora:
1. crea un nuovo gruppo contenente un nuovo gruppo da un solo elemento, e lo metto in $\lambda_0$.
2. Se $\lambda_0$ diventa grande $B+2$, allora faccio il **merge** di due elementi più a sinistra di $\lambda_0$, creando un nuovo gruppo di dimensione $2^1$ e lo inserisco in $\lambda_2$.
3. Continuo su $\lambda_2, \lambda_3,...$ finché possibile.

Il costo sarà:
- **creazione/merge/spostamento** ha costo costante $O(1)$.
- itero lo spostamento è il merge al più $O(\vert L \vert)$ volte

Il costo complessivo darà $O(\log{N})$.

### Query
Data una query **query(n)**:
1. trovo tutti i gruppi che **intersecano** gli ultimi $n$ bit dello stream.
2. ritorna la somma delle dimensioni dei gruppi presi.

Per trovare i gruppi che intersecano $n$ si può fare scorrendo tutti i gruppi da destra verso sinistra, finché non trovo l'ultimo.
In totale avremo $O(\varepsilon^{-1}\log{n})$.


Per $k$ un intero tale che il gurppo più a destra che interseca ha dimensione $2^{k}$.
Siano gli eventi
- $Y$ = risposta esatta
- $X$ = risposta ritornata

> **obs**
> se $k = 0$, allora $X=Y$. Perciò assumiamo che $k > 0$.

Ovviamente è sempre vero che $$X \leq Y + 2^{k} - 1$$
Per un lowerbound ad $Y$ sarà $$Y \geq B \cdot 2^{k-1} + B \cdot 2^{k-2} + ... + B \cdot 2^{0} = B(2^k-1)$$

Perciò $$\frac{X}{Y} \leq \frac{Y + 2^{k} - 1}{Y} \leq 1 + \frac{1}{B} \leq 1 + \varepsilon$$

