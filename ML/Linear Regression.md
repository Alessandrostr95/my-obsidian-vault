Supponiamo di avere un dataset $\mathcal{T} = (X,t) = \lbrace (x_1,t_1), ..., (x_n, t_n) \rbrace \subseteq \mathbb{R^d} \times \mathbb{R}$.
Ovvero ad ogni vettore $x_i \in \mathbb{R}^d$ è associato un valore reale (il **target**) $t_i \in \mathbb{R}$.

Supponiamo che i valori target seguano una **funzione** (*sconosciuta*), per esempio $\sin(2\pi x)$, più un **rumore** distribuito come $\varepsilon \sim \mathcal{N}(0, \sigma^2)$. 
Quindi $$t_i = \sin(2\pi x_i) + \varepsilon_i$$
![[ML_04_2.png]]