---
date: 2023-03-22
draft: true
content:
  - linear programming
  - rounding
  - dual
---

# Tecnica Duale
Idea:
- modelliamo il problema come LP intera.
- lo rilassiamo.


Iniziamo con:
- una soluzione **ammissibile** per il duale
- prendiamo l'equivalente soluzione prima, che possibilmente non è ammissibile.

## Minimum Set Cover
Esempio [[ATA/Note 2#Minimum Set Cover Problem|Minimum Set Cover]].

- **Primale** min SetCover (rilassato) (problema di covering)
$$\text{minimize} \sum_{S \in \mathcal{S}} c(S) \cdot x_S$$
$$\text{subject to} \;\; \sum_{S: e \in S} x_S \geq 1 \;\; \forall e \in U$$
$$x_S \geq 0 \;\; \forall S \in \mathcal{S}$$

- **Duale** (problema di packing)
$$\text{maximize} \;\; \sum_{e \in U} y_e$$
$$\text{s.t.} \;\; \sum_{e: e \in S} y_e \leq \text{cost}(S) \;\; \forall S \in \mathcal{S}$$
$$y_e \geq 0 \;\; \forall e \in U$$


Data una soluzione duale $y$, noi diciamo che un insieme $S$ è **tight** se $\sum_{e \in S} y_e = c(S)$ (abbiamo riempito tutto $S$).

**Idea**: prendiamo i soli insiemi tight.

> **ALG1**
> 1. Inizializza $x = 0, \;\; y = 0$.
> 2. Finché non ho un cover, prendi un qualsiasi elemento $e$ **non coperto**.
> 3. Alzo il valore $y_e$ finché non rendo tight un insieme $S$ che lo contiene.
> 4. Prendi gli insiemi diventati tight grazie ad $e$, li toglo e li aggiungo nella soluzione $x$. (ovvero $x_S = 1$).
> 5. Ritorno $x$.


> **THM**
> L'**ALG1** è una $f$-approssimazione.
> 
> **Proof**:
> Sicuramente $x$ è una soluzione ammissibile.
> Affermiamo che $$\sum_{S \in \mathcal{S}}c(s) x_s \leq f \sum_{e \in U} y_e$$
> Pensiamo a $f \sum_{e \in U} y_e$ come dei **soldi** che possiamo spendere per comprare la soluzione ammissibile $x$.
> Basta dimostrare che tali soldi sono sufficienti per comprare $x$.
> 
> Per ogni elemento $e$ distribuiamo $f \cdot y_e$ soldi.
> Dopodiché l'elemento $e$ paga $y_e$ soldi per ogni insieme $S$ nella soluzione (dove $x_S = 1$ t.c. $e \in S$).
> Dato che la freqeunza di $e$ è **al più** $\leq f$, $e$ ha sempre abbastanza soldi per pagare i suoi insiemi che lo contengono.
> 
> Dato che ogni insieme $S$ è **tight**, allora $S$ sarà sempre pagato per intero dai suoi elementi.
> 
> In fine dato che $y$ è ammissibile, allora $\text{cost}(y) \leq \text{OPT}$ $\square$.



==vedi esempio tight==

```ad-note
Osservare che in questo caso si usa la LP **solo** per l'analisi, ma non per la risoluzione.
```


## Steiner Forset

