Il problema della **classificazione lineare** gli elementi del dominio $\mathcal{X}$ sono **divisi** (assumiamo **partizionati**) in **regioni**.
Assumiamo per il momento che le regioni siano **linearmente separabili**, ovvero che ogni regione è separata dalle altre da **iperpiani lineari**.

Le etichette degli elementi del dominio $\mathcal{X}$ hanno quindi valori **discreti**, a differenza della [[Linear Regression|regressione]] in cui possono avere un qualsiasi valore reale.

Il caso più semplice è la **classificazione binaria**, in cui ogni elemento $x$ può appartenere ad una delle **classi** $C_0$ o $C_1$.
Una codifica, in questo caso, può essere un singolo valore numerico $t \in \lbrace 0,1 \rbrace$, dove $t = 0$ se $x \in C_0$ e $t = 1$ se $x \in C_1$.

Nel caso più generale in cui abbiamo $K > 2$ classi, è preferibile usare la codifica **one-hot**, ovvero $t \in \lbrace 0, 1\rbrace^K$ è un vettore con $K-1$ zeri ed un solo $1$.
Se $x \in C_i$ allora calle con $1$ è quella in posizione $i$. ^85e2b9

$C_i$ | $t$
---|---
1 | 0001
2 | 0010
3 | 0100
4 | 1000

Esistono tre principali approcci alla classificazione:
1. **Discriminant Function**: nella quale si vuole identificare una **funzione** $f: \mathbf{X} \to \lbrace 1,...,K \rbrace$ tale che mappa ogni elemento $x$ nella classe $C_{f(x)}$. ^3cfa43
2. **Discriminative Approach**: il quale è diviso in due fasI: ^1baa54
	- **inference phase**: nella quale si cerca di inferire la probabilità $p(C_i \vert x)$ sulla base delle osservazioni fatte.
	- **decision phase**: nella quale si usa la precedente distribuzione per assegnare una classe all'elemento in input $x$ (e.g. assegnare la classe $C$ che massimizza $p(C \vert x)$).
3. **Generative Approach**: nella quale si determina la probabilità condizionata $p(x \vert C_i)$ e la probabilità a priori $p(C_i)$, per poi derivare tramite la regola di bayes la probabilità a posteriori $p(C_i \vert x)$. ^00923c

## Discriminative Approaches
Gli approcci [[#^3cfa43|1]] e [[#^1baa54|2]] sono **discriminativi**, affrontano il problema della classificazione derivando dal training set $\mathcal{T}$ delle **condizioni** (come i confini decisionali) che, se applicate a un punto $x$, **discriminano** in quale classe ci si trova.

I confini tra le regioni che descrivono le classi sono specificati dalle **funzioni discriminanti**.

Nella [[Linear Regression|regressione lineare]] il modello predittivo è una funzione lineare del tipo $y(x) = w_0 + w_1x$ (possibilmente applicando qualche [[Some Base Function|base function]] ad $x$).
Possiamo **generalizzare** questo modello, adattandolo alla **classificazione**: la classificazione è fatta attraverso un **generalized linear model** $$y(x) = f(w_0 + w_1x)$$ dove $f$ è una funzione **non lineare** con codominio in $\left[0,1\right]$.

Secondo il **generalized linear model** i *margini delle regioni* corrispondono alle soluzioni di $$y(x) = c$$ per qualche $c$ fissato.
Perciò i margini saranno della forma $$w_0+w_1x = f^{-1}(c)$$ e quindi avremo dei **margini lineari**.
La funzione inversa $f^{-1}$ è anche nota come **inverse function**.

## Generative Approaches
L'approccio [[#^00923c|3]] invece è di tipo **generativo**.
Infatti l'output del modello è una **distribuzione di probabilità**, la quale può essere usata per **generare** nuovi elementi (random) delle classi.
Comprando un elemento alle classi, possiamo poi identificare quella classe che meglio lo rappresenta (in termini di probabilità).

----
# [[Linear Discriminant Function in Binary Classification]]
# [[Linear Discriminant Analysis - Fisher Linear Discriminant]]
# [[Perceptron]]