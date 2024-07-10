---
date: 2022-10-26
draft: true
---

vedi [[NLP/Lecture 5|note precedenti]].

Abbiamo un **oracolo** $O$ e un sistema $S$ che interpretano delle frasi $f$

index | $f$ | $I_S(f)$ | $I_O(f)$
---|---|---|---
1 | $f_1$ | $S_1$ | $O_1$
2 | $f_2$ | $S_2$ | $O_2$
3 | $f_3$ | $S_3$ | $O_3$
$\vdots$ | $\vdots$ | $\ddots$ | $\ddots$
$n$ | $f_n$ | $S_n$ | $O_n$

L'**accuracy** del nostro sistema è definita come la frequenza di interpretazione corrette rispetto ad un oracolo $O$
$$Acc = \frac{\sum_i \left[ S_i \equiv O_i \right]}{n}$$

Indichiamo con
$$O^+ \equiv \lbrace f : O(f) = \text{positive} \rbrace \subseteq O$$
$$S^+ \equiv \lbrace f : S(f) = \text{positive} \rbrace \subseteq S$$

Definiamo quindi
- **Precision** $$P = \frac{\vert O^+ \cap S^+ \vert}{\vert S^+\vert}$$
- **Recall** $$R = \frac{\vert O^+ \cap S^+ \vert}{\vert O^+\vert}$$

- **F-measure** $$F = \frac{1}{\dfrac{\alpha}{R} + \dfrac{(1-\alpha)}{P}}$$

Supponiamo di avere una base line **BL**, e due sistemi $S_1, S_2$, con misure di valutazione

**Model** | **Measure**
---|---
$S_1$ | $83.9\%$
$S_2$ | $85.2\%$
$BL$ | $70\%$

Purtroppo tali risultati dipendono dall'oracolo $O$ in questione, perciò potrebbe essere  solo un **caso** che $S_1$ ha valore $.839$.
Oppure, peggio ancora, l'orcaolo $O$ potrebbe essere "**sbagliato**".

Si desidera che $O$ descriva in maniera più realistica possibile la realtà rappresentata.

L'ideale sarebbe di avere differenti oracoli, per vedere come si comporta un sistema nei diversi contesti.
Per risparmiare, posso **partizionare** $O$ in $k$ parti, e computo la [[Random Sample|media e la varianza campionaria]] delle misure.

**Model** | **Measure 1** | **Measure 2** | **...** | **mean** $\pm$ **bias**
---|---|---|---|---
$S_1$ | $83.9\%$ | ... | ... | $.815 \pm .16$
$S_2$ | $85.2\%$ | ... | ... | $.826 \pm .25$
$BL$ | $70\%$ | ... | ... | $.732 \pm .32$


Supponiamo dia vere un **parametro ignoto** $\theta$.
- **Approccio frequentista**: assumo che $\theta$ esiste, e per la **[[Convergenza#Teorema Forte dei Grandi Numeri - Strong LLN|legge dei grandi numeri]]** posso ricavarlo.
- **Approccio bayesiano**: $\theta$ non c'è modo di scoprirlo, però posso ricavarlo **in distribuzione** con media e varianza.

----
# Sign-Test
Date le misure di due sistemi $S_1, S_2$, tale test non fa alcuna assunzione sulle **<u>relative distrubizioni</u>**: si osservano direttamente i risultati.

Assumo che $S_1$ ed $S_2$ abbiano la stessa distribuzione, allora avremo che $$P(S_1(i) > S_2(i)) = 0.5$$
Osserviamo che la v.a. **binaria** $Y_i = S_1(i) > S_2(i)$ è una **[[Distribuzioni#Bernoulli|bernoulliana]]** di parametro $p = 0.5$

A questo punto posso definire l'ipotesi nulla $$H_0: p = 0.5$$

Considerando tutte le $m$ mesurazioni, avremo una distribuzione **[[Distribuzioni#Binomiale|binomiale]]** di parametri $B(m, 0.5)$, e possiamo calcolare il $p$[[p-value|-value]].

-----
# Wilcoxon's ranked sign-test
Nel test precedente non si tiene conto di **quanto** un sistema vince rispetto ad un altro, ma semplicemente si teneva conto solamente del numero di volte in cui $S_1$ *"vince"* rispetto ad $S_2$.

Una buona idea sarebbe quella di **pesare** in qualche modo di quanto una vince rispetto ad un altra.

- Definiamo le v.a. $\lbrace X_i = S_1(i) -  S_2(i) \rbrace_{i = 1, ..., m}$
- Ordiniamo le v.a. $\vert X_1 \vert, ..., \vert X_m \vert$.
- Definiamo la distribuzione $$T = \sum_{k = 1}^{m} k \cdot \text{sign}{(X_k)}$$
Sotto $H_0$, ovvero che $S_1$ ed $S_2$ hanno la stessa distribuzione, allora $T$ avrà una distribuzione [??], dalla quale posso calcolarne il $p$[[p-value|-value]].