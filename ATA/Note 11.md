---
date: 2023-04-26
draft: true
content:
    - algorithm for big data
    - locality-sensitive hashing
---

# Locality-Sensitive Hashing
Dati $N$ items, si vogliono trovare coppie di items tali che la loro **similarità** è al di sopra di una **threshold**.

Il problema è che su quantità di $N$ troppo grandi, andare a vedere tutte le coppie ($\Theta(N^2)$) non è **trattabile**.
Un altro problema è che gli oggetti sono "*grandi*", ovvero hanno un eleveto numero di infomrazioni. Perciò non è sempre possibile salvare gli oggetti tutti in memoria.

Applicazioni:
- **plagiarismo**: voglio vedere se dati due docuementi, capire se uno ha copiato un'altro.
- **mirror pages**: capire se ci sono pagine web duplicate.
- **documenti riguardo lo stesso topic**: il concetto di similarità in questo caso è semantica (e non sintattica).
- **matching fingerprints**: trovare velocemente duplicati in un databases.
- **finding similar customers**: trovare quali utenti sono simili in base all'insieme di oggetti che hanno comprato.

Ogni item è un **insieme** di elementi di un dato universo (e.g. i prodotti comprati da un utente amazon).

Quando due item $A$ e $B$ sono **simili**?

Usiamo come misura di similirità la [[Jaccard coefficient|Jacard Similarity]].
$$J(A,B) = \frac{\vert A \cap B \vert}{\vert A \cup B \vert}$$
**Goal**: trovare le coppie di items la cui jacard similarity è sopra una certa soglia.

```ad-note
Assumiamo che le coppie con un data similarità sopra la threshold non sono troppe.
Perché se fossero $\Theta(n^2)$ allora non si posso ritornare tutte in meno tempo di $\Theta(n^2)$.
```

Come evitare di effetture $\Theta(n^2)$ confronti?
L' LSH (locality-sensitive hashing) ha due ingredienti:
- **rappresentazione randomizzata** degli elementi, tali che però **preserva le similarità**. Questo ingrediente dipende fortemente dalla misura di similarità che consideriamo.
- l'idea è di usare delle funzioni/tabelle hash in modo tale che oggetti simili vengano mappate nello stesso slot/bucket.

## Minhashing & Signatures
Vogliamo ora avere una rappresentazione compatta degli items, in modo tale da preservare la similarità.

Prendiamo un permutazione random $\pi$ delle righe della matrice.
Data una generica colonna $S$, posso rappresentare $S$ come
$$h_\pi(S) = \text{forst row index in which appears 1, according } \pi$$

> **Lemma**
> Per ogni coppia di elementi $S,S'$, $$P(h_\pi(S) = h_\pi(S')) = J(S,S')$$
> 
> **Proof**
> Sia $i$ il primo indice in cui appare un 1 o nella colonna $S$ o nella colonna $S'$ (o entrambe).
> $$i = \min\lbrace h_\pi(S), h_\pi(S') \rbrace$$
> Certamente avremo che l'elemento $i$ appartiene all'insieme $S \cup S'$.
> Dato che ho scelto $\pi$ u.a.r., allora la probabilità che $i = x \in S \cup S'$ è **uniforme**.
> Ovvero $$P(i = x) = \frac{1}{\vert S \cup S' \vert} \; \forall x \in S \cup S'$$
> Perciò la probabilità che $h_\pi(S) = h_\pi(S)$ è pari alla probabilità che $i \in S \cap S'$.
> Perciò $$P(h_\pi(S) = h_\pi(S)) = P(i \in S \cap S') = \sum_{x \in S \cap S'}P(i = x) = \frac{\vert S \cap S' \vert}{\vert S \cup S' \vert} \;\; \square = J(S,S')$$

A questo punto, anziché scegliere un permutazione random $\pi$, calcoliamo il $n$ permutazioni random $\pi_1, ..., \pi_n$ con i rispettivi hash $h_1, ..., h_n$.

A questo punto rappresento ogni items $S$ con un vettore $n$-dimensionale $\langle h_1(S), ..., h_n(S)\rangle$, anziché un vettore binario lungo $N$.
Questa rappresentazione è anche detta **signature**.

```ad-note
Osserviamo:
- la nuova rappresentazione è molto più piccola di quella esplicita, perché $n << N$.
- due colonne $S,S'$ hanno due **signature** simili. Infatti il numero di celle simile nelle due signature è **in media** $n \cdot J(S,S')$.
```

## Banding Technique
Consideriamo ora la sola matrice delle signature.

Indichiamo con $$s = J(S,S')$$

Idea:
- raggruppa gli $n$ min-hash in $b$ **bande** di $r$ righe ciascuna (ovvero t.c. $n = b \cdot r$).
- dico che due colonne $S,S'$ sono **cadidati simili** se concidono in **almeno** una banda.
- trovare i candidati velocemente: consideriamo la sotto-signature di una banda come la **chiave** di una **tabella hash**. Perciò una banda di $S$ coincide con una di $S'$ se vengono mappate nello stesso elemento di una tabella hash. Quindi per ogni banda, mappo le sottosignature di ogni colonna in una tabella hash. Tutte le sotto-signature che collidono sono potenzialmente uguali.
- Quando collidono due sotto-signature che in realtà non sono uguali? Molto bassa quando la dimensione $m$ della tabella hash è molto bassa.

**Assunzione**: non ci sono collisioni occasionali.

La probabilità che $S$ ed $S'$ non coincidono in una data banda è $1 - s^r$.

La probabilità che $S$ ed $S'$ non coincidono in nessuna banda è $(1 - s^r)^b$.

La probabilità che $S$ ed $S'$ sono candidati è $1 - (1-s^r)^b$.

Sia la curva $f(s) = 1 - (1-s^r)^b$.
Indichiamo con $\varphi$ il valore di $s$ per cui $f(\varphi) = 1/2$.
Si può calcolare $\varphi$ come $\varphi = (1/b)^{1/r}$.