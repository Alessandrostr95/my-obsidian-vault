# Clustering in Approximation-Stable Instances
Esistono *problemi d'ottimizzazione* in cui la **funzione obiettivo** non deve necessariamente essere presa alla lettera per raggiungere un obiettivo.
Prendiamo per esempio il problema del **clustering**.
Abbiamo un insieme di **punti** (o **dati**) e l'obiettivo è quello di raggrupparli in "**gruppi coesi**", in modo tale che due punti in uno stesso gruppo sono "**simili**", mentre due punti appartenenti a gruppi differenti sono "**dissimili**".

Non esiste un modo ovvio e unico di tradurre questo concetto di **coesione** o **similarità** in una funzione obiettivo **numerica**.
Si sono studiate svariate funzioni obiettivo numeriche ($k$-median, $k$-mean, $k$-center, ecc...) con lo scopo di trasformare il conceto *intuitivo* di **gruppo coeso** in un problema **concreto** e **formale** di ottimizzazione.

A noi non interessa la funzione obiettivo di per se, piuttosto ci interessa trovare dei **pattern significativi** nei nostri dati.

> È preferibile computare dei "**cluster significativi**" anche se la funzione obiettivo non è ottimale, anziché massimizzare la funzione obiettivo ma senza trovare nessun parten tra i dati (il che ci suggerisce che o ci siamo chiesti la cosa sbagliata, oppure non esistono tali pattern nei dati)

Il punto focale della questione è:

> **se staimo cercando di classificare i nostri dati, allora noi stiamo implicitamente assumendo che esistano delle strutture interessanti in tali dati**

