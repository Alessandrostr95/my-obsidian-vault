Supponiamo di voler fare una **classificazione binaria**, e quindi di voler trovare l'iperpiano di separazione che meglio separa le nostre due classi.

Spesso capita che il dataset non sia **ben separabile**, quindi un modo creativo per approcciare la problematica è:
1. **ammborbidire** il concetto di *separabilità*.
2. **aggiungiamo dimensioni** al nostro dominio, in modo tale che le due classi siano meglio separabili.

Consideriamo il classico problema della classificazione binaria, in cui ad ogni elemento del domino $\mathbf{x}$ gli si vuole assegnare il target $y = -1$ se appartiene alla classe $C_0$ e $y=1$ se appartiene alla classe $C_1$.

Per approcciare il problema possiamo utilizzare un [[Naive Bayes Classifier|modelli lineare]] del tipo $$y(\mathbf{x} \vert \mathbf{w}) = \text{sign}(\mathbf{w}^T \mathbf{x} + w_0)$$

Sia $(\mathbf{x}_i,t_i)$ un qualsiasi elemento del training set.
Definiamo il **margine funzionale** dei parametri $\mathbf{w}$ rispetto all'item $(\mathbf{x}_i,t_i)$ rispetto  come $$\overline{\gamma}_i = t_i \cdot (\mathbf{w}^T\mathbf{x}_i + w_0) = t_i \cdot y(\mathbf{x}_i \vert \mathbf{w})$$
Osserviamo che l'elemento $\mathbf{x}$ viene classificato correttamente dal nostro modello $y(\cdot \vert \mathbf{w})$ se e solo se $\overline\gamma_i > 0$.
Tenendo in considerazione l'intero dataset $\mathcal{T} = \lbrace (\mathbf{x}_1,t_1), ..., (\mathbf{x}_n,t_n)\rbrace$ definiamo **margine funzionale** dei parametri $\mathbf{w}$ rispetto a $\mathcal{T}$ come $$\overline\gamma = \min_{1 \leq i \leq n} \overline\gamma_i$$ ^0f29df

Definiamo invece con $\gamma_i$ il **margine geometrico** rispetto all'item $(\mathbf{x}_i, t_i)$, come il **prodotto** tra $t_i$ e la distanza tra $\mathbf{x}_i$ è l'iperpiano $\mathbf{w} \cdot \mathbf{x}= 0$ di separazione delle due classi.
![[ML/img/ML_11_1.png]] ^89f316

