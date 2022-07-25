# Myerson’s Lemma
Negli [[2 - Mechanism Design Basics|appunti precedenti]] è stata introdotta la [[2 - Mechanism Design Basics#Vickrey's Second-Price Auctions|Vickrey's Second-Price Auctions]] e provato che essa garantisce le [[2 - Mechanism Design Basics#^e83fd1|tre proprietà desiderabili]].

Per estendere la garanzia di queste proprietà desiderabili ad aste più complesse, come la [[2 - Mechanism Design Basics#Sponsored Search Auctions|sponsored search auctions]], abbiamo introdotto [[2 - Mechanism Design Basics#Our Design Approach|l'approccio da adottare]].

Per esempio, il [[2 - Mechanism Design Basics#^40f690|primo punto]]  può essere implementato con un ***approccio greedy*** semplicemente assegnando gli slot in ordine *non crescente* di offerte, ovvero lo slot $j$-esimo è assegnato al player che ha fatto la $j$-esima offerta più alta.
Infatti è facile verificare che tale approccio <u>massimizza</u> il [[2 - Mechanism Design Basics#^db3c63|social surplus]], inoltre è computabile in [[2 - Mechanism Design Basics#^92a5ec|tempo polinomiale]] semplicemente **ordinando** in ordine *non crescente* le offerte in $b$. ^cbc983

Per implementare il [[2 - Mechanism Design Basics#^9a8ea2|secondo punto]] invece verrà enunciato e dimostrato ***Myerson’s Lemma***.

------------------
## Single-Parameter Environments
Introduciamo l'*environment* che sarà utile a dimostrare il *lemma di Myerson*.

Abbiamo $n$ partecipanti ad un'asta, ognuno dei quali possiede una **valutazione privata** $v_i$ del valore che otterrebbe dalla vincita dell'asta.

Sia $X$ l'insieme delle **soluzioni ammissibili**.
Tale insieme $X$ è composto da vettori $n$-dimensionali $\underline{x} = (x_1, ..., x_n) \in \mathbb{R}^n$, dove $x_i$ indica una sorta di *"ammontare"* dato al partecipante $i$-esimo. ^43b362

Per esempio, nell'asta a [[2 - Mechanism Design Basics#Single item auction|asta a signolo oggetto]] $X$ è formato da vettori di 0-1 con **al più** un 1, nella posizione corrispondente al vincitore.
Per esempio, se abbiamo $n=6$ partecipanti e il vincitore dell'asta risulta essere il player $i = 4$ il corrispondente vettore in $X$ risulta essere $(0,0,0,1,0,0)$.
Al più 1, perché nel caso in cui non si riesce a definire un vincitore, il vettore relativo risulta essere composto da tutti zeri.
Osserviamo quindi che $\sum_i x_i \leq 1$.

Oppure, in un asta con $k$ identici oggetti, ognuno dei quali assegnati a $k$ differenti vincitori, avremo un vettore di 0-1 tale che $\sum_i x_i \leq k$.

Nella sponsored search auction invece, $X$ invece è l'insieme dei vettori $n$-dimensionali tali che, se al partecipante $i$-esimo viene assegnato lo slot $j$ allora avremo che in posizione $i$ del vettore corrispondente ci sarà il [[2 - Mechanism Design Basics#^af7b13|CTR]] $\alpha_j$.
In tutte le altre posizione (ovvero le posizioni dei partecipanti che non hanno vinto) invece ci sarà uno $0$.
Dato che per definizione di [[2 - Mechanism Design Basics#^af7b13|CTR]] esso è una **distribuzione di probabilità**, allora anche in questo caso avremo che $\sum_i x_i \leq 1$.

## Allocation and Payment Rules
Ricordiamo che nel problema dell'asta bisogna effettuare due scelte: chi vince e quanto paga.
Queste due decisioni sono formalizzate con le **allocation rule** (*regole di allocazione*, chi vince) e **payment rule** (*regole di pagamento*, quanto paga).

Perciò formalmente il problema d'asta è suddiviso in tre step:
1. Il banditore colleziona le offerte dei singoli partecipanti $\mathbf{b}=(b_1, ..., b_n)$. D'ora in avanti ci riferiremo a $\mathbf{b}$ con il termine **profilo di strategie**, o semplicemente **profilo**.
2. **allocation rule.** Viene scelto una assegnazione di vincitori accettabile $\mathbf{x}(\mathbf{b}) \in X \subseteq \mathbb{R}^n$ in funzione del del profilo $\mathbf{b}$. Per convezione indichiamo con $x_i(\mathbf{b})$ l'allocazione di player $i$-esimo in funzione del profilo.
3. **payment rule.** Vengono definiti i pagamenti $\mathbf{p}(\mathbf{b}) \in \mathbb{R}^n$  in funzione del del profilo $\mathbf{b}$. Per convezione indichiamo con $p_i(\mathbf{b})$ il pagamento del player $i$-esimo in funzione del profilo.

Dati quindi un profilo $\mathbf{b}$, una regola di assegnazione $\mathbf{x}$ e uno schema di pagamenti $\mathbf{p}$, definiamo in fine l'utilità del player $i$-esimo in funzione di $\mathbf{b}$ come
$$u_i(\mathbf{b}) = v_i \cdot x_i(\mathbf{b}) - p_i(\mathbf{b})$$
Osserviamo che siamo interessati ad analizzare pagamenti nell'intervallo
$$p_i(\mathbf{b}) \in \left[ 0, b_i \cdot x_i(\mathbf{b}) \right]$$
Sicuramente il pagamento deve essere <u>non negativo</u> (altrimenti non avrebbe senso).
Invece il vincolo $p_i(\mathbf{b}) \leq b_i \cdot x_i(\mathbf{b})$ garantisce invece che i partecipanti **sinceri** ricevano utilità <u>non negativo</u>.

------------------------------
## Myerson’s Lemma
> Def. (**Implementable Allocation Rule**) Una regola di allocazione $\mathbf{x}$ per un sistema a singolo parametro è detta **implementabile** se esiste uno schema di pagamento $\mathbf{p}$ tale che il meccanismo $(\mathbf{x}, \mathbf{p})$ è [[2 - Mechanism Design Basics#^a63f67|DSIC]].

^1898c6

Perciò implementare un meccanismo DSIC per un asta equivale a restringersi a tutte le regola di allocazione $\mathbf{x}$ che sono *implementabili*.

Una domanda che possiamo porci è:
> la regola di allocazione che massimizza il social surplus nella *sponserd search auction* (vedi [[#^cbc983|qui]]), ovvero quella che associa la $j$-esima offerta più alta al $j$-esimo slot migliore, risulta essere *implementabile*?

Per esempio, considerando l'asta a signolo oggetto, la regola di allocazione che assegna l'oggetto all'offerente migliore è implementabile: abbiamo infatti visto la [[2 - Mechanism Design Basics#Vickrey's Second-Price Auctions|second-price rule]], la quale è DSIC.

> Def. (**Monotone Allocation Rule**) Una regola di allocazione $\mathbf{x}$ per un sistema a singolo parametro è detta **monotona** se per ogni offerente $i$, e per ogni insieme di altre offerte $\mathbf{b}_{-i}$ (tutte le offerte tranne quella $i$-esima), l'allocazione $x_i(z, \mathbf{b}_{-i})$ del player $i$ è **non decrescente** nell'offerta $z$.
> 
> Più intuitivamente, indipendentemente dalle altre offerte, l'allocazione del player $i$ *non decresce al crescere della sua offerta*: se offro di più non posso andare peggio.

^3c29c4

Per esempio, la regola del miglio offerente è monotona, in quanto (fissando le altre offerte) se offrendo $b_i$ il player $i$ vince, se offre di più continua a vincere.

In questo caso dire che [[#^cbc983|l'apporccio greedy]] che massimizza il social surplus nella *sponserd search auction* è monotona.
Infatti, offrendo di più, un player può solo salire in classifica, ottenendo quindi uno slotto con CTR migliore.

### Theorem (Mayerson's Lemma)
Consideriamo un ambiente a singolo parametro, allora:
- ***(a)*** Una regola di allocazione $\mathbf{x}$ è *implementabile* <u>se e solo se</u> è *monotona*. ^d589da
- ***(b)*** Assumendo che $b_i = 0 \implies p_i(\mathbf{b}) = 0$, se $\mathbf{x}$ è *monotona*, esiste un <u>unico</u> *schema di pagamento* $\mathbf{p}$ tale che il meccanismo d'asta $(\mathbf{x}, \mathbf{p})$ è DSIC. ^3bc8f8
- ***(c)*** Il pagamento nel punto precedente può essere espresso con una *formula esplicita*. ^9c8cc8

### Proof of Myerson’s Lemma (informal)
Consideriamo un $\mathbf{x}$ monotono.
Per esempio nella seguente immagine possiamo vedere le regole di assegnazione per un generico player $i$ al crescere dell'offerta, per il problema dell'asta a singolo oggetto e per la ricerca sponsorizzata.

![](lecture03-1.png)

Supponimo che $\mathbf{x}$ sia *implementabile*, e cerchiamo di capire come può essere fatto uno schema di pagamento $\mathbf{p}$ in modo tale che il meccanismo d'asta $(\mathbf{x}, \mathbf{p})$ sia DSIC.

Fissiamo un player $i$ e un $\mathbf{b}_{-i}$ arbitrari.
Per comodità, indichiamo con $x(z)$ l'allocazione $x_i(z, \mathbf{b}_{-i})$ del player $i$ se dichiara $z$, e con $p(z)$ il pagamento $p_i(z, \mathbf{b}_{-i})$ che dovrà fare.

Consideriamo due valori $0 \leq z < y$.

 Sfruttando il fatto che  $(\mathbf{x}, \mathbf{p})$ è DSIC, se $v_i = z$ allora avremo che
 $$\underbrace{z \cdot x(z) - p(z)}_{u_i(z, \mathbf{b}_{-i})} \geq \underbrace{z \cdot x(y) - p(y)}_{u_i(y, \mathbf{b}_{-i})}$$
 Analogamente, se $v_i = y$ allora
  $$\underbrace{y \cdot x(y) - p(y)}_{u_i(y, \mathbf{b}_{-i})} \geq \underbrace{y \cdot x(z) - p(z)}_{u_i(z, \mathbf{b}_{-i})}$$
Riarrangiando queste due equazioni otterremo un range nel quale è contanuta la differenza di pagamenti $\Delta_p = p(y) - p(x)$
$$
	z \cdot \left[ x(y) - x(z) \right] \leq p(y) - p(z) \leq y \cdot \left[ x(y) - x(z) \right]
$$

^af2847

> Osserviamo che il precedente intervallo può esistere solamente se $\mathbf{x}$ è **monotona**.
> Infatti, supponiamo che $v_i = z$.
> Se $\mathbf{x}$ fosse non monotona, allora esisterebbe certamente un $y > z$ tale che $x(y) < x(z)$.
> In tal caso avremo che la differenza $x(y) - x(z) < 0$, e quindi avremo che $\Delta_p$ deve necessariamente essere
> $$ z \cdot \left[ x(z) - x(y) \right] \geq \Delta_p \geq y \cdot \left[ x(z) - x(y) \right] $$
> e questo intervallo è vuoto perche $z < y$.

A questo punto facciamo tendere $y$ dall'alto a $z$, ottenendo quindi che
$$\lim_{y \to z^+} \Delta_p = z \cdot \left[ x(y) - x(z) \right]$$
Cerchiamo ora di analizzare che succede a questo limite.

Dato che $x(\cdot)$ procede *"a scalini"*, possono capitare due cose: $z$ e $y$ si trovano in uno stesso scalino, oppure $z$ si trova in un punto di discontinuità e essendo che $y$ lo approssima da destra essi si trovano su due scalini differenti.

Nel primo caso allora il limite è $0$.
Nel secondo invece avremo che $\Delta_p$, ovvero il *"salto*" di pagamento che otterrebbe $i$ nel dichiarare $z$ oppure $y$, è pari al *"salto"* che c'è nella funzione $x(\cdot)$ nel pungo $z$ moltiplicato per $z$.
Ovvero
$$\Delta_p = z \cdot \left[ \text{salto di } x(\cdot) \text{ in } z \right]$$
Assumendo che $p(0) = 0$ è possibile ricavare che

$$p_i(b_i, \mathbf{b}_{-i}) = \sum_{j=1}^{\ell} z_j \cdot \left[ \text{salto di } x_i(\cdot, , \mathbf{b}_{-i}) \text{ in } z_j \right]$$ ^232507

dove $z_1, ..., z_\ell$ sono i punto di $x_i(\cdot, , \mathbf{b}_{-i})$ dove avvengono i salti, rispetto all'intervallo $\left[ 0, b_i \right]$.

> Un ragionamento simile si può applicare per funzioni $x$ che non sono necessariamente *"a gradini"*.
> Per esempio, consideriamo un $x$ derivabile, e dividiamo le precedenti disuguaglianze per $y-z$.
> $$\frac{z \cdot \left[ x(y) - x(z) \right]}{y-z} \leq \frac{p(y) - p(z)}{y-z} \leq \frac{y \cdot \left[ x(y) - x(z) \right]}{y-z}$$
> Come prima, facendo tendere $y$ a $z$ da destra otteremo che
> $$p'(z) = z \cdot x'(z)$$
> Da cui poi
> $$p_i(b_i, \mathbf{b}_{-i}) = \int_0^{b_i} z \cdot x'_i(z, \mathbf{b}_{-i}) \, dz$$

^539c5a


Ribadiamo che, partendo dall'assunzione che dall'assunzione che $x_i(\cdot)$ è monotona, abbiamo ricavato una [[#^232507|formula]] per lo schema di pagamanto $p_i(\cdot)$, tale che $p_i(\cdot)$ rispetta il vincolo di monotonia di $x_i(\cdot)$.
Dato che abbiamo ricavato $p_i(\cdot)$ come <u>unico</u> risultato di una equazione, allora possiamo dire che $p_i(\cdot)$ è <u>unica</u>.

Per completare la dimostrazione basta dimostrare che se $\mathbf{x}$ è monotona allora il meccanismo $(\mathbf{x}, \mathbf{p})$ (con lo schema $\mathbf{p}$ ricavato come appena visto) è DSIC.

Consideriamo il caso in cui un player $i$ dichiara il vero $b_i = v_i$.
La sua utilità sarà quindi pari a
$$u_i(b_i, \mathbf{b}_{-i}) = v_i \cdot x_i(z_\ell) - \sum_{j = 1}^\ell z_j \cdot (x_i(z_j) - x_i(z_{j-1}))$$
dove $z_0 = 0$ e $z_1, ..., z_\ell$ i punto di $x_i(\cdot, , \mathbf{b}_{-i})$ dove avvengono i salti, rispetto all'intervallo $\left[ 0, b_i \right]$.

**Immagine**

Vediamo ora che accadrebbe se dovesse offrire meno $b_i = y^{(-)} < v_i$.
In tal caso l'utilità sarà
$$u_i(y^{(-)}, \mathbf{b}_{-i}) = v_i \cdot x_i(z_m) - \sum_{j = 1}^m z_j \cdot (x_i(z_j) - x_i(z_{j-1}))$$
dove $z_0 = 0$ e $z_1, ..., z_m$ i punto di $x_i(\cdot, , \mathbf{b}_{-i})$ dove avvengono i salti, rispetto all'intervallo $\left[ 0, y^{(-)} \right]$.
Osserviamo che essendo $y^{(-)} < v_i$ allora $m \leq \ell$.

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
	Ovvero $u_i(v_i, \mathbf{b}_{-i}) > u_i(y^{(-)}, \mathbf{b}_{-i})$.

Simmetricamente, possiamo applicare lo stesso ragionamento nel caso in cui si dichiara di più $b_i = y^{(+)} > v_i$ (vedi [[Exercise Set 2#Exercise 12|Exercise 12]]).

Con lo stesso ragionamento si può dimostrare lo stesso anche nel caso di funzioni $x_i(\cdot)$ differenziabili $\square$.

![](lecture03-2.png)

### Osservazione sui pagamenti
Riformulando la [[#^232507|formula]] per lo schema di pagamenti, essa si può riscrivere come segue
$$ p_i(b_i, \mathbf{b}_{-i}) = b_i \cdot x_i(b_i) - \left[\sum_{j=1}^{\ell-1} (z_{j+1} - z_j) \cdot x_i(z_j) + (b_i - z_{\ell}) \cdot x_i(b_i) \right]$$
Graficamente si può intuire meglio
![|380](lecture03-5.png)

Si può ricavare lo stesso risultato anche nel caso di funzione di allocazione [[#^539c5a|differenziabile]]
$$\begin{align}
p_i(b_i, \mathbf{b}_{-i})
&= \int_0^{b_i} z \cdot x'_i(z, \mathbf{b}_{-i}) \, dz\\
&= z \cdot x_i(z, \mathbf{b}_{-i}) - \int_0^{b_i} x_i(z, \mathbf{b}_{-i}) \, dz \; \Bigg\vert^{b_i}_0\\
&= b_i \cdot x_i(b_i, \mathbf{b}_{-i}) - \int_0^{b_i} x_i(z, \mathbf{b}_{-i}) \, dz
\end{align}$$
semplicemente integrando per parti.

------------------------------
## Payment Formula for Sponsored Search Solved
Cerchiamo di capire come poter applicare la [[#^232507|formula]] per lo schema di pagamento al problema della *sponsored search auction*.

Ricordiamo che abbiamo $k$ slots, ordinati per i rispettivi valori di click-through-rates (CTRs) $\alpha_1 \geq \alpha_2 \geq ... \geq \alpha_k$.
Abbiamo anche una regola di assegnazione $\mathbf{x}(\mathbf{b}) \in \mathbb{R}^n$ che assegna il $j$-esimo slot più "migliore" al player che ha fatto la $j$-esima offerta più alta, per $j = 1, ..., k$, oppure $0$ per $j > k$.
Ricordiamo anche che tale regola è **monotona**, e che sotto assunzione di *sincerità* dei partecipanti garantisce la massimizzazione del *social surplus*.

Applicando il [[#Theorem Mayerson's Lemma|Myerson's lemma]] possiamo derivare uno schema di pagamento $\mathbf{p}$ (<u>unico</u>) tale che il meccanismo $(\mathbf{x}, \mathbf{p})$ sia DSIC.

Senza perdita di generalità, ordiniamo i partecipanti in ordine di offerta $b_1 \geq b_2 \geq ... \geq b_n$.
Intuitivamente, la funzione di assegnazione $\mathbf{x}$ sarà della seugente forma.

![|500](lecture03-4.png)

Per non avere un assegnazione pari a $0$, è necessario offrire di più di $b_k$.
Analogamente, per ottenere ancora di più (ovvero per *salire di gradino*) sarà necessario offrire più di $b_{k-1}$, e così via...

Seguendo la [[#^232507|formula]] avremo che
$$
\begin{align*}
p_k(\mathbf{b}) &= b_k \cdot \alpha_k\\
p_{k-1}(\mathbf{b}) &= b_{k-1}(\alpha_{k-1} - \alpha_k) + b_k \cdot \alpha_k\\
p_{k-2}(\mathbf{b}) &= b_{k-2}(\alpha_{k-2} - \alpha_{k-1}) + b_{k-1}(\alpha_{k-1} - \alpha_k) + b_k \cdot \alpha_k\\
&\vdots
\end{align*}
$$


In generale, quando si sale di gradino, il valore di assegnazione fa un salto di magnitudo $\alpha_j - \alpha_{j+1}$.
Perciò, fissato un qualsiasi $i$ e un qualsiasi $\mathbf{b}$ avremo
$$p_i(\mathbf{b}) = \sum_{j = i}^{k} b_j \cdot (\alpha_j - \alpha_{j+1})$$
dove $\alpha_{k+1} = 0$.