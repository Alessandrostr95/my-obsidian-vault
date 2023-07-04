L'idea della **Linear Discriminant Analysis** (**LDA**) è quella di cercare una **proiezione lineare** degli elementi del dataset $\mathbf{X} \in \mathbb{R}^D$ in un **sottospazio** che sia il più **linearmente separabile possibile**.

L'approccio più base è fornito dal **FIsher Linear Discriminant** (**Discriminante lineare di Fisher**) in cui gli elementi del dataset vengono proiettati su una **retta** (o spazio di dimensione 1) per mezzo di una **trasformazione lineare** del tipo $$y = \mathbf{w} \cdot \mathbf{x} = \mathbf{w}^T \mathbf{x}$$ dove $\mathbf{w}$ è un vettore $D$-dimensionale che corrisponde alla **direzione** della proiezione.

![[ML/img/ML_06_2.png]]

![[ML/img/ML_06_3.png]]

Per esempio con due classi $C_0, C_1$, fissiamo una certa soglia $\tilde{y}$ e assegnamo ogni punto $\mathbf{x}$ alla classe $C_0$ se e solo se la sua proiezione supera la soglia $$\mathbf{x} \in C_0 \iff \mathbf{w}^T \mathbf{x}= y > \tilde{y}$$ altrimenti lo assegnamo alla classe $C_1$.



