Per semplicità, restringiamoci al [[Supervised Learning#Secondo Approccio|secondo approccio]].

Dato un qualsiasi elemento $x \in \mathcal{X}$, il suo target $t \in \mathcal{Y}$ e un **predittore** $h$, definiamo:
- **Error**: l'errore derivante dal confronto tra $h(x)$ e $t$.
- **Loss**: é una **funzione** usata per misurare un **errore** $$L: \mathcal{Y} \times \mathcal{Y} \to \mathbb{R}$$ $$L(h(x), t) \in \mathbb{R}$$
- **Prediction Risk**: Il **rischio** di una predizione $h(x)$ è semplicemente l'applicazione della funzione loss $$\mathcal{R}(h(x),x)=L(h(x),t)$$
  Nel caso in cui invece non esiste una funzione che definisce la relazione $f(x) = t$, bensì abbiamo una distribuzione $p(t \vert x)$ (per ogni valore di $t$), allora possiamo definire il rischio $\mathcal{R}$ in termini probabilistici come la media $$\mathcal{R}(h(x),x) = \mathbb{E} \left[ L(h(x),t) \right] = \int_{\mathcal{Y}} L(h(x),t) \cdot p(t \vert x) \,dt$$ o nel caso di classificazione $$\mathcal{R}(h(x),x) = \mathbb{E} \left[ L(h(x),t) \right] = \sum_{t \in \mathcal{Y}} L(h(x),t) \cdot p(t \vert x)$$

Ragionevolmente, la predizione **ottimale** $y^*$ è quella che **minimizza** il rischio.
Ovvero
- nel caso semplice $$y^* = \arg \min_{y \in \mathcal{Y}} L(y, f(x))$$ dove $f(x)$ è la funzione che mette in relazione $\mathcal{X}$ ad $\mathcal{Y}$.
- nel caso probabilistico $$y^* = \arg \min_{y \in \mathcal{Y}} \mathbb{E}_p\left[ L(y, t) \right] = \arg \min_{y \in \mathcal{Y}} \int_{\mathcal{Y}} L(y,t) \cdot p(t \vert x) \,dt$$ dove $p$ è la distribuzione dei target $t \in \mathcal{Y}$ rispetto all'elemento $x$.

Questa predizione ottima $y^*$ è anche nota come **stimatore Bayesiano**.

Purtroppo in contensti reali non si conosce mai ne la funzione $f(\cdot)$ ne la distribuzione $p( \cdot \vert x)$.
Ciò che possiamo fare è quindi definire un **rischio** rispetto ai nostri **dati** a disposizione nel training set $\mathcal{T}$.

Tale misura è nota come **rischio empiricio** ed è definita come come la media $$\overline{\mathcal{R}}_{\mathcal{T}}(h) = \frac{1}{\vert \mathcal{T} \vert} \sum_{(x,t) \in \mathcal{T}}L(h(x),t)$$