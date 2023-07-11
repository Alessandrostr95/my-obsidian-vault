Il **Gradient Boosting** è una tecnica di [[Boosting]], che segue la seguente idea:
1. facciamo il fitting di un [[Adaboost#Additive Model|modello additivo]] $\sum_{j=1}^{m}\alpha_j y_j(x)$.
2. ad ogni stage, ingroduciamo un **weak learner** che compensi le **carenze** di quelli precedenti.
3. le carenze sono identificate da tutti quei punti che hanno avuto un **alto peso**.

Assumiamo di avere già a disposizione il modello 
$$y^{(1)}(\mathbf{x}_i) = y^{(1)}_i$$ con **residui** $t_i - y^{(1)}_i$.

Creiamo un **nuovo dataset** con target $$\mathbf{t} - y^{(1)}(\mathbf{X})$$

Definiamo ora un **modello** $h^{(1)}$ addestrato su questo **nuovo dataset**.
Certamente il modello $$y^{(2)}(\mathbf{x}) = y^{(1)}(\mathbf{x}) + h^{(1)}(\mathbf{x})$$ è un modello che **migliora** il comportamento di $y^{(1)}$.
Il ruolo di $h^{(1)}(\mathbf{x})$ è proprio quello di **compensare** le carense di $y^{(1)}(\mathbf{x})$.
Se $y^{(2)}(\mathbf{x})$ è ancora carente, possiamo definire dei nuovi target $\mathbf{t} - y^{(2)}(\mathbf{X})$ su cui addestrare il modello $h^{(2)}$, per poi definire $$y^{(3)}(\mathbf{x}) = y^{(2)}(\mathbf{x}) + h^{(2)}(\mathbf{x}) = y^{(2)}(\mathbf{x}) = y^{(1)}(\mathbf{x}) + h^{(1)}(\mathbf{x}) + h^{(2)}(\mathbf{x})$$
In maniera iterativa, avremo $$y^{(t+1)}(\mathbf{x}) = y^{(t)}(\mathbf{x}) + h^{(t)}(\mathbf{x}) = y^{(2)}(\mathbf{x}) = y^{(1)}(\mathbf{x}) + \sum_{j=1}^{t} h^{(j)}(\mathbf{x})$$
# Gradient Boosting and Gradient Descent
Il gradient boosting è in realtà correlato al [[Gradient Descent]].
Consideriamo la [[Some Loss Functions#Quadratic Loss|square loss]] $$L(y,t) = \frac{1}{2}(t-y)^2$$
Quello che fa il gradient boosting è cercare di **minimizzare** il [[Prediction Risk#^9cd1a0|rischio empirico]] $$R = \sum_{i=1}^{n} L(t_i, y_i) = \frac{1}{2} \sum_{i=1}^{n} (t_i - y(x_i))^2$$
aggiustando ad ogni step le predizioni $y(x_1), ..., y(x_n)$.

Consideriamo le singole derivate rispetto ai $y(x_i)$
$$\begin{align}
\frac{\delta}{\delta y(x_i)} R 
&= \frac{\delta}{\delta y(x_i)} \sum_{i=1}^{n} L(t_i, y_i)\\ 
&= \sum_{i=1}^{n} \frac{\delta}{\delta y(x_i)}  L(t_i, y_i)\\ 
&= \frac{1}{2}\sum_{i=1}^{n} \frac{\delta}{\delta y(x_i)} (t_i - y(x_i))^2\\ 
&=  -(t_i - y(x_i))\\ 
&=  y(x_i) - t_i
\end{align}$$

Perciò, quando definiamo delle nuove label
$$\mathbf{t} - y(\mathbf{X})$$
stiamo in realtà definendo il datastet
$$(\mathbf{X}, \mathbf{t} - y(\mathbf{X})) = (\mathbf{X},- \nabla R) = \bigg\lbrace \left(\mathbf{x}_1, - \frac{\partial R}{\partial y_1}\right), ..., \left(\mathbf{x}_n, - \frac{\partial R}{\partial y_n}\right) \bigg\rbrace$$

Ci ora chiediamo qual è il significato di cercare un predittore $h$ che si adatti a questo nuovo dataset.
L'idea è che il nuovo target rappresentano gli errori del modello $y$.
Quindi, al crescere dell'errore $y(\mathbf{x}_i)$ il predittore $h(\mathbf{x}_i)$ dovrebbe essere un valore molto negativo (dato il segno $-$ ), **aggiustando** quindi il tiro di $y(\mathbf{x}_i)$.

Il gradient boosting può essere quindi riassunto col seguente algoritmo

1. $y^{(1)}(\mathbf{x}) = \frac{1}{n}\sum_{i=1}^{n} t_i$
2. Per $k = 1, ..., m$
	1. Computiamo il **gradiente negativo** $$-g_i^{(k)} = t_i - y^{(k)}(\mathbf{x}_i)$$
	2. Facciamo il fit di un **weak learner** $h^{(k)}$ considerando il dataset $(\mathbf{x}_i, -g^{(k)}_i)_{i=1, ..., n}$
	3. Deriviamo il predittore $y^{(k+1)}(\mathbf{x}) = y^{(k)}(\mathbf{x}) + h^{(k)}(\mathbf{x})$


# Gradient Boosting for Regression
Il beneficio del formulare l'algoritmo di gradient boosting usando i **gradienti negativi** è che ci consente di considerare **[[Some Loss Functions|altre funzioni loss]]**.

Per esempio, la [[Some Loss Functions#Quadratic Loss|loss quadratica]] è semplice da usare matematicamente, però non è robusta rispetto a errori piccoli (infatti si da troppo peso errori più grandi).

Potremmo per esempio utilizzare:
- la [[Some Loss Functions#Absolute Loss|absolute loss]] se vogliamo dare uguale peso a tutti gli errori 
	- $$L(y, t) = \vert t - y \vert$$
	- $$-g = \text{sign}(t - y)$$
- la [[Some Loss Functions#Huber Loss|Huber loss]] che penalizza in **maniera quadratica** tutti gli errori più piccoli piccoli di $\delta$, e tutti gli altri in maniera **lineare**
	- $$L(y,t) = \begin{cases}
\dfrac{1}{2}(y-t)^2 &\vert t-y \vert \leq \delta\\
\\
\delta \cdot \vert t-y \vert - \dfrac{\delta^2}{2} &\vert y-t \vert > \delta\\
\end{cases}$$
	- $$- g = \begin{cases}
y - t &\vert t-y \vert \leq \delta\\
\\
\delta \cdot \text{sign}(t-y) &\vert t-y \vert > \delta
\end{cases}$$


