Nel problema della **classificazione** le etichette degli elementi del dominio $\mathcal{X}$ hanno valori **discreti**, a differenza della [[Linear Regression|regressione]] in cui possono avere un qualsiasi valore reale.

Il caso più semplice è la **classificazione binaria**, in cui ogni elemento $x$ può appartenere ad una delle **classi** $C_0$ o $C_1$.
Una codifica, in questo caso, può essere un singolo valore numerico $t \in \lbrace 0,1 \rbrace$, dove $t = 0$ se $x \in C_0$ e $t = 1$ se $x \in C_1$.

Nel caso più generale in cui abbiamo $K > 2$ classi, è preferibile usare la codifica **one-hot**, ovvero $t \in \lbrace 0, 1\rbrace^K$ è un vettore con $K-1$ zeri ed un solo $1$.
Se $x \in C_i$ allora calle con $1$ è quella in posizione $i$.

$C_i$ | $t$
---|---
1 | 0001
2 | 0010
3 | 0100
4 | 1000

