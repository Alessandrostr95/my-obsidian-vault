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
- **Input**:
	- $G(V,E, c \geq 0)$
	- una collezione di **sottoinsiemi disgiunti** $S_1,...,S_k \subseteq V$
- **Feasible Solution**
	- una foresta $F$ t.c. per ogni $S_i$ abbiamo uno seiner tree $T_i$
- **Misura** $$\text{minimze cost}(F) = \sum_{e \in F} c(e)$$


==vedi esempio==


```ad-note
Possiamo riportare tale problema ad una **istanza metrica**:
- $G$ diventa completo.
- gli archi rispettano la disuguaglianza triangolare.
```


Introduciamo una *funzione di connettività* **connectivity requirement funcito** $r$ tale che
$$r(u,v) = \begin{cases}
1 &\exists S_i : u,v \in S_i\\
0 &\text{altrimenti}\\
\end{cases}$$


Definiamo una funzione $f$ per ogni **taglio** di $G$.
Sia il taglio $(A, A'= V \setminus A)$.
Allora
$$f(A) = \begin{cases}
1 &\exists u\in A, v \in A' : r(u,v) = 1\\
0 &\text{altrimenti}\\
\end{cases}$$

$$\text{minimize} \sum_{e \in E} c_e x_e$$
$$\text{s.t.} \sum_{e: e \in \delta{(S)}} x_e \geq f(S) \;\; \forall S \in \mathcal{P}(S)$$
$$x_e \in \lbrace 0,1 \rbrace \;\; \forall e \in E$$


Dove $\delta{(S)} = (S, V \setminus S)$.

```ad-note
Osservare che il numero di vincoli è **esponenziale**.
```


Ora **rilassiamo il vincolo di interezza** $$x_e \geq 0 \;\; \forall e \in E$$

Facciamo ora il **duale**.

$$\text{maximize} \sum_{S \in \mathcal{P}(S)} f(S) y_e$$
$$\text{s.t.} \sum_{e: e \in \delta{(S)}} y_S \leq c(e) \;\; \forall e \in E$$
$$y_e \geq 0 \;\; \forall S \in \mathcal{P}(S)$$



1. Prendiamo un arco $e$ ed una **qualsiasi** variabile $y_S$. Diciamo che $e$ **sente** $y_S$ se $y_S > 0$ ed $e \in \delta{(S)}$.
2. Diremo che un insieme $S$ è **incrementato** in una soluzione **duale** se $y_S > 0$. Osserviamo che
	1. incrementare $S$ oppure $S'$ ha lo **stesso effetto** sia nei vincoli che nel costo.
	2. non ho alcun vantaggio ad incrementare $S$ se $f(S) = 0$. Perciò per ogni $f(S) = 0$ assumiamo $y_S = 0$.
3. Un arco $e$ è detto **tight** se la somma degli $y_S$ che lui **sente** è pari a $c(e)$.

Osserviamo che il duale cerca di massimizzare la somma degli $y_S$ soggetto al vincolo che nessun arco è **over-tight**.


Ad ogni iterazione, algoritmo tiene una foresta $F$ (magari non ancora ammissibile come soluzione).
Diremo che un insieme $S$ è **non-soddisfatto** se
- $f(S) = 1$
- non ci sono archi nella foresta che attraversano il tagli $\delta{(S)}$.

Diremo che un insieme $S$ è **attivo** se
1. è **non-soddisfatto**
2. è **minimale rispetto all'inclusione**. Non ci sono sottoinsiemi proprio di $S$ che sono non-soddisfatti.

**OSS.** $F$ è **non ammissibile** IFF esiste un $S$ attivo.


> **Lemma**
> Un $S$ è attivo se e solo se $S$ è una **componente connessa** nella foreste corrente ed $f(S)=1$.
> 
> **Proof**
> ==da vedere dopo==


> **ALG2**
> 1. $F = \emptyset$, $y_S = 0$.
> 2. finché ci sono ancora:
> 	1. prendi gli insiemi $S$ **attivi**
> 	2. aumenta **simultaneamente** $y_e$ finché non staturo un arco $e$ (finché non diventa tight)
> 	3. aggiungi $e$ ad $F$
> 3. ritorna $F' = \lbrace e \in F \vert F \setminus \lbrace e \rbrace \text{ e' ammissibile per il primale} \rbrace$.

> **THM**
> L'algoritmo ALG2 è 2-approssimante.
> 
> **Proof**
> La soluzione duale è ammissibile per costruzione, perché non ci sono archi **over-tight**.
> 
> Affermiamo che $$\sum_{e \in F'} c(e) \leq 2 \sum_{S \in \mathcal{P}(S)} y_s$$
> $$\sum_{e \in F'}c(e) = \sum_{e \in F'} \sum_{S: e \in \delta{(S)}}y_e = \sum_{S \in \mathcal{P}(S)} \sum_{e \in F' \cap \delta{(S)}} y_e = \sum_{S \in \mathcal{P}(S)} \text{deg}_{F'}(S) y_s$$
> dove $\text{deg}_{F'}(S)$ = numero di archi che appartengono al taglio $\delta{(S)}$ e sono stati inseriti in $F$.
> 
> Ora bisogna dimostrare che $$\sum_{S}\text{deg}_{F'}(S) y_s \leq 2 \sum_{S} y_e$$
> Dimostriamo un claim più forte:
> 
> > Ad ogni iterazione l'incremento del lato sinistro della sommatoria è sempre minore uguale di quello a destra.
> 
> Consideriamo una generica iterazione, e sia $\Delta$ l'incremento fatto sulle componenti attive.
> Avremo quindi che
> $$\sum_{S \text{ attivo}}\text{deg}_{F'}(S) \Delta \leq 2 \sum_{S \text{ attivo}} \Delta$$
> $$\implies \Delta \sum_{S \text{ attivo}}\text{deg}_{F'}(S) \leq 2 \Delta \text{(\# attivi)}$$
>  $$\implies \sum_{S \text{ attivo}}\text{deg}_{F'}(S) \leq 2 \text{(\# attivi)}$$
> ==da finire==





