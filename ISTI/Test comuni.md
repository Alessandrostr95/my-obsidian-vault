# Test più comuni

# z-test
## Distribuzione normale, varianza nota, media sconosciuta - part 1
Sia il [[Random Sample#Random Sample|campionamento]] $X_1, ..., X_n$ da una popolazione [[Distribuzioni#Normale|normale]] $N(\theta, \sigma^2)$, con $\theta$ sconosciuto.
Consideriamo l'*[[Hypothesis Testing#^349ec3|ipotesi nulla]]* $$H_0: \theta = \theta_0$$ e quella *alternativa* $$H_1: \theta = \theta_1$$

Consideriamo ora il valore $$T = \frac{\overline{X} - \theta_0}{\sigma/\sqrt{n}}$$ e definiamo il test $$\text{rifiuto } H_0 \iff |T| > c$$ dove $c$ verrà scelto appositamente per rendere il valore di $\alpha = P(\mathbf{X} \in C \vert \theta = \theta_0)$ (rifiuto $H_0$ nonstante sia vera) il più piccolo possibile.

Calcoliamo qual è il valore di $c$ migliore che ci da una probabilità di errore $\alpha$ abbastanza piccola
Sotto l'ipotesi nulla (ovvero se $\theta = \theta_0$) avremo che $T \sim N(0,1)$, perciò scgliendo per esempio $c = 1.96$ avremo che
$$\begin{align*}
\alpha
&= P(\mathbf{X} \in C \vert \theta = \theta_0)\\
&= P(|T| > 1.96)\\
&= 1 - P(-1.96 \leq T \leq 1.96)\\
&= 1 - (\Phi(1.96) - \Phi(-1.96))\\
&\approx 5\%
\end{align*}$$
 Se per esempio invece volessimo trovare un $c$ che ci dia una probabilità di errore più bassa, per esempio $\alpha \approx 2.5\%$ dobbiamo risolvere l'equazione in $c$
 $$\begin{align*}
0.025
&= P(\mathbf{X} \in C \vert \theta = \theta_0)\\
&= P(|T| > c)\\
&= 1 - P(-c \leq T \leq c)\\
&= 1 - (\Phi(c) - \Phi(-c))
\end{align*}$$

^3d2fb2

ovvero risolvendo $$0.975 = \Phi(c) - \Phi(-c)$$
Perciò nel nostro caso, con $c = 2.25$ avremo all'incirca il $2.5\%$ che il test commetta un errore.

## Distribuzione normale, varianza nota, media sconosciuta - part 2
Supponiamo di essere nella situazione del [[#Distribuzione normale varianza nota media sconosciuta - part 1|precedente esercizio]], con la differenza che le ipotesi sono del tipo
$$\begin{align*}
H_0: \theta \leq \theta_0\\
H_1: \theta > \theta_0\\
\end{align*}$$
Si rifiuta $H_0$ se $$T = \frac{\overline{X} - \theta_0}{\sigma/\sqrt{n}} > c$$ ovvero se $T$ è *"troppo"* più grande di un certo $c$.

Supponiamo che vogliamo avere una probabilità di errore $\alpha \approx 5\%$.
Allora calcoliamo
$$\begin{align*}
0.05
&= P(\mathbf{X} \in C \vert \theta = \theta_0)\\
&= P(T > c)\\
&= 1 - P(T \leq c)\\
&= 1 - \Phi(c)
\end{align*}$$
Perciò per trovare il $c$ desiderato in funzione di $\alpha$ dato basta risolvere l'equazione $$1 -\alpha = \Phi(c)$$
Nel nostro caso $c \approx 1.65$.

----------------
## Distribuzione approssimativamente normale, varianza nota, media sconosciuta
[TODO]

------------
# t-test (di Student)
## Distribuzione normale, varianza e media sconosciute
Consideriamo un [[Random Sample#Random Sample|campionamento]] $X_1, ..., X_n$ da una popolazione [[Distribuzioni#Normale|normale]] $N(\mu, \sigma^2)$, con $\mu, \sigma$ **ignoti**.

Approssimiamo $\sigma^2$ con la [[Random Sample#Media campionaria|media campionaria]] $S^2$, e definiamo la quantità $$T = \frac{\overline{X} - \mu_0}{S/\sqrt{n}}$$ dove $\mu_0$ fa parte della nostra ipotesi, del tipo $H_0: \mu \leq \mu_0$ oppure $H_0: \mu = \mu_0$.

Esiste un risultato di [William Gosset](https://it.wikipedia.org/wiki/William_Sealy_Gosset), sotto pseudonimo *Student*, che dimostra che $T$ segue una [[Distribuzioni#Distribuzione t di Stundet|distribuzione t di Student]] con $n-1$ *gradi di libertà*.

Tale distribuzione assomiglia ad una [[Distribuzioni#Normale|normale]], ma più *"schiacciata"* e quindi con maggiore probabilità nelle code.
Al crescere dei gradi di libertà, tende sempre di più ad una normale standard $N(0,1)$.
Intuitivamente, possiamo pensare ad una distribuzione normale che tiene conto dell'insicurezza introdotta dalla stima $S^2$, che tale insicurezza tende a diminuire al crescere di $n$.

A questo punto, come nel caso della normale, consideriamo il test $$\text{rifiuto } H_0 \iff |T| > c$$ oppure $$\text{rifiuto } H_0 \iff T > c$$ a seconda del tipo di ipotesi.


------------------
# Varianti su differenze di valori attesi
## Differenza tra due valori attesi, campioni indipendenti, varianze note
Consideriamo due campionamenti **indipendenti** $\mathbf{X}_1$ e $\mathbf{X}_2$ rispettivamente di lunghezze $n_1$ ed $n_2$, con medie $\mu_1, \mu_2$ sconosciute e <u>varianze note</u> $\sigma_1^2, \sigma_2^2$.

Supponiamo di voler verificare l'*ipotesi nulla* $$H_0: \mu_1 = \mu_2$$ ovvero di voler verificare se i due campioni hanno la stessa media.

Riformulare l'ipotesi nulla come $$H_0: \mu_1 - \mu_2 = 0$$

 Possiamo  quindi stimare la differenza tra i due parametri ignoti $\mu_1 - \mu_2$, semplicemente tramite la differenza tra le due [[Random Sample#Media campionaria|medie campionarie]] $\overline{X}_1 - \overline{X}_2$.

Prima di procedere, calcoliamo lo **standard error** di questa differenza, partendo dalla varianza
$$\begin{align*}
\text{Var}(\overline{X}_1 - \overline{X}_2)
&= \mathbb{E}\left[ (\overline{X}_1 - \overline{X}_2)^2 \right] - (\mu_1 - \mu_2)^2\\
&= \mathbb{E}\left[ \overline{X}_1^2 \right] + \mathbb{E}\left[ \overline{X}_2^2 \right] -  \cancel{2\mathbb{E}\left[ \overline{X}_1 \overline{X}_2 \right]} - \mu_1^2 - \mu_2^2 + \cancel{2\mu_1\mu_2}\\
&= \left(\mathbb{E}\left[ \overline{X}_1^2 \right] - \mu_1^2\right) + \left(\mathbb{E}\left[ \overline{X}_2^2 \right] - \mu_2^2\right)\\
&= \text{Var}(\overline{X}_1) + \text{Var}(\overline{X}_2)\\
&= \frac{\sigma_1^2}{n_1} + \frac{\sigma_2^2}{n_2}
\end{align*}$$

Importante notare che abbiamo ottenuto questo risultato solamente sfruttando l'**indipendenza** dei due campioni.
L'ulitma disuguaglianza è invece data da [[Random Sample#^2864bf]].

L'errore standard risulterà quindi essere $\sqrt{\sigma_1^2/n_1 + \sigma_2^2/n_2}$.

Definiamo quindi la statistica $$T = \frac{\left(\overline{X}_1 - \overline{X}_2\right) - (\mu_1 - \mu_2)}{\sqrt{\sigma_1^2/n_1 + \sigma_2^2/n_2}} \overbrace{=}^{\text{sotto } H_0} \frac{\overline{X}_1 - \overline{X}_2}{\sqrt{\sigma_1^2/n_1 + \sigma_2^2/n_2}}$$ la quale avrà distribuzione $N(0,1)$.

A questo punto, come prima vogliamo **rifiutare** $H_0$ se $|T| > c$.

Calcoliamo quindi la probabilità di errore tipo I, con la [[#^3d2fb2|fromula precedente]] $$1 - \alpha = \Phi(c) - \Phi(-c)$$

------------------
## Differenza tra due valori attesi, campioni indipendenti, varianze ignote
Come prima, consideriamo due **campioni indipendenti** $\mathbf{X}_1$ e $\mathbf{X}_2$ rispettivamente di lunghezze $n_1$ ed $n_2$, con medie $\mu_1, \mu_2$  e varianze $\sigma_1^2, \sigma_2^2$ **sconosciute**.

Sostituendo $\sigma_1^2$ e $\sigma_2^2$ con le reispetive varianze campionarie $S_1^2$ e $S_2^2$ questa volta non si ottiene che
$$T = \frac{\left(\overline{X}_1 - \overline{X}_2\right) - (\mu_1 - \mu_2)}{\sqrt{S_1^2/n_1 + S_2^2/n_2}}$$
**non** segue una distribuzione $t$ di Student, come nel [[Test comuni#Distribuzione normale varianza e media sconosciute|caso precedente]].

In questo caso possiamo applicare 2 approcci:
1. Se $n_1, n_2$ sono molto grandi (per esempio $\geq 30$ o $\geq 60$ a seconda dei libri) allora possiamo dire che $T$ segue una distribuzione che *approssima* una $t$ di Student con $n_1 + n_2 - 2$ gradi di libertà. Questo perché al crescere della grandezza del campione, l'approssimazione $S^2$ stima sempre meglio $\sigma^2$.
2. se si può supporre che la distribuzione dei dati sia <u>normale</u> e che le due varianze ignote siano comunque <u>uguali</u>, $\sigma_1 = \sigma_2 = \sigma$, allora è possibile trovare una stima di $\sigma$ basata sulle due varianze campionarie $S^2_1$ e $S^2_2$.
   Tale stima è nota come **pooled variance stimate** ed è definita come $$S^2 = \frac{(n_1 - 1)S_1^2 + (n_2 - 1)S_2^2}{(n_1 - 1) + (n_2 - 1)}$$
   In tal modo avremo che $$T = \frac{\left(\overline{X}_1 - \overline{X}_2\right) - (\mu_1 - \mu_2)}{\sqrt{S_1^2/n_1 + S_2^2/n_2}} = \frac{\left(\overline{X}_1 - \overline{X}_2\right) - (\mu_1 - \mu_2)}{S\sqrt{1/n_1 + 1/n_2}}$$ avrà un distribuzione [[Distribuzioni#Distribuzione t di Stundet|t di Student]] con $n_1 + n_2 - 2$ gradi di libertà.


-------------------
# t-test (di Welch)
## Differenza tra due valori attesi, campioni indipendenti, varianze ignote differenti
Poniamoci nel [[#Differenza tra due valori attesi campioni indipendenti varianze ignote|contesto precedente]], e supponiamo di non poter in alcun modo assumere che le due varianze ignote siano uguali.

Esiste un terzo approccio che **approssima** una soluzione, e che è ritenuta essere una buona approssimazione.
Come prima definiamo la statistica $$T = \frac{\overline{X}_1 - \overline{X}_2}{\sqrt{S_1^2/n_1 + S_2^2/n_2}}$$
(Ricorda, stiamo semprsotto ipotesi nulla $H_0: \mu_1 - \mu_2 = 0$)

E possibile **approssimare** $T$ con una distribuzione $t$ di studente con un numero di gradi libertà $\nu$ calcolato con l'approssimazione di Welch-Satterthwaite $$\nu \approx \frac{\left(\frac{S_1^2}{n_1} + \frac{S_2^2}{n_2}\right)^2}{\frac{S_1^4}{n_1^2(n_1 - 1)} + \frac{S_2^4}{n_2^2(n_2 - 1)}}$$

Molti software (per esempio R) preferiscono presentare questo test come alternativa primaria per il confronto di due mediei poiché l'ipotesi di **varianze note** o **variance ignote ma uguali** raramente si presenta.