# Problem set 2
## Lecture 3 Exercises
### Exercise 9
Use [[3 - Myerson’s Lemma#Theorem Mayerson's Lemma|Myerson’s Lemma]] to prove that the [[2 - Mechanism Design Basics#Vickrey's Second-Price Auctions|Vickrey auction]] is the unique single-item auction that is [[2 - Mechanism Design Basics#^a63f67|DSIC]], always awards the good to the highest bidder, and charges losers 0.
#### Solution
È facile verificare che la regola di assegnazione $\mathbf{x}$ che assegna l'oggetto al più alto offerente è [[3 - Myerson’s Lemma#^3c29c4|monotona]].
Infatti, fissiando un $i$ e un $\mathbf{b}_{-i}$ e $B = \max{\{b_j : b_j \in \mathbf{b}_{-i}\}}$, si può vedere che al variare dell'offerta $b_i$ avremo che:
- $\mathbf{x}(b_i, \mathbf{b}_{-i}) = 0$ per ogni $b_i \leq B$.
- $\mathbf{x}(b_i, \mathbf{b}_{-i}) = 1$ per ogni $b_i > B$.

Il punto [[3 - Myerson’s Lemma#^d589da|(a)]] ci garantisce che se $\mathbf{x}$ è monotona allora è anche [[3 - Myerson’s Lemma#^1898c6|implementabile]] (ovvero abbiamo DSIC).
Il punto [[3 - Myerson’s Lemma#^3bc8f8|(b)]] invece garanti l'esistenza di un **unico** sistema di pagamento $\mathbf{p}$.
Il punto [[3 - Myerson’s Lemma#^9c8cc8|(c)]] ci dice invece che il pagamento è definito tramite una [[3 - Myerson’s Lemma#^232507|formula esplicita]], ottenendo che
$$
p_i(b_i, \mathbf{b}_{-i}) = \begin{cases}
B &\mbox{se } b_i > B\\
0 &\mbox{se } b_i \leq B
\end{cases}
$$
ovvero lo schema di pagamento del [[2 - Mechanism Design Basics#Vickrey's Second-Price Auctions|Vickrey auction]].

----------------------
### Exercise 10
Use the [[3 - Myerson’s Lemma#^af2847|"payment difference sandwich"]]  in the [[3 - Myerson’s Lemma#Proof of Myerson’s Lemma informal|proof of Myerson's Lemma]] to prove that if an allocation rule **is not** [[3 - Myerson’s Lemma#^3c29c4|monotone]], then it **is not** [[3 - Myerson’s Lemma#^1898c6|implementable]].
#### Solution
Abbiamo che dati due valori $0 < z \leq y$, se $\mathbf{x}$ è monotona allora
$$ z \cdot \left[ x(z) - x(y) \right] \geq \Delta_p \geq y \cdot \left[ x(z) - x(y) \right] $$
dove $$\Delta_p = p(y) - p(z)$$
Il precedente intervallo può esistere (ovvero è **non vuoto**) solamente se $\mathbf{x}$ è **monotona**.
Infatti, supponiamo che $v_i = z$.
Se $\mathbf{x}$ fosse **non** monotona, allora esisterebbe certamente un $y > z$ tale che $x(y) < x(z)$.
In tal caso avremo che la differenza $x(y) - x(z) < 0$, e quindi avremo che $\Delta_p$ deve necessariamente appartenere all'intervallo
$$ z \cdot \left[ x(z) - x(y) \right] \geq \Delta_p \geq y \cdot \left[ x(z) - x(y) \right] $$
e questo intervallo è vuoto perche $z < y$ $\square$.

----------------------
### Exercise 11
We concluded the [[3 - Myerson’s Lemma#Proof of Myerson’s Lemma informal|proof of Myerson’s Lemma]] by giving a *"proof by picture"* that coupling a monotone and piecewise constant allocation rule $\mathbf{x}$ with the payment formula

$$p_i(b_i, \mathbf{b}_{-i}) = \sum_{j=1}^{\ell} z_j \cdot \left[ \text{salto di } x_i(\cdot, , \mathbf{b}_{-i}) \text{ in } z_j \right]$$
where $z_1,...,z_\ell$ are the **breakpoints** of the allocation function $x_i(\cdot, \mathbf{b}_{-i})$ in the range $\left[0, b_i \right]$, yields a DSIC mechanism. ^8fc753

Where does the proof-by-picture break down if the piecewise constant allocation rule $\mathbf{x}$ is **not** monotone?
#### Solution
Osserviamo cosa accade *graficamente* se si dichiara di più del vero nel caso in cui $\mathbf{x}$ non è monotona.

![](exercise11-1.png)
Osservare che $p(b)$ (nel caso in cui $b>v$) racchiude un rettangolo tratteggiato.
Ciò sta ad indicare un "**pagamento negativo**", ovvero dei soldi da ricevere anziché saldare.
Ciò accade perché avviene un *"salto"* verso il basso, e quindi viene sottratto del pagamento anziché aggiunto.

In colcusione, se $\mathbf{x}$ è non monotona, potrebbe accadere di avere utilità maggiore dichiarando il falso (in questo caso di più).

-------------------
### Exercise 12
Give a purely algebraic proof that coupling a monotone and piecewise constant allocation rule x with the [[#^8fc753|payment rule]] yields a DSIC mechanism.

#### Solution
Fissiamo un player $i$ e consideriamo il caso in cui dichiara il vero $b_i = v_i$.
La sua utilità sarà pari a
$$u_i(b_i, \mathbf{b}_{-i}) = v_i \cdot x_i(z_\ell) - \sum_{j = 1}^\ell z_j \cdot (x_i(z_j) - x_i(z_{j-1}))$$
dove $z_0 = 0$ e $z_1, ..., z_\ell$ i punto di $x_i(\cdot, , \mathbf{b}_{-i})$ dove avvengono i salti, rispetto all'intervallo $\left[ 0, b_i \right]$.

**Immagine**

Vediamo ora cosa accadrebbe se dovesse offrire meno $b_i = y^{(-)} < v_i$.
In tal caso l'utilità sarà
$$u_i(y^{(-)}, \mathbf{b}_{-i}) = v_i \cdot x_i(z_m) - \sum_{j = 1}^m z_j \cdot (x_i(z_j) - x_i(z_{j-1}))$$
dove $z_0 = 0$ e $z_1, ..., z_m$ i punto di $x_i(\cdot, , \mathbf{b}_{-i})$ dove avvengono i salti, rispetto all'intervallo $\left[ 0, y^{(-)} \right]$.

Osserviamo che essendo $y^{(-)} < v_i$ allora $m \leq \ell$, ovvero il numero di gradini non può essere maggiore rispetto a prima.

- **Caso $m = \ell$**. In questo caso avremo che la differenza di utilità sarà pari a
	$$u_i(v_i, \mathbf{b}_{-i}) - u_i(y^{(-)}, \mathbf{b}_{-i}) = 0$$
- **Caso $m < \ell$**. In tal caso avremo che $p_i(b_i) > p_i(y^{(-)})$, perciò
	$$\begin{align*}
	u_i(v_i, \mathbf{b}_{-i}) - u_i(y^{(-)}, \mathbf{b}_{-i})
		&= v_i \cdot (x_i(v_i) - x_i(y^{(-)})) - p_i(v_i) + p_i(y^{(-)})\\
		&= v_i \cdot (x_i(v_i) - x_i(y^{(-)})) + \underbrace{\left[p_i(y^{(-)}) - p_i(v_i)\right]}_{< 0}\\
		&> v_i \cdot (x_i(v_i) - x_i(y^{(-)}))\\
		&= v_i \cdot \underbrace{(x_i(z_\ell) - x_i(z_m))}_{\geq 0}\\
		&\geq 0
	\end{align*}$$
	Ovvero $u_i(v_i, \mathbf{b}_{-i}) > u_i(y^{(-)}, \mathbf{b}_{-i})$ (non conviene dichiarare meno).

Simmetricamente, possiamo applicare lo stesso ragionamento nel caso in cui si dichiara di più $b_i = y^{(+)} > v_i$.

Infatti, l'utilità sarà
$$u_i(y^{(+)}, \mathbf{b}_{-i}) = v_i \cdot x_i(z_t) - \sum_{j = 1}^t z_j \cdot (x_i(z_j) - x_i(z_{j-1}))$$
dove $z_0 = 0$ e $z_1, ..., z_t$ i punto di $x_i(\cdot, , \mathbf{b}_{-i})$ dove avvengono i salti, rispetto all'intervallo $\left[ 0, y^{(+)} \right]$.

Osserviamo che essendo $y^{(+)} > v_i$ allora $t \geq \ell$, ovvero il numero di gradini non può essere minore rispetto a prima.

- **Caso $t = \ell$**. In questo caso avremo che la differenza di utilità sarà pari a
	$$u_i(v_i, \mathbf{b}_{-i}) - u_i(y^{(+)}, \mathbf{b}_{-i}) = 0$$
- **Caso $t > \ell$**. In tal caso avremo che $p_i(y^{(+)}) > p_i(b_i)$.
  Inoltre per monotonia $x_i(y^{(+)}) \geq x_i(v_i)$. Perociò avremo che  
	$$\begin{align*}
	u_i(v_i, \mathbf{b}_{-i}) - u_i(y^{(+)}, \mathbf{b}_{-i})
		&= v_i \cdot (x_i(v_i) - x_i(y^{(+)})) - p_i(v_i) + p_i(y^{(+)})\\
		&= v_i \cdot \underbrace{(x_i(v_i) - x_i(y^{(+)}))}_{\leq 0} + \left[p_i(y^{(+)}) - p_i(v_i)\right]\\
		&\geq p_i(y^{(+)}) - p_i(v_i)\\
		&> 0
	\end{align*}$$
	Ovvero $u_i(v_i, \mathbf{b}_{-i}) > u_i(y^{(+)}, \mathbf{b}_{-i})$ (non conviene dichiarare di più).

Con lo stesso ragionamento si può dimostrare anche nel caso di funzioni $x_i(\cdot)$ differenziabili $\square$.

-------------------
## Lecture 4 Exercises
### Exercise 13
Consider the following extension of the sponsored search setting discussed in lecture.
Each bidder $i$ now has a **publicly known quality** $\beta_i$ (in addition to a private valuation $v_i$ per click).

As usual, each slot $j$ has a CTR $\alpha_j$, and $\alpha_1 \geq \alpha_2 \geq ··· \geq \alpha_k$.
We assume that if bidder $i$ is placed in slot $j$, its probability of a click is $\beta_i\alpha_j$ — thus, bidder $i$ derives value $v_i\beta_i\alpha_j$ from this outcome.

Describe the surplus-maximizing allocation rule in this generalized sponsored search setting.
Argue that this rule is monotone.
Give an explicit formula for the per-click payment of each bidder that extends this allocation rule to a DSIC mechanism.
#### Solution
$$
x_i(b_i, \mathbf{b}_{-i}) = \begin{cases}
\beta_i\alpha_j &\text{se a } i \text{ viene assegnato lo slot } j\\
\\
0 &\text{altrimenti}
\end{cases}
$$
Senza perdita di generalità, ordiniamo i player nel seguente ordine
$$
v_1\beta_1 \geq v_2\beta_2 \geq ... \geq v_n\beta_n
$$
In tal modo, quello che si vuole massimizzare è
$$
\sum_{i=1}^{k} v_i\beta_i\alpha_i
$$
Ordinando nello stesso modo in base alle offerte dichiarate (ovvero $b_1\beta_1 \geq ... \geq b_n\beta_n$).
Una regola di allocazione che massimizza il social-surplus è quindi quella che assegna i $k$ slot ai primi $k$ player secondo l'ordine precedentemente descritto.
In tal caso quindi avremo come social surplus la seguente quantità
$$
\sum_{i=1}^{k} b_i \cdot x_i(b_i) = \sum_{i=1}^{k} b_i\beta_i\alpha_i
$$
Se tale regola di allocazione $\mathbf{x}$ è DSIC allora il social-surplus sarà **massimizzato**.

Grazie al [[3 - Myerson’s Lemma#Theorem Mayerson's Lemma|Mayerson's Lemma]] per dimostrare che $\mathbf{x}$ è DSIC basta dimostrare che $\mathbf{x}$ è [[3 - Myerson’s Lemma#^3c29c4|monotona]].

Fissiamo un $i$, un $\mathbf{b}_{-i}$ e vediamo come varia $x_i(z, \mathbf{b}_{-i})$ al crescere di $z$.
Consideriamo senza perdita di generalità che i CTR siano ordinati in modo tale che $\beta_1\alpha_1 \geq ... \geq \beta_k\alpha_k$ (esattamente come le offerte).
È facile verificare che $x_i(\cdot, \mathbf{b}_{-i})$ è fatta a gradini esattamente come nell'asta sposorizzata semplice
![](exercise-13.png)
facendo salti di magnitudo $\beta_j\alpha_j - \beta_{j+1}\alpha_{j+1}$ nel punto $b_j\beta_j$.

Perciò, per $z \in \left[ b_j\beta_j, \;\; b_{j-1}\beta_{j-1}\right.)$ avremo che $x_i(z) = \beta_j\alpha_j$, per ogni $j \leq k$.
Aumentando $z \geq b_{j-1}\beta_{j-1}$ avremo che $x_i(z)$ **non diminuisce mai**, assumendo un valore $\beta_{j-1}\alpha_{j-1} \geq \beta_j\alpha_j$.
Perciò $x_i(\cdot)$ è *monotona*.

Infine lo schema di pagamento sarà
$$
p_i(b_i, \mathbf{b}_{-i}) = \sum_{j=i}^{k} \beta_j \cdot \left[ b_j \cdot (\alpha_j - \alpha_{j+1}) \right]
$$
dove $\alpha_{k+1} = 0$ $\square$.

--------------------------------
### Exercise 14
Consider an arbitrary single-parameter environment, with feasible set $X$.
The surplus-maximizing allocation rule is $\mathbf{x}(\mathbf{b}) = arg \max_{(x_1,...,x_n) \in X} \sum_{i=1}^{n}b_ix_i$.
Prove that this allocation rule is monotone.
*\[You should assume that ties are broken in a deterministic and consistent way, such as lexicographically.\]*

#### Solution
