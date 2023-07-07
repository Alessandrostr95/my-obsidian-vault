## Introduzione #todo

-----
# Approccio non parametrico
Nell'approccio non parametrico si cerca di derivare la probabilità $p(C_k \vert x)$ **direttamente dai dati**, senza stimare alcun parametro.

## Histogram
Questo è uno degli approcci più semplici.
Partizioniamo il **dominio** $\mathcal{X}$ in $m$ **intervalli** $m$-dimensionali tutti di dimensione $\Delta$, detti **bins** o **bucket**.
Dato un qualsiasi elemento $x$ e il bin a cui appartiene $B(x)$, la probabilità $P_x$ di campionare dal trainig set $\mathbf{X}$ un elemento dal bin $B(x)$, sarà $$P_x = \frac{\vert B(x) \vert}{\vert \mathbf{X} \vert} = \frac{n(x)}{n}$$ dove $n(x) = \vert B(x) \vert$.
Questa probabilità è **discreta**, se invece volessimo stimare una **densità** per un qualsiasi punto continuo, allora possiamo farlo nel seguente modo
$$f_\mathbf{x}(x) = \frac{P_x}{\Delta}$$

![[ML/img/ML_09_1.png]]

## Kernel Density Estimators