La distanza tra un punto $\mathbf{x}_i$ e l'iperpiano $\mathbf{w} \cdot \mathbf{x} + w_0= 0$ può essere calcolato come $$\frac{\mathbf{w} \cdot \mathbf{x}_i + w_0}{\Vert \mathbf{w} \Vert}$$
Pericò avremo che il **margine geometrico** risulta essere $$\gamma_i = t_i \cdot \left( \frac{\mathbf{w} \cdot \mathbf{x}_i + w_0}{\Vert \mathbf{w} \Vert} \right) = \frac{\overline\gamma_i}{\Vert \mathbf{w} \Vert}$$
Analogamente a [[#^0f29df|prima]], definiamo il margine rispetto al training set $\mathcal{T} = \lbrace (\mathbf{x}_1,t_1), ..., (\mathbf{x}_n,t_n)\rbrace$ come il minimo **margine geometrico** dei parametri $\mathbf{w}$ $$\gamma = \min_{1 \leq i \leq n} \gamma_i$$ ^6abbe9

```ad-note
Osserviamo che il [[#^6abbe9|margine geometri]] è **invariante rispetto allo scaling**.
Ovvero se sostituiamo $\mathbf{w} + w_0$ con $c \mathbf{w} + cw_0$ (per un qualsiasi valore $c$) il margine geometrico rimane invariato
$$\gamma_i = t_i \cdot \left( \frac{c\mathbf{w}}{\Vert c\mathbf{w} \Vert} \cdot \mathbf{x}_i + \frac{cw_0}{\Vert c\mathbf{w} \Vert} \right) = t_i \cdot \left( \frac{\mathbf{w}}{\Vert \mathbf{w} \Vert} \cdot \mathbf{x}_i + \frac{w_0}{\Vert \mathbf{w} \Vert} \right)$$
```

^cfe44c

Geometricamente possiamo interpretare $\gamma$ come meta della larghezza della striscia più larga possibile, centrata in $\mathbf{w}\cdot\mathbf{x} + w_0 = 0$, che non contiene nessun punto di $\mathcal{T}$.
![[ML_11_2.png]]

Tutti i punti $\mathbf{x}$ presenti lungo i *margini* sono detti **vettori di supporto**, da cui il nome del modello. ^38a5d9

# Optimal Margin Classifier
Dato un dataset $\mathcal{T}$, assumendo per ora **linearmente separabile**, si vuole trovare un iperpiano di separazione $\mathbf{w}\cdot \mathbf{x} + w_0 = 0$ che **massimizzi** il [[#^6abbe9|margine di separazione]].
Infatti, trovare un iperpiano che massimizza i margini di separazione equivale ad avere una **maggiore confidenza** nelle predizioni.

Si può modellare questo problema con un problema di **programmazione lineare**
$$\begin{align}
\max_{\mathbf{w}} &\;\;\gamma\\
s.t. &\;\;\gamma_i = \frac{t_i}{\Vert \mathbf{w} \Vert}(\mathbf{w}\cdot\mathbf{x}_i + w_0) \geq \gamma &\forall i = 1, ..., n
\end{align}$$
oppure 
$$\begin{align}
\max_{\mathbf{w}} &\;\;\gamma\\
s.t. &\;\;\gamma_i = t_i(\mathbf{w}\cdot\mathbf{x}_i + w_0) \geq \gamma \cdot \Vert \mathbf{w} \Vert &\forall i = 1, ..., n
\end{align}$$


Sfruttando l'[[#^cfe44c|invarianza moltiplicativa]] del margine geometrico possiamo far sì che $\gamma = 1$, basta considerare i soli parametri $\mathbf{w}$ tali che $\Vert \mathbf{w} \Vert = \dfrac{1}{\gamma}$.
Ciò equivale anche a considerare uno **scaling** dei dati, tale che il margine massimo tra le due classi è 2.

Perciò riformuliamo il problema come 
$$\begin{align}
\max_{\mathbf{w}} &\;\;\gamma = \Vert \mathbf{w} \Vert^{-1}\\
s.t. &\;\;\gamma_i = t_i(\mathbf{w}\cdot\mathbf{x}_i + w_0) \geq 1 &\forall i = 1, ..., n
\end{align}$$

Diciamo che un punto $\mathbf{x}$ è **attivo** se $t_i (\mathbf{w} \cdot \mathbf{x} + w_0) = 1$, nel caso $\geq 1$ il punto è invece detto **inattivo** o **non attivo**.
Osservare che per simmetria, devono esistere almeno 2 punti attivi.

Osserviamo che **massimizzare** $\Vert \mathbf{w} \Vert^{-1}$ equivale a **minimizzare** $\Vert \mathbf{w} \Vert^{2}$.
Possiamo quindi formalizzare il problema come un problema di **ottimizzazione convesso quadratico**. $$\begin{align}
\min_{\mathbf{w}} &\;\;\frac{1}{2}\Vert \mathbf{w} \Vert^{2}\\
s.t. &\;\;\gamma_i = t_i(\mathbf{w}\cdot\mathbf{x}_i + w_0) \geq 1 &\forall i = 1, ..., n
\end{align}$$ ^d4c925


## Lagrangiano
Sia un problema di ottimizzazione convessa
$$\begin{align}
\min_{x} &\;\;f(x)\\
s.t. &\;\;g_i(x) \leq 0 &\forall i = 1, ..., n
\end{align}$$

Definiamo il **lagrangiano** la funzione $$L(x, \pmb\lambda) = f(x) + \sum_{i=1}^{n}\lambda_i g_i(x)$$

Consideriamo il problema di **massimizzazione** rispetto a $\pmb\lambda$ 
$$\max_{\pmb\lambda\,:\, \lambda_i \geq0}L(x, \pmb\lambda) = f(x) + \max_{\pmb\lambda}\sum_{i=1}^{n}\lambda_i g_i(x)$$

```ad-note
Nel caso in cui i vincoli sono del tipo $g_i(x) \geq 0$, il lagrangiano è del tipo $$L(x, \pmb\lambda) = f(x) - \sum_{i=1}^{n}\lambda_ig_i(x)$$
Tutto il resto rimane invariato.
```


Se $x$ è una **soluzione ammissibile**, allora avremo che $g_i(x) \leq 0$ (per ogni $i=1,...,n$), perciò di conseguenza nella massimizzazione del lagragiano deve essere vero che $\pmb\lambda = \mathbf{0}$.
Ovvero, definiamo con $\Omega = \lbrace x : g_i(x) \leq 0 \forall i=1,...,n \rbrace$ l'insieme delle soluzioni ammissibili per il problema di minimizzazione.
Avremo quindi che
$$x = \arg\min_{x \in \Omega} f(x) \implies \max_{\pmb\lambda\,:\, \lambda_i \geq0}L(x, \pmb\lambda) = f(x)$$
$$\implies \min_{x \in \Omega}f(x) = \min_{x \in \Omega}\max_{\pmb\lambda\,:\, \lambda_i \geq0}L(x, \pmb\lambda)$$

Per il teorema della **dualità forte** abbiamo che $$\min_{x \in \Omega}f(x) = \min_{x \in \Omega}\max_{\pmb\lambda\,:\, \lambda_i \geq0}L(x, \pmb\lambda) = \max_{\pmb\lambda\,:\, \lambda_i \geq0} \min_{x \in \Omega} L(x, \pmb\lambda)$$ ^1a63c9

### Karus-Kuhn-Tucker conditions
Le condizioni necessarie e sufficienti a finché **esista una soluzione ottima** $(x^*,\pmb\lambda^*)$ per il [[#^1a63c9|problema]] sono:
1. $$\frac{\partial}{\partial x}L(x, \pmb\lambda) \Bigg\vert_{x^*, \pmb\lambda^*} = 0$$
2. $$\frac{\partial}{\partial \lambda}L(x, \pmb\lambda_i) \Bigg\vert_{x^*, \pmb\lambda^*} = g_i(x^*)\geq 0 \;\; \forall i=1,...,n$$
3. $$\lambda^*_i\geq 0 \;\; \forall i=1,...,n$$
4. $$\lambda^*_ig_i(x^*) =  0 \;\; \forall i=1,...,n$$


## Application to SVM
Poniamo
- $f(x) = \frac{1}{2}\Vert\mathbf{w}\Vert^2$
- $g_i(x) = t_i(\mathbf{w}\cdot\mathbf{x}_i + w_0) - 1 \geq 0$

Il rispettivo [[#Lagrangiano]] sarà $$L(\mathbf{w},\pmb\lambda) = \frac{1}{2}\Vert\mathbf{w}\Vert^2 - \sum_{i=1}^{n} \lambda_i\left(t_i(\mathbf{w}\cdot\mathbf{x}_i + w_0) - 1\right) = \frac{1}{2}\Vert\mathbf{w}\Vert^2 - \sum_{i=1}^{n} \lambda_it_i\mathbf{w}\cdot\mathbf{x}_i - w_0\sum_{i=1}^{n} \lambda_it_i + \sum_{i=1}^{n} \lambda_i$$
L'ottimo al [[#^d4c925|problema di ottimizzazione iniziale]] avrò ottimo equivalente all'ottimo di $$\max_{\pmb\lambda \,:\, \lambda_i}\min_{\mathbf{w}}L(\mathbf{w}, \pmb\lambda)$$
```ad-warning
title: Important
Osservare che, quando un elemento $\mathbf{x}_i$ è un [[#^38a5d9|vettore di supporto]], allora il vincolo $t_i(\mathbf{w}\cdot \mathbf{x}_i + w_0) - 1 \geq 0$ è **saturato**, ovvero $$t_i(\mathbf{w}\cdot \mathbf{x}_i + w_0) - 1 = 0$$
Per tutti gli altri punti non vettori di supporto invece avremo la disuguaglianza stretta $> 0$.

Ciò significa che per i vettori non di supporto $x_i$ deve essere necessariamente vero che $\lambda^*_i = 0$.
```

^6ff4f5

Applicando le [[#Karus-Kuhn-Tucker conditions|condizioni di Karus-Kuhri-Tucker]] deve necessariamente essere che una soluzione ottima $\mathbf{w}^*$ deve rispettare i vincoli
- $$\frac{\partial}{\partial \mathbf{w}}L(\mathbf{w}, \pmb\lambda) \Bigg\vert_{\mathbf{w}^*} = \mathbf{w}^* - \sum_{i=1}^{n}\lambda_it_i\mathbf{x}_i = \pmb{0}$$
- $$\frac{\partial}{\partial w_0}L(\mathbf{w}, \pmb\lambda) \Bigg\vert_{w_0^*} = -\sum_{i=1}^{n}\lambda_it_i = 0$$
- $$t_i(\mathbf{w}^*\cdot\mathbf{x}_i + w_0^*) - 1 \geq 0$$
- $$\lambda_i \geq 0$$
- $$\lambda_i\left(t_i(\mathbf{w}^*\cdot\mathbf{x}_i + w_0^*) - 1\right) \geq 0$$


Ciò implica che $$\mathbf{w}^* = \sum_{i=1}^{n}\lambda_it_i\mathbf{x}_i$$

Calcoliamo il lagrangiano $L(\mathbf{w}^*, \pmb\lambda)$ coi parametri ottimi
$$\begin{align}
\min_{\mathbf{w}}L(\mathbf{w}, \pmb\lambda)
&= L(\mathbf{w}^*, \pmb\lambda)\\
&= \frac{1}{2}\Vert \mathbf{w}^* \Vert^2 - \sum_{i=1}^{n} \lambda_it_i\mathbf{w}^*\cdot\mathbf{x}_i - w_0^*\underbrace{\sum_{i=1}^{n} \lambda_it_i}_{=0} + \sum_{i=1}^{n}\lambda_i\\
&= \frac{1}{2}\left(\sum_{i=1}^{n}\lambda_it_i\mathbf{x}_i\right)\cdot\left(\sum_{i=1}^{n}\lambda_it_i\mathbf{x}_i\right) - \sum_{i=1}^{n} \lambda_it_i\cdot\left(\sum_{i=1}^{n}\lambda_it_i\mathbf{x}_i\right)\cdot\mathbf{x}_i + \sum_{i=1}^{n}\lambda_i\\
&= \frac{1}{2}\left(\sum_{i=1}^{n}\sum_{j=1}^{n}\lambda_i\lambda_jt_it_j(\mathbf{x}_i \cdot \mathbf{x}_j)\right) - \sum_{i=1}^{n}\sum_{j=1}^{n}\lambda_i\lambda_jt_it_j(\mathbf{x}_i \cdot \mathbf{x}_j) + \sum_{i=1}^{n}\lambda_i\\
&= \sum_{i=1}^{n}\lambda_i - \frac{1}{2}\sum_{i=1}^{n}\sum_{j=1}^{n}\lambda_i\lambda_jt_it_j(\mathbf{x}_i \cdot \mathbf{x}_j)\\
&= L(\pmb\lambda)
\end{align}$$
Abbiamo quindi espresso il problema in funzione dei soli parametri $\pmb\lambda$, ovvero risolviamo il solo **problema duale**.

Ovviamente il tuo vale anche nel caso di applicazioni di [[Some Base Function|base function]] sugli elementi $\mathbf{x}_i$.
Definendo la seguente funzione kernel $$\kappa(\mathbf{x}_i, \mathbf{x}_j) = \phi(\mathbf{x}_i) \cdot \phi(\mathbf{x}_j)$$ ridefiniamo quindi il lagrangiano come
$$\max_{\pmb\lambda \,:\, \lambda_i \geq 0}\min_{\mathbf{w}}L(\mathbf{w}, \pmb\lambda) = \max_{\pmb\lambda \,:\, \lambda_i \geq 0}L(\pmb\lambda) = \max_{\pmb\lambda \,:\, \lambda_i \geq 0}\left(\sum_{i=1}^{n}\lambda_i - \frac{1}{2}\sum_{i=1}^{n}\sum_{j=1}^{n}\lambda_i\lambda_jt_it_j\kappa(\mathbf{x}_i \cdot \mathbf{x}_j)\right)$$ ^c9f4f7

```ad-info
title: Passing from primal to dual
- **Svantaggi**: il numero di variabili da ottimizzare aumentano da $d$ (o $m$ se si usano funzioni base) ad $n$.
- **Vantaggi**: dato che però $\lambda_i^* = 0$ per ogni elemento del dataset che **non** è un vettore di supporto, allora il numero di parametri da stimare è $\vert \mathcal{S} \vert \leq n$, dove $\mathcal{S}$ è l'insieme dei vettori di supporto.
```


## Deriving coefficients
Supponiamo di aver ottenuto il vettore ottimo $\pmb\lambda^*$ che massimizza il [[#^c9f4f7|duale]].
Da $\pmb\lambda^*$ possiamo ricavare i coefficienti
$$\mathbf{w}^* = \sum_{i=1}^{n}\lambda^*_it_i\phi(\mathbf{x}_i)$$

Per quanto riguarda $w_0^*$ [[#^6ff4f5|ricordiamo]] che, per ogni vettore di supporto $\mathbf{x}_k$ deve essere **saturata** il vincolo
$$\begin{align}
1
&= t_k(\mathbf{w}^*\cdot\phi(\mathbf{x}_k) + w_0^*)\\\\
&= t_k\left(\sum_{i=1}^{n}\lambda^*_it_i\phi(\mathbf{x}_i)\cdot\phi(\mathbf{x}_k) + w_0^*\right)\\\\
&= t_k\left(\sum_{i=1}^{n}\lambda^*_it_i\kappa(\mathbf{x}_i,\mathbf{x}_k) + w_0^*\right)\\\\
&= t_k\left(\sum_{\mathbf{x} \in \mathcal{S}}\lambda^*_it_i\kappa(\mathbf{x},\mathbf{x}_k) + w_0^*\right)
\end{align}$$
dove $\mathcal{S}$ è l'insieme dei vettori di supporto.

Dato che $t_k = \pm 1$ allora $1/t_k = t_k$, perciò
$$t_k = \mathbf{w}^*\cdot\phi(\mathbf{x}_k) + w_0^*$$
$$\implies w_0^* = t_k - \mathbf{w}^*\cdot\phi(\mathbf{x}_k) = t_k - \sum_{\mathbf{x} \in \mathcal{S}}\lambda^*_it_i\kappa(\mathbf{x},\mathbf{x}_k)$$

Una soluzione più precisa si può ottenere tramite la media 
$$w^*_0 = \frac{1}{\vert \mathcal{S} \vert} \sum_{\mathbf{x}_k \in \mathcal{S}}\left( t_k - \sum_{\mathbf{x}_i \in \mathcal{S}}\lambda^*_it_i\kappa(\mathbf{x}_i,\mathbf{x}_k) \right)$$

## Classification through SVM
Un qualsiasi punto $\mathbf{x}$ del dominio può essere classificato, data una funzione base $\phi$ e una funzione kernel $\kappa$ come segue
$$y(\mathbf{x}) = \text{sign}\left( \mathbf{w}^* \cdot \phi(\mathbf{x}) + w_0^*\right) = \text{sign}\left( \sum_{i=1}^{n}\lambda^*_it_i\phi(\mathbf{x}_i) \cdot \phi(\mathbf{x}) + w_0^*\right) = \text{sign}\left( \sum_{\mathbf{x}_i \in \mathcal{S}}\lambda^*_it_i\kappa(\mathbf{x}_i,\mathbf{x}) + w_0^*\right)$$

-------
# [[SVM - removing separability assumption]]
# [[SVM and Gradient Descent]]