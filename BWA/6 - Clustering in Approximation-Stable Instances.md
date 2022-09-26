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
$$\sum_{x \in X} \left( \min_{c \in S} d(c) \right)$$

## Approximation Stability

> **Def. (Approximation Stability)**
> Un clustering target di un $k$-median è detto $(c, \varepsilon)$-**approximation stable** se ogni $k$-clustering $c$-approssimante è $\varepsilon$-**accurato**, il che significa che è concorde con il clustering target per **almeno** una frazione $1 - \varepsilon$ dei punti.

^d2b2c2

Quando diciamo che due $k$-clustering sono "*concordi" con una frazione $1 - \varepsilon$ dei punti, si intende che esiste un modo di **etichettare** i due clustering con etichette $1, 2, ..., k$ in modo tale che **almeno** $(1-\varepsilon)n$ punti hanno le stesse etichette in entrambi i $k$-clustering.

```ad-note
La [[#^d2b2c2|definizione]] diventa più *restrittiva* al **crescere** di $c$ oppure al **decrescere** di $\varepsilon$.
```

