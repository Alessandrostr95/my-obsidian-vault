# Test goodness-of-fit (bontà d'adattamento)
Supponiamo di avere un campione di dati $\mathbf{X} = X_1, ..., X_n$, e che esistano $k$ **classi di eventi possibili** $A_1, ..., A_k$.
Ovvero ogni elemento $X_i$ può risolversi in uno solo dei $k$ possibili eventi.
Perciò, indichiamo con $p_j$ la probabilità che avvenga l'evento $j$-esimo $$P(X_i \in A_j) = p_j \;\; \forall i=1,...,n$$

Dato che gli eventi formano una partizione di $\Omega$, avremo quindi che $$\sum_{i=1}^{k} p_i = 1$$

Possiamo modellare il nostro esperimento con una [[Distribuzioni#Multinomiale multivariata|distribuzione multinomiale]].
Indichiamo con $$Y_j = \sum_{i=1}^{n} 1(\{X_i \in A_j\}) \;\; \forall j =1, ..., k$$ ovvero $Y_j$ **conta** il numero di volte che nel campione $\mathbf{X}$ occorre l'evento $j$-esimo $E_j$.

Avremo quindi che $$\mathbf{Y} = (Y_1, ..., Y_k) \sim \text{Multi}(n, k, (p_1, ..., p_k))$$

Dato che i parametri $\mathbf{p} = (p_1, ..., p_k)$ sono ignoti, consideriamo la seguente *[[Hypothesis Testing#^349ec3|ipotesi nulla]]* $$H_0: \mathbf{p} = \mathbf{p}^{(0)}$$

Definiamo ora la statistica $$T = \sum_{i=1}^{k}\frac{(Y_i - np^{(0)}_i)^2}{np^{(0)}_i}$$
(Vedere media distribuzione multinomiale!).

È possibile dimostrare ([TO DO]) che (**sotto ipotesi nulla**) *asintoticamente* $T \sim \chi^2_{k-1}$ segue una distribuzione [[Distribuzioni#Chi Quadro|chi-quadro]] con $k-1$ gradi di libertà.
Il numero di gradi di libertà è $k-1$ perché siamo vincolati dal fatto che $p_1 + ... + p_k = 1$.

Riconducendoci quindi ad una distribuzione *chi-quadro* siamo in gradi di eseguire uno [[Test più comuni#t-test di Student|t-test]].
Consultando quindi la [[tabella dei quinatili di una chi-quadro]] possiamo facilmente definire un test con precisione $1-\alpha$.

Un modo più semplice ed empirico per vedere questo test è il seguente:
> Supponiamo di avere $A_1, ..., A_k$ come insieme di possibili eventi.
> Prendiamo un campione di $n$ elementi.
> Siano $O_1, ..., O_k$ la frequenza (il numero di volte) con il quale i singoli eventi si sono presentati, dette anche **frequenze osservate**.
> Supponiamo che ci aspettavamo di osservare le frequenze $E_1, ..., E_k$, dette **frequenze teoriche** o **attese**.
> Nel nostro caso $E_i$ sta ad indicare $np^{(0)}_i$, per ogni evento $i$.
> 
> Evento | A_1 | A_2 | ... | A_k
> --|--|--|--|--
> Frequenze osservate | O_1 | O_2 | ... | O_k
> Frequenze attese | E_1 | E_2 | ... | E_k
> 
>Allora la nostra statistica sarà $$T = \sum_{i=1}^{k}\frac{(O_i - E_i)^2}{E_i} \sim \chi^2_{k-1}$$

## Esempio
Un dado viene lanciato 2000 volte e i vari risultati escono con le seguenti sequenze
Esito | Occorrenza
-|-
1|388
2|322
3|314
4|316
5|344
6|316

Si può affermare che il dado è non equo?
Consideriamo come *ipotesi nulla* $$H_0: p_i = 1/6 \; \forall i =1,...,6$$ ovvero che il dado sia equo.

Sotto ipotesi nulla avremo la seguente statistica $$T = \sum_{i=1}^{6}\frac{(Esito_i - 2000/6)^2}{2000/6} = 12.616$$
Sappiamo che $T$ segue una distribuzione $\chi^2$ con $6-1 = 5$ gradi di libertà.

Supponiamo di voler avere un errore $\alpha = 0.05 = 5\%$.
Dando uno sguardo alle [[quantili.pdf|tabelle dei quantili]] abbiamo che
$$P(T \leq 11.070) = 0.95 \implies P(T = 12.616) \leq P(T > 11.070) = 1 - 0.95 = \alpha$$
Ovvero la probabilità che $T$ abbia assunto tale valore, sotto ipotesi $H_0$ è minore di $\alpha$, dobbaimo perciò **rifiutare** $H_0$.

------------------
# Test del chi quadro per tabelle di contingenza
Un altro tipo di test $\chi^2$ è quello per **tabelle di contingenza**.
Supponiamo di avere un campione di $n$ elementi, classificabili due **differenti criteri**.
Per esempio, supponiamo di aver campionato da 5 differenti stati dell'unione europea $n$ persone, e di aver chiesto che frutto preferiscono mangiare d'estate tra cocco 🥥, anguria 🍉 o ananas 🍍.

Possiamo rappresentare tali dati come una tabella dove abbiamo $r$ righe che rappresentano le classi del primo criterio, e $c$ colonne che rappresentano le classi del secondo criterio.
\ | 🥥 | 🍉 | 🍍
-|-|-|-
🇮🇹| 40 | 80 | 21
🇫🇷 | 45 | 67 | 35
🇩🇪 | 58 | 17 | 69
🇪🇸 | 12 | 33 | 91
🇵🇹| 7 | 101 | 38

Il test $\chi^2$ per **tabelle di contingenza** serve per verificare l'**ipotesi di indipendenza tra i due criteri di valutazione** (nel nostro caso voglia valutare l'indipendenza tra nazionalità e gusti).

Possiamo modellare i nostri dati come una [[Distribuzioni#Multinomiale multivariata|multinomiale]] $\{X_{ij} : 1 \leq i \leq r \land 1 \leq j \leq c\}$ dove ogni evento $(i,j)$ avviene con probabilità $p_{ij}$.
Se c'è indipendenza tra i due criteri allora avremo che esistono due vettori $\underline{\alpha} = (\alpha_1, ..., \alpha_r)$ e $\underline{\beta} = (\beta_1, ..., \beta_c)$ tali che $$\forall i,j \; p_{ij} = \alpha_i \cdot \beta_{j}$$
Questa sarà la nostra **ipotesi nulla**.

Per stimare $\underline{\alpha}, \underline{\beta}$ iniziamo col fare le **somme marginali** di righe e colonne, ovvero



$$r_i = \sum_{j=1}^{c} X_{ij}$$
$$c_j = \sum_{i=1}^{r} X_{ij}$$
Osserviamo che $$n = \sum_{i=1}^{r} r_i = \sum_{j=1}^{c} c_j$$
\ | 🥥 | 🍉 | 🍍 | $r_i$ 
-|-|-|-|-
🇮🇹 | 40 | 80  | 21 | 141
🇫🇷 | 45 | 67  | 35 | 147
🇩🇪 | 58 | 17  | 69 | 144
🇪🇸 | 12 | 33  | 91 | 136
🇵🇹 | 7  | 101 | 38 | 146
$c_j$  |162 | 298 | 254 | $n = 714$

Approssimiamo quindi il valore atteso come $$\mathbb{E}\left[ X_{ij} \right] = np_{ij} \approx n \cdot \frac{r_i}{n} \cdot \frac{c_j}{n} = \frac{r_i c_j}{n}$$
A questo punto, definiamo la statistica $$T = \sum_{i,j} \frac{\left(X_{ij} - \dfrac{r_ic_j}{n}\right)^2}{\dfrac{r_ic_j}{n}}$$
Sotto ipotesi nulla $H_0$ avremo che $T \sim \chi^2_{(r-1)(c-1)}$ ovvero segue una [[Distribuzioni#Chi Quadro|distribuzione chi-quadro]] con $(r-1) \cdot (c-1)$ **gradi di libertà**.
Perciò noi avremo
$$T = \frac{(40 - 31.99)^2}{31.99} + ... + \frac{(38 - 51.938)^2}{51.938} = 203.277$$

Nel nostro caso, la probabilità che $T$ assuma il valore osservato è pressoche nulla sotto ipotesi $H_0$, perciò troppo rilevante per essere solamente un caso.

Possiamo rifiutare $H_0$, e supporre che invece esista qualche dipendenza tra i gusti e la nazionalità.

## Esempio semplice
Supponiamo di aver testato due antibiotici, e di aver contato i guariti alla fine della cura sperimentale.
Raccogliamo tutti i dati, e otteniamo la seguente tabella di contingenza.

Trattamento \ Esito | Guariti | Non guariti | Totale
:-|:-:|:-:|:-:
Antibiotico 1 | 52 | 10 | 62
Antibiotico 2 | 40 | 21 | 61
**Totale** | 92 |  31 | 123

Stimiamo i valori attesi con la formula 
$$np_{ij} = \frac{r_ic_j}{n}$$
Trattamento \ Esito Atteso | Guariti Attesi | Non guariti Attesi
:-|:-:|:-:
Antibiotico 1 | 46.37 | 15.63
Antibiotico 2 | 45.63 | 15.37

Ci si chiede quindi se effeticamente il primo trattamento è migliore del primo oppure no, ovvero se il numero dei guarti dipende dal trattamento (la nostra **ipotesi nulla** $H_0$).

Sotto $H_0$ avremo quindi
$$T = \frac{(52-46.37)^2}{46.37} + \frac{(10-15.63)^2}{15.63} + \frac{(40-45.63)^2}{45.63}+ \frac{(21-15.37)^2}{15.37} = 5.46$$

Sotto ipotesi nulla $T \sim \chi^2_1$ e quindi la probabilità che abbia assunto il valore osservato $5.46$ è compresa tra il $2.5\%$ e l'$1\%$ (ovvero è poco probabile che $H_0$ sia vero).

Possiamo rifiutare $H_0$, e affermare che 
> **Antibiotico 1 è migliore di Antibiotico 2 con una certezza del 97.5\%**

Infatti, osservando le [[quantili.pdf|tabelle]] avremo che 
$$P(T \geq 5.46) \leq P(T \geq 5.024) = 0.025$$
$$P(T \geq 5.46) \geq P(T \geq 6.635) = 0.01$$