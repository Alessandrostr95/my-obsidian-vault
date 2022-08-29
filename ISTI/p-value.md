# p-value
Un altro metodo per verificare se è corretto accettare o rifiutare una [[Hypothesis Testing|ipotesi]] è il calcolo del **p-value**, noto anche come **significatività osservata**.
In parole semplici, potrebbe capitare di osservare dei dati che inducono al rifiuto dell'*ipotesi nulla* $H_0$.
Il *p-value* aiuta a capire se la differenza tra i dati osservati e l'ipotesi nulla sia dovuta a un puro caso, oppure se in realtà tale differenza è **statisticamente rilevante**, ovvero difficilmente dovuta a un puro caso.

Consideriamo un [[Random Sample#Random Sample|campione]] $X_1, ..., X_n$ preso da una popolazione della quale non conosciamo le media $\mu$, e definiamo l'ipotesi nulla $$H_0: \mu \leq \mu_0$$
Normaliziamo i dati tramite un semplice [[Test comuni#z-test|z-test]], definendo il valore osservato $$T = \frac{\overline{X} - \mu_0}{\sigma/\sqrt{n}}$$ che avrà una distribuzione normale standardizzata $N(0,1)$.

Il p-value è definita come la probabilità di ottenere un risultato "**più estremo**" di quello osservato $T$, sotto l'ipotesi che $H_0$ sia vera.

Definiamo cosa si intende per *"più estremo"*.

Nel nostro esempio ($H_0: \mu \leq \mu_0$) abbiamo un test **unilaterale destro**, ovvero il z-test rifiuta $H_0$ se $T > c$ per un certo $c$ fissato.
Perciò per valori estremi si intedono dati osservati $T$ che vanno verso la **coda destra** di una normale.
Perciò, il p-value in questo caso è definito come $$p(X_1,...,X_n) = P(Z \geq T \;\vert\; H_0) = 1 - \Phi(T) = \Phi(-T)$$ dove $Z \sim N(0,1)$.

```julia
using Distributions, Plots

x = collect(-3:0.01:3)
y = pdf.(Normal(), x)

c = 1.2
i = x .≥ c

T = 0.73
j = x .≥ T

plot(x[j],y[j], fillrange=zero(x[j]), label="p-value", fillstyle=:x, fillcolor=:orange, c=:orange)
plot!(x[i],y[i], fillrange=zero(x[i]), fα=.17, label="α", fillcolor=:blue, c=:blue)
plot!(x,y, lw=2, label=:none, c=:blue)
vline!([c], c=:black, ls=:dash, label="c")
vline!([T], c=:black, ls=:dot, label="T")
```

![](isti_p-value_2.png)

Perciò, dire che $T > c$ equivale a dire che il p-value $p(X_1, ..., X_n) < \alpha$, ovvero la differenza tra i dati osservati e l'ipotesi è **significativamente** rilevante.

> più il *p-value* è piccolo, più la *significatività* è alta poiché il risultato del test ci dice che $H_0$ non spiega adeguatamente i dati osservati.

Perciò il test del *p-value* consiste nel $$\text{rifiutare } H_0 \iff p(X_1, ..., X_n) < \alpha$$

Consideriamo ora il caso simmetricamente opposto, l'**unilaterale sinistro** con $$H_0: \mu \geq \mu_0$$
In questo caso si rifiuta $H_0$ quando $T < c$, bisogna quindi calcolare il p-value per valori inerenti alla **coda sinistra** della normale.
Perciò avremo un p-value definito in maniera opposta a prima, ovvero $$p(X_1, ..., X_n) = P(Z \leq T \; \vert \; H_0) = \Phi(T)$$
Analogamente, rifiutaremo $H_0$ se $p(X_1, ..., X_n) < \alpha$.
```julia
c = -1
i = x .≤ c

T = -1.5
j = x .≤ T

plot(x[j],y[j], fillrange=zero(x[j]), label="p-value", fillstyle=:x, fillcolor=:orange, c=:orange)
plot!(x[i],y[i], fillrange=zero(x[i]), fα=.17, label="α", fillcolor=:blue, c=:blue)
plot!(x,y, lw=2, label=:none, c=:blue)
vline!([c], c=:black, ls=:dash, label="c")
vline!([T], c=:black, ls=:dot, label="T")
```
![](isti_p-value_3.png)
Se prendiamo in esempio la figura in esempio, abbiamo che $p(\mathbf{X}) < \alpha$, ovvero i dati osservati risultano *significativamente differenti* da ci che ci si aspettava se $H_0$ fosse vera, perciò rifiutiamo $H_0$.

Infine consideriamo il caso **bilaterale**, con iptesi nulla del tipo $$H_0: \mu = \mu_0$$
Avevamo [[Test comuni#Distribuzione normale varianza nota media sconosciuta - part 1|visto]] che rifiutavamo $H_0$ quando $\vert T \vert > c$.
In questo caso, dato che rifiutavamo $T$ nelle due code in maniera <u>simmetrica</u>, avevamo $\alpha/2$ di errore sulla coda destra e $\alpha/2$ di errore sulla coda sinistra.
Perciò è significativo rifiutare $H_0$ quando $$p(X_1,...,X_n) < \frac{\alpha}{2}$$

```julia
c = 1.5
i₁, i₂ = x .≥ c, x .≤ -c

T = 0.73
j = x .≥ T

plot(x[j],y[j], fillrange=zero(x[j]), label="p-value", fillstyle=:x, fillcolor=:orange, c=:orange)
plot!(x[i₁],y[i₁], fillrange=zero(x[i₁]), fα=.17, label="right α/2", fillcolor=:blue, c=:blue)
plot!(x[i₂],y[i₂], fillrange=zero(x[i₂]), fα=.2, label="left α/2", fillcolor=:purple, c=:blue)
plot!(x,y, lw=2, label=:none, c=:blue)
vline!([c, -c], c=:black, ls=:dash, label="c, -c")
vline!([T], c=:black, ls=:dot, label="T")
```
![](isti_p-value_4.png)
Nella figura in esempio accetteremo $H_0$ perché la differenza tra il valore atteso e dati osservati non è abbastanza significativa.