Questa prospettiva suggerisce che un modello esplicito di dati potrebbe aiutarci ad affinare le informazioni che riusciamo a ricavare col tradizionale approccio *worst-case*, come per esempio abbiamo visto nella modellazione della [[4 - Parameterized Analysis of Online Paging#Quantifying Locality The Working Set Model|località di riferimento nel paging online]].

Perciò noi ci approcceremo al problema del clustering con la seguente congettura

> **Congettura.** *Clustering is hard only when it doesn’t matter*.

In poche parole saremo ottimisti, proseguendo con l'idea che le "**soluzioni significative**" sono più facili da computare rispetto a quelle nel caso peggiore.

-----------------
# Approximation Stability and the k-Median Problem
Innanzitutto partiamo con la (<u>forte</u>) assunzione che nei nostri dati è implictamente presente un "*verità di fondo*", ovvero esiste un unico **clustering target**.
Per esempio se vogliamo distinguere le foto di cani da foto di gatti (i nostri punti), non è possibile che esista una foto che raffiguri un animale che sia sia gatto che cane.

```ad-attention
Non è detto che il **clustering target** corrisponda **esattamente** al clustering che **ottimizza** la funzione obiettivo.

Assumiamo però che abbiamo modellizato abbastanza bene la funzione obiettivo, e che quindi il clustering target è **quasi** ottimo.
```

Il nostro obiettivo è recuperare, quantomeno approssimativamente, questo *clustering target* tramite un algoritmo.

La seconda assunzione importante è che qualsiasi clustering che è **quasi ottimo** rispetto alla funzione obiettivo, non è molto diversa dal clustering target.
In poche parole 
> soluzione **numericamente quasi ottima** è anche **strutturalmente quasi ottima**

![|600](BWA_06_1.png)

## k-Median Problem
L'input è un insieme di $n$-punti su uno **spazio metrico** $(X, d)$, dove $d$ è una **funzione di distanza**
$$d: X \times X \to \mathbb{R}^+$$
soddisfacendo
- $d(x, x) = 0$ per ogni $x \in X$
- $d(x, y) = d(y, x)$ per ogni $x,y \in X$ (**simmetria**)
- $d(x, y) \leq d(x, z) + d(z, y)$ per ogni $x,y,z \in X$ (**disuguaglianza triangolare**)

La funzione di distanza $d$ può essere può essere interpretata come una sorta di **similitudine**, più la distanza è bassa più due punti sono simili.

Un $k$-**clustering** di uno spazio metrico finito $(X,d)$ è una **partizione** di $X$ in $k$ insiemi **non vuoti**.
L'obiettivo del $k$-median problem è di identificare un sottoinsieme $S \subset X$ composto da $k$ **centri** (uno per ogni cluster) che minimizza la somma delle distanze verso i punti nei rispettivi clustering.

Più formalmente si vogliono trovare $k$ punti $S = (c_1, ..., c_k)$ tali che
$$\sum_{x \in X} \left( \min_{c \in S} d(c) \right)$$ ^cc98b0

## Approximation Stability

> **Def. (Approximation Stability)**
> Un clustering target di un $k$-median è detto $(c, \varepsilon)$-**approximation stable** se ogni $k$-clustering $c$-approssimante è $\varepsilon$-**accurato**, il che significa che è concorde con il clustering target per **almeno** una frazione $1 - \varepsilon$ dei punti.

^d2b2c2

```ad-caution
Eseguire un algoritmo $c$-**approssimante** per un problema di **minimizzazione** significa trovare una soluzione che è **al più** $c$ volte peggiore di quella ottima.

Ovvero sia $S$ una soluzione restituita da un algoritmo $c$-approssimante, e sia $S^*$ una **soluzione ottima**, allora avremo che $$\frac{\text{val}(S)}{\text{val}(S^*)} \leq c$$

Importante infine notare che visto che $S^*$ è la soluzione che **minimizza** una funzione obiettivo, allora necessariamente avremo che $c \geq 1$.
```


Quando diciamo che due $k$-clustering sono "*concordi" con una frazione $1 - \varepsilon$ dei punti, si intende che esiste un modo di **etichettare** i due clustering con etichette $1, 2, ..., k$ in modo tale che **almeno** $(1-\varepsilon)n$ punti hanno le stesse etichette in entrambi i $k$-clustering.

```ad-note
La [[#^d2b2c2|definizione]] diventa più *restrittiva* al **crescere** di $c$ oppure al **decrescere** di $\varepsilon$.
```

### Discussion of Approximation Stability
La [[#^d2b2c2|approximation stability]] richiede che l'unico modo per ottenere una soluzione quasi ottima numericamente, bisogna trovare una soluzione quesi uguale al clustering target.

> La **similarità numerica** (rispetto alla funzione obiettivo) implica la **similarità strutturale**.

Quello che ci si può chiedere e se tale definizione è **ragionevole**.
Infatti presa toppo alla regola la [[#^d2b2c2|definizione di approximation stability]] è troppo forte, e non è chiaro se essa può occorrere su dati reali.
Poiché la definizione fa riferimento sia a un **clustering target sconosciuto** che a tutte le soluzioni **quasi ottimali**, non è nemmeno chiaro come verificare se la condizione vale o meno negli input che ci interessano.

Innanzitutto, c'è una motivazione plausibile per cui le istanze di clustering "*reali*" dovrebbero tendere (quantomeno vagamente) a soddisfare la [[#^d2b2c2|definizione di approximation stability]].
Infatti, per la classificazione di immagini di cani e gatti, ad esempio, sembra molto improbabile che una classificazione del tutto errata delle immagini produca un 2-clustering <u>numericamente quasi ottimale</u> (per qualsiasi ragionevole codifica del problema).

In secondo luogo invece, l'insieme di istanze in cui è possibile dimostrare <u>formalmente</u> le buone prestazioni di un algoritmo, è in genere un **piccolo sottoinsieme** delle istanze in cui l'algoritmo funziona bene empiricamente.

## Connection to Approximation Algorithms
Come prima cosa possiamo usare degli algoritmo $c$-approssimanti già noti per il problema $k$-median.

Purtroppo però è noto che, a meno che $P = NP$, non esistono algoritmi **polinomiali** $c$-approssimanti per il $k$-median con $c < 1 + \dfrac{2}{e} \approx 1.736$ .

Inoltre il miglior upperbound ad oggi noto è di $1 + \sqrt{3} \approx 2.73$

E se invece del fattore di parrossimazione $c$ ci interessassimo solamente all'accuratezza $\varepsilon$?
Ovvero, anziché cercare di approssimare bene numericamente l'ottimo, ci interessassimo invece di trovare un clustering che sia il quanto più simile (strutturalmente) a quello target?
Oppure potremmo chiederci se classificare "*bene*" la maggior parte dei punti implica automaticamente una buona approssimazione numerica della funzione obiettivo.

La risposta è negativa.
Infatti si può dimostrare che, nel caso di instanze **approximation-stable** di $k$-median, trovare clustering "*accurati*" è **strettamente più semplice** che trovare *approssimazioni numeriche*.

> **Theorem 1**
> Per ogni coppia di **costanti** $\alpha, \varepsilon > 0$, esiste un **algoritmo polinomiale** che, per ogni istanza $(1 + \alpha, \varepsilon)$-[[#^d2b2c2|approximation stable]] del $k$-median, computa un $k$-clustering che è uguale a quello target, eccetto che per una frazione $O(\varepsilon/\alpha)$ dei punti.

^3c1f47

```ad-warning
Quando $\alpha < 2/e$ il [[#^3c1f47|precedente teorema]] garantisce classificazioni accurate anche quando l'approssimazione numerica è NP-hard!

Ciò è possibile grazie alla [[#^d2b2c2|approximation stability]] che garantisce la presenza di una **struttura** nei dati utile alla classificazione. 
```

-----------
# Structural Consequences of Approximation Stability
Sia $C^*_1, ..., C^*_k$ un clustring ottimo che minimizza la [[#^cc98b0|funzione obiettivo]] del $k$-median problem.
Supponiamo di conoscere il valore $OPT$ della soluzione ottima $C^*_1, ..., C^*_k$.
Sia inoltre $S^*$ l'insieme dei rispettivi centri di $C^*_1, ..., C^*_k$.

> **Definition 1**
> Per ogni punto $x \in X$, indichiamo con $w(x)$ il **contributo** di $x$ nella soluzione ottimva $C^*_1, ..., C^*_k$.
> Ovvero $$w(x) = \min_{c \in S^*} d(x,c)$$
> 
> Diremo che:
> 1. Il punto $x$ è **vicino** se $$w(x) \leq \frac{OPT}{n} \cdot \frac{\alpha}{5\varepsilon}$$
> 2. Il punto $x$ è **ben separato** se $$w_2(x) - w(x) > \frac{OPT}{n} \cdot \frac{\alpha}{\varepsilon}$$ dove $w_2(x)$ indica la distanza dal **secondo** centro più vicino al punto $x$.
> 3. Il punto $x$ è detto **buono** se è **vicino** e **ben separato**, altrimenti è detto **cattivo**.

^3bdd74

L'idea dietro la [[#^3bdd74|definizione]] è che un'istanza $(1+\alpha, \varepsilon)$-approximation stable è "*ben strutturata*" quando tutti (o quasi) i punti sono **vicini** al proprio centro, e **ben separati** dagli altri centri.
![|600](BWA_06_2.png)

Dimostreremo ora che il clustering ottimale è ben strutturato, comprendendo cluster strettamente collegati e ben separati.

Innanzitutto, una **frazione costante** di tutti i punti è **vicina** (indipendentemente dal fatto che l'istanza sia *approximation-stable* o meno).

> **Lemma 1**
> Per **ogni** istanza del $k$-median problem, **al più** $\dfrac{5\varepsilon}{\alpha}n$ punti sono **[[#^3bdd74|non vicini]]**.

^848593

> **Proof**
> Supponiamo *per assurdo* che ci sono **più** di $\dfrac{5\varepsilon}{\alpha}n$ punti **non vicini**.
> Ricordiamo che 
> $$OPT = \sum_{x \in X} w(x) = \sum_{x \in X} \min_{c \in S^*}d(x,c)$$
> Sia $E \subseteq X$ l'inseme dei punti **non vicini**, con $\vert E \vert > \dfrac{5\varepsilon}{\alpha}n$.
> Dato che $\vert E \vert \leq \vert X \vert$, allora avremo che $$\sum_{x \in E}w(x) \leq \sum_{x \in X}w(x) = OPT$$
> 
> Però, visto che $E$ è composto da punti **non buoni**, avremo che $$\sum_{x \in E}w(x) > \vert E \vert \cdot \frac{OPT}{n}\cdot\frac{\alpha}{5\varepsilon} > \cancel{\dfrac{5\varepsilon}{\alpha}}\cancel{n} \cdot \frac{OPT}{\cancel{n}}\cdot\cancel{\frac{\alpha}{5\varepsilon}} > OPT$$
> assurdo $\square$.

Sfruttando la [[#^d2b2c2|approximation stability]] possiamo anche limitare il numero di punti che non sono **ben separati**.

> **Lemma 2**
> In una istanza $(1+\alpha,\varepsilon)$-approximation stabe, **al più** di $\varepsilon n$ punti **non** sono **ben separati**.

^46dfb6

> **Proof**
> Supponiamo per assurdo che ci siano **strettamente più** di $> \varepsilon n$ punti che non sono ben separati.
> Osserviamo che $$w_2(x) - w(x) \leq \frac{OPT}{n} \cdot \frac{\alpha}{\varepsilon}$$
> $$\implies w_2(x) \leq w(x) + \frac{OPT}{n} \cdot \frac{\alpha}{\varepsilon}$$
> Partendo dalla soluzione ottima $C^*_1, ..., C^*_k$, riassegnamo $\varepsilon n$ ai cluster con i **secondi** più vicini centri (rispettivamente).
> Indichiamo con $\hat{X}$ l'inseme dei $\varepsilon n$ punti riassegnati.
> 
> Così facendo abbiamo ottenuto un nuovo clustering, con una frazione $\varepsilon$ di punti **mal etichettati**, e con **valore**
> $$\begin{align}
\sum_{x \in X} w_2(x)
&= \sum_{x \in X \setminus \hat{X}} w(x) + \sum_{x \in \hat{X}} w_2(x)\\
&\leq \sum_{x \in X \setminus \hat{X}} w(x) + \sum_{x \in \hat{X}} \left(w(x) + \frac{OPT}{n} \cdot \frac{\alpha}{\varepsilon} \right)\\
&= \sum_{x \in X}w(x) +\sum_{x \in \hat{x}}\frac{OPT}{n} \cdot \frac{\alpha}{\varepsilon}\\
&= OPT + \varepsilon n \frac{OPT}{n} \cdot \frac{\alpha}{\varepsilon} = (1 + \alpha) \cdot OPT
\end{align}$$
> Questo però contraddice l'ipotesi che l'istanza è $(1+\alpha, \varepsilon)$-[[#^d2b2c2|approximation stable]], perché abbiamo trovato una $1 + \alpha$ approssimazione della soluzione ottima con **strettamente di più** di $\varepsilon\%$ delle etichette in comune (mentre noi vogliamo al più $\varepsilon\%$) $\square$.
> 

I punti che **non sono** buoni, sono tutti quelli che non sono **vicini** o non sono **ben distanti** <u>oppure entrambi</u>.
Perciò sia $A$ l'insieme dei punti **non vicini**, con dimensione $\vert A \vert \leq \dfrac{5\varepsilon}{\alpha}n$ (per [[#^848593|Lemma 1]]), e sia $B$ l'insieme dei punti **non ben distanti**, con dimensione $\vert B \vert \leq \varepsilon n$ (per [[#^46dfb6|Lemma 2]]), avremo che il numero di punti **non buoni** sarà
$$\vert A \cup B\vert \leq \vert A \vert + \vert B \vert = \varepsilon n \left( 1 + \frac{5}{\alpha}\right) := b$$



-----------
# Approximate Recovery in Approximation-Stable k-Median Instances


