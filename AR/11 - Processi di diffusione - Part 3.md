Lezioni predenti collegate:

-   [[9 - Processi di diffusione - Part 1|← Lezione 9]]
-   [[10 - Processi di diffusione - Part 2|← Lezione 10]]

# Processi di diffusione in presenza di Compatibilità
Nelle lezioni precedenti abbiamo visto come un gioco di coordinazione molto semplice possa portare ad una vasta quantità di domande e relative analisi non banali.
Una prima generalizzazione è stata passare da un modello *omogeneo*, in cui tutti i nodi traevano uno stesso vantaggio dall'adottare una nuova tecnologia `A`, ad uno *eterogeneo*, in cui ciascun individuo ricava un personale guadagno dall'adottare l'innovazione.

In tutti i modelli analizzati però si assumeva la **mutua esclusione** degli stati `A-B`. In un contesto reale però succede spesso che due stati coesistono in un individuo.
Ad esempio, `A` e `B` potrebbero essere lingue diverse che coesistono lungo un confine nazionale, oppure `A` e `B` potrebbero essere due applicazioni di messaggistica differenti.

Infatti gli individui che si trovano al confine tra un'area in cui prevale `A` ed una in cui prevale `B`, conviene adottare entrambi gli stati: ovvero essere in qualche modo "bilingua".

Adottare però due stati contemporaneamente può comportare un costo aggiuntivo non trascurabile.
Un individuo sceglie di utilizzare entrambi i comportamenti disponibili, barattando la maggiore facilità di interazione con persone di più tipi, contro il costo di dover acquisire e mantenere entrambe le forme di comportamento (cioè i costi di dover imparare una lingua aggiuntiva, mantenere due diverse versioni di una tecnologia e così via ...).

Un'oservazione da fare riguardo il costo del "bilinguismo", è che tale *effort*[^1] viene pagato <u>una sola volta</u>.
Perciò, se si è in contatto con tanta gente nello stato `A` ed altrettanta nello stato `B`, potrebbe convenire adottare il doppio stato `AB`, in qunato il guadagno ricavato sarebbe molto maggiore dell'unico costo che pago nell'avere il doppio stato: *il gioco ne vale la candela*.

Modellando in maniera più formale, possiamo definire un nuovo *coordination game* in accordo alla seguente tabella

![Nuovo coordination game con opzione di doppio stato (bilingua).](ar-lesson11-img1.png)

dove con $(a,b)^+$ si indica il massimo tra $a$ e $b$.

Indichiamo ora con $V_A, V_B, V_{AB}$ l'insieme dei nodi negli stati `A`, `B` e `AB` rispettivamente, i quali compongono una *partizione* dell'insieme $V$ di tutti gli individui.
Perciò, fissando con $c \geq 0$ il costo di adozione del *doppio stato* `AB`, possiamo dire che il guadagno complessivo del nodo $u$ che adotta `AB` è pari a $$\Big(\sum_{v \in N(u) \cap V_A} a\Big) + \Big(\sum_{v \in N(u) \cap V_B} b\Big) + \Big(\sum_{v \in N(u) \cap V_{AB}} (a,b)^+\Big) - c$$

Indichiamo quindi con $p_A(u), p_B(u), p_{AB}(u)$ i rispettivi guadagni che ha il nodo $u$ nell\'assumere gli stati `A`, `B` o `AB` come segue

$$
\begin{align*}
	p_A(u) &= \sum_{v \in N(u) \cap V_A} a + \sum_{v \in N(u) \cap V_{AB}} a \\
	p_B(u) &= \sum_{v \in N(u) \cap V_B} b + \sum_{v \in N(u) \cap V_{AB}} b\\
	p_{AB}(u) &= \Big(\sum_{v \in N(u) \cap V_A} a\Big) + \Big(\sum_{v \in N(u) \cap V_B} b\Big) + \Big(\sum_{v \in N(u) \cap V_{AB}} (a,b)^+\Big) - c
\end{align*}
$$

Per rendere meglio le idee consideriamo un esempio.
Consideriamo ancora una volta la catenta infinita $\mathbb{Z}$, e poniamo come parametri $(a,b,c) = (5,3,1)$.
Supponiamo di partire da un solo nodo iniziatore $u$, e senza perdita di generalità consideriamo solamente lo sviluppo del porcesso alla sua destra (tanto a sinistra è simmetrico).

Prendiamo il nodo $v$ alla usa destra.
I suoi guadagni nell\'essere negli stati `A`, `B` e `AB` saranno
$$
\begin{align*}
	p_A(v) &= 5\\
	p_B(v) &= 3\\
	p_{AB}(v) &= 5+3-1 = 7
\end{align*}
$$

Perciò a $v$ converrà diventare *bilingua* e adottare il doppio stato `AB`.

