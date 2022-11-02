---
date: 2022-11-02
draft: true
---

# Index Compression
Spesso può capitare che anche gli indici sono troppo grandi, perciò è vantaggioso cercare di comprimere i documenti.

Mentre la codifica di **Huffman** è **general purpose**, sono stati definiti algoritmi di compressione estremamente specifici ed ottimizzati per gli indici.

Quello che vogliamo inoltre è che la **decompressione** deve essere il più veloce possibile, possibilmente *gratuita*.
Questo perché ci sono principalmente operazioni di **query** al mio indice, perciò un numero elevato di operazioni in lettura.

## Vocabulary size vs Collection Size

### Heaps' Law
Data una collezione di documenti, si vuole stimare la dimensione del dizionario.
- $M$ la dimensione del vocabolario.
- $T$ il numero di token nella mia collezione.

> **Heaps' Law**
> $$M \approx k T^b$$
> dove $30 \leq k \leq 100$ e $b \approx .5$.

Se facciamo il grafico **log-log** otteniamo una retta di pendenza $\approx 0.5$
$$\log{M} \approx \log{k} + b \log{T}$$

```ad-success
Infatti, al crescere dei miei documenti la dimensione non tende ad *"esplodere"*.
Infatti ci si aspetta che man mano che processo documenti, incontrerò termini già incontrati in precedenza, e che quindi eseguiamo sempre meno operazioni d'inserimento.
```


### Zipfs' Law
Si vuole stimare la frequenza dei termini che appaiono nel nostro corpus.

> **Zipfs' Law**
> L'$i$-esimo termine più frequenza ha frequenza proprozionale a $i^{-1}$.

Ovvero la frequenza dei termini segue una **powerlaw**, ovvero come un'inversa di un polinomio.