---
date: 2023-09-15
content:
  - algorithm for big data
draft: true
---
Un **MinHash Sketch** è "riassunto randomizzato" di un sottoinsieme $N \subseteq U$.
Esistono tre varianti:
1. $k$-min
2. bottom-$k$
3. $k$-partition

Uno sketch è definito rispetto ad una o più **permutazioni random** del dominio $U$.
Definiamo una permutazione random di $U$ assegnando un [[Random Sample|rank casuale]] $r(j) \sim U\left[0,1\right]$  ad ogni elemento $j \in U$.
Una permutazione quindi e una **sequenza ordinata** di rank $r(1) < r(2) < ...$. ^c7e4f7

> [!note]
> Essendo $U\left[0,1\right]$ una [[Distribuzioni#Uniforme|uniforme continua]] avremo che $$P(r(i) = r(j)) = 0$$ per ogni $i \neq j$.
> 

## $k$-min sketch
Un $k$-min sketch consiste nel considerare $k$ **permutazioni indipendenti** e selezionando i rank **minimi** da ciascune di queste.
Essa equivale quindi a $k$ campionamenti **con rimpiazzo**. ^ba3b96

## bottom-$k$ sketch
Un bottom-$k$ sketch consiste nel prelevare i $k$ rank più piccoli di una data permutazione.
Essa equivale quindi a $k$ campionamenti **senza rimpiazzo**.

## $k$-partition sketch
In un $k$-partition sketch si **mappano in maniera uniforme** gli elementi in $k$ buckets.
Dopodiché si preleva l'elemento con rank minimo di ciascuno due $k$ bucket.

> [!note]
> Osservare che per $k=1$ le tre varianti sono equivalenti.

## Coordinated MinHash Sketches
Differenti MinHash Sketches di $N \subseteq U$ sono detti **coordinati** se si riferiscono alle stesse permutazioni di $U$.