Consideriamo ora il succesivo nodo $w$ sulla destra. I suoi guadagni saranno
$$
\begin{align*}
	p_A(w) &= 5\\
	p_B(w) &= 3+3 = 6\\
	p_{AB}(w) &= 5+3-1 = 7
\end{align*}
$$
Perciò anche a $w$ converrà passare ad `AB`.

Adesso, la situazione rispetto a $v$ è cambiata, in quanto ora ha un vicino nello stato `A` ed uno nello stato `AB`.
Infatti anche i suoi guadagni sono ora cambiati, e risultano essere
$$
\begin{align*}
	p_A(v) &= 10\\
	p_B(v) &= 3\\
	p_{AB}(v) &= 5+5-1 = 9
\end{align*}
$$

perciò ora $v$ passa nello stato `A`.

È facile convincersi che si genera una cascata completa di `A`.

![$(a,b,c) = (5,3,1)$ e $V_0 = \lbrace u \rbrace$.](ar-lesson11-img2.gif)

Analizzeremo ora il processo di diffusione nel nuovo modello con compatibilità, e cercheremo di capire in quali situazioni si ha una cascata completa.

## Analisi
Dalle precedenti lezioni sappiamo che nel modello omogeneo, `A` <u>non</u> si diffonde su reti infinite di grado finito se la sua soglia d'adozione è $q > \frac{1}{2}$, ovvero se $a$ non è **almeno** pari al valore di $b$.

Uno studio di *Kleinberg* del 2007 ha invece dimostrato uno strano comportamento riguardo il modello con compatibilità, ovvero che:
- `A` si diffonde facilmente se $a$ è parecchio più grande di $c$ (**ragionevole**)
- `A` fa fatica a diffondersi se $c$ è parecchio grande rispetto ad $a$ (**ragionevole**)
- `A` fa fatica a diffondersi anche se $c$ non è molto grande rispetto ad $a$ (**strano**)

Analizziamo cosa succede nella rete infinita più semplice, la catena infinita $\mathbb{Z}$.

Dato che fare un'analisi su tre parametri $a,b,c$ risulta molto complessa, conviene **normalizzare** $b$ ad 1, e quindi descrivere il processo solo in funzione di $a(b) = a$ e $c$. 
Perciò, in questo modello semplificato (ma non meno espressivo) la soglia d'adozione di `A` sarà $$q(b) = q = \frac{1}{a+1}$$
Consideriamo la situazione in cui c'è un unico nodo iniziatore $x$.
Il suo vicino $u$ avrà un vicino in $V_A$ e l'altro in $V_B$.

![Il nodo $u$ deve scegliere quale stato conviene adottare.](ar-lesson11-img3.png)

Avremo che i guadagni nell\'adottare uno stato sono
$$
\begin{align*}
	p_A(u) &= a\\
	p_B(u) &= 1\\
	p_{AB}(u) &= a + 1 - c
\end{align*}
$$
Sicuramente $u$ assume `A` se $p_A(u) \geq p_B(u)$ e se $p_A(u) \geq p_{AB}(u)$
$$
\begin{equation}
   \begin{cases}
     p_A(u) \geq p_B(u)\\
     p_A(u) \geq p_{AB}(u)
   \end{cases}
   \implies
   \begin{cases}
     a \geq 1\\
     c \geq 1
   \end{cases}
\end{equation}
$$

Invece, $u$ rimane nello stato `B` se $p_B(u) > p_A(u)$ e se $p_B(u) > p_{AB}(u)$
$$
\begin{equation}
   \begin{cases}
     p_b(u) \geq p_A(u)\\
     p_b(u) \geq p_{AB}(u)
   \end{cases}
   \implies
   \begin{cases}
     a < 1\\
     a < c
   \end{cases}
\end{equation}
$$

Infine, $u$ adotta il bilinguismo `AB` se $p_{AB}(u) > p_A(u)$ e se $p_{AB}(u) \geq p_B(u)$
$$
\begin{equation}
   \begin{cases}
     p_{AB}(u) > p_A(u)\\
     p_{AB}(u) \geq p_B(u)
   \end{cases}
   \implies
   \begin{cases}
     c < 1\\
     c \leq a
   \end{cases}
\end{equation}
$$
Possiamo riassumere tutto in un grafico con assi $a$-$c$.

![Grafico $a$-$c$.](ar-lesson11-img4.png)

Dalle precedenti osservazioni possiamo trarre due prime conclusioni:
- se il nodo $u$ rimane nello stato `B`, allora la diffusione di `A` è bloccata e non potrà mai più procedere in alcun modo.
-  se $u$ invece passa <u>direttamente</u> allo stato `A`, allora certamente avverrà una cascata completa di `A` in maniera diretta, senza transitare mai per `AB`.

