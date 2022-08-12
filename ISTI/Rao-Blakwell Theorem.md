# Rao-Blakwell Theorem
Sia un [[Random Sample#Random Sample|campionamento]] $\mathbf{X} = (X_1, ..., X_n)$ dipendente da un parametro $\theta$, sia $T(\mathbf{X})$ una **[[Statistiche Sufficienti#Statistica Sufficiente|statistica sufficiente]]** per $\theta$, e sia $W(\mathbf{X})$ uno **[[Stimatori Puntuali#^401481|stimatore non distorto]]** per $\tau(\theta)$.

Definiamo la funzione $$\phi(t) = \mathbb{E}\left[ W(\mathbf{X}) \vert \;t\; \right]$$

Allora avremo che
1. $$\mathbb{E}_{\theta}\left[ \phi(T(\mathbf{X})) \right] = \tau(\theta)$$ ^75479a
2. $$\text{Var}_\theta\big(\phi(T(\mathbf{X}))\big) \leq \text{Var}_\theta(W(\mathbf{X}))$$ ^57e6d5

Ovvero $\phi(T(\mathbf{X}))$ è il **miglior** stimatore non distorto di $\tau(\theta)$.

## Proof
Prima di iniziare è utile ricordare che per ogni coppia di v.a. $X,Y$ si ha che
- $\mathbb{E}\left[ \mathbb{E}\left[ X \vert Y \right]\right] = \mathbb{E}\left[ X \right]$
- $\text{Var}(X) = \text{Var}(\mathbb{E}\left[ X \vert Y \right]) + \mathbb{E}\left[\text{Var}( X \vert Y) \right]$

Il [[#^75479a|punto 1]] è banalmente verificabile $$\mathbb{E}_{\theta}\left[ \phi(T(\mathbf{X})) \right] = \mathbb{E}_{\theta}\left[ \mathbb{E}\left[ W(\mathbf{X}) \vert T(\mathbf{X}) \right] \right] = \mathbb{E}_\theta\left[ W(\mathbf{X}) \right] = \tau(\theta)$$ perciò $\phi(T(\mathbf{X}))$ è **non distorto** per $\tau(\theta)$.

Invece per il [[#^57e6d5|punto 2]] abbiamo che
$$\begin{align*}
\text{Var}_\theta(W(\mathbf{X}))
&= \text{Var}_\theta(\mathbb{E}\left[ W(\mathbf{X}) \vert T(\mathbf{X}) \right]) + \mathbb{E}_\theta\left[ \text{Var}(W(\mathbf{X}) \vert T(\mathbf{X})) \right]\\
&= \text{Var}_\theta\big(\phi(T(\mathbf{X}))\big) + \mathbb{E}_\theta\left[ \text{Var}(W(\mathbf{X}) \vert T(\mathbf{X})) \right]\\
&\geq \text{Var}_\theta\big(\phi(T(\mathbf{X}))\big)
\end{align*}$$
Perciò abbiamo che $\phi(T(\mathbf{X}))$ è **uniformemente** migliore di $W(\mathbf{X})$.

Ora manca solo dimostrare che $\phi(T(\mathbf{X}))$ è uno stimatore.
Basta osservare che $\phi(t) = \mathbb{E}\left[ W(\mathbf{X}) \vert \;t\; \right]$ è una funzione dei soli parametri $X_1, ..., X_n$, perciò è *indipendente* da $\theta$, perciò è uno stimatore $\square$.

-------------------
# Conclusioni
Il teorema di Rao-Blakwell in sintesi ci dice che è inutile cercare stimatori che non siano funzioni della [[Statistiche Sufficienti#Def Statistica Sufficiente Minimale|statistica sufficiente minimale]].