Resta ora da capire che succede se $u$ passa allo stato `AB`.
In tal caso, avremo che il nodo $v$ avrà un nodo vicino nello stato `AB` e uno nello stato `B`, come nella seguente immagine

![Il nodo $v$ deve scegliere quale stato conviene adottare.](ar-lesson11-img5.png)

Dato che stiamo assumendo che $u$ è ricaduto nella zone verde del grafico, fissiamo le relazioni $c < 1$ e $c \leq a$.
Ci chiediamo ora:
- Per quali valori la diffusione di `A` si blocca?
- Oppure, per quali valori si genera una cascata completa di `A`?
- Oppure ancora, è possibile che si generi una cascata completa di `AB`?

Iniziamo col calcolare i vantaggi che avrebbe $v$ ad adottare uno stato
$$
\begin{align*}
	p_A(v) &= a\\
	p_B(v) &= 2\\
	p_{AB}(v) &= \max{\lbrace a, 1 \rbrace} + 1 - c
\end{align*}
$$

A questo punto possiamo dividere l\'analisi in due casi:
1. $a < 1$, ovvero quando la coppia $(a,c)$ cade all'interno del triangolo verde $(0,0), (1,0), (1,1)$.
2. $a \geq 1$, ovvero quando la coppia $(a,c)$ cade all\'interno della restante area verde.

Consideriamo il caso `(1)`.
In tale situazione avremo che i rispettivi guadagni sono
$$
\begin{align*}
	p_A(v) &< 1\\
	p_B(v) &= 2\\
	p_{AB}(v) &= 2 - c
\end{align*}
$$

ovvero avremo che $p_A(v) < p_B(v)$ e $p_{AB}(v) < p_B(v)$.
In tal csaso accadrà che $v$ adotta `B`, e quindi la diffusione di `A` si blocca dopo un'altro passo.
Possiamo quindi estendere la parte blu del grafico come nell'immagine seguente

![Estensione area blu.](ar-lesson11-img6.png)

Non resta che considerare il caso `(2)`, quando $a \geq 1$.

Anche in questo caso conviene dividere l'analisi in due casi disgiunti:
il caso $1 \leq a < 2$ e il caso $a > 2$.

Nel caso $a > 2$ avremo che i guadagni di $v$ sono
$$
\begin{align*}
	p_A(v) &= a > 2\\
	p_B(v) &= 2\\
	p_{AB}(v) &= a + 1 - c > a
\end{align*}
$$

Possiamo dire $v$ adotterà `AB`, perché abbiamo la catena di disuguaglianze $p_{AB}(v) > p_A(v) > p_B(v)$.
Dopo un passo osserviamo che il nodo $u$ passa dalla stato `AB` allo stato `A`.
Possiamo quindi concludere che quando quando sono vere le seguenti condizioni, dopo una fase transitoria allo stato `AB`, avviene una cascata completa di `A`.
$$
\begin{cases}
	c < 1\\
	a > 2
\end{cases}
$$

![](ar-lesson11-img7.png)

Infine, consideriamo l\'ultimo caso $1 \leq a < 2$.
In queste condizioni, i valori di guadagno di $v$ risultano essere
$$
\begin{align*}
	p_A(v) &= a < 2\\
	p_B(v) &= 2\\
	p_{AB}(v) &= a + 1 - c
\end{align*}
$$

Dato che $p_A(v) < p_B(v)$, sappiamo che in qeull'area il processo di diffusione di `A` si ferma, e quindi non avremo mai una cascata completa di `A`.
Poi, $p_B(v) > p_{AB}(v)$ se e solo se $2 > a + 1 - c$, ovvero se $a < c + 1$. In tal caso $v$ adotterà lo stato `B`, e in un altro passo verrà bloccato il processo di diffusione di `A`.
Viceversa, se $a > c + 1$, allora avremo che $p_{AB}(v) > p_B(v)$, e quindi $v$ adotterà lo stato `AB`. Come prima, dopo un passo il nodo $u$ adotterà `A`, e così via dopo il nodo $v$, scatenando una cascata completa di `A`, dove però tutti i nodi transitano prima per `AB`.

Il grafico risultante finale è il seguente

![](ar-lesson11-img8.png)

Riassumendo:
- quando la coppia $(a,c)$ cade nell'area blu, allora il processo di diffusione di `A` si blocca definitivamente in un passo.
- quando la coppia $(a,c)$ cade nell'area gialla, si scatena una cascata completa di `A`, senza che nessun nodo transiti mai attraverso lo stato `AB`.
- quando la coppia $(a,c)$ cade nell'area verde, tutti i nodi passeranno allo stato `A` dopo però essere prima transitati attraverso lo stato `AB`.

[^1]: sforzo.
