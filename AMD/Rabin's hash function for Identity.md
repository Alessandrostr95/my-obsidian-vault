---
date: 2024-02-18
---
Sia $\Sigma = \left[0,M-1\right]$ un *alfabeto* di $M$ simboli.
Osserviamo che possiamo vedere una qualsiasi *stringa* $x \in \Sigma^n$ di $n$ simboli come un **numero** di $n$ cifre in base $M$.
**Rabin’s hashing** è una [[Hashing|funzione hash]] $\phi$ su stringhe con le seguenti proprietà:
1. data **la sola firma** $\phi(x)$ di una stringa $x = x_1x_2\dots x_n$ è possibile ricavare la firma $\phi(x \circ x_{n+1})$ della concatenazione $x \circ x_{n+1} = x_1x_2 \dots x_n x_{n+1}$ .
2. date **le sole firme** $\phi(x_1\dots x_n)$ e $\phi(x_1 \dots x_i)$ (con $i < n$) è possibile calcolare la firma $\phi(x_{i+1} \dots x_n)$.
Il Rabin's hashing è anche detto **sliding fingerprint**.
Sia $q$ un generico **numero primo** e $z \in \left[ 0,q\right)$ scelto uniformemente a caso.
L'hashing di Rabin è definito come 
$$\kappa_{q,z}(x) = \left( \sum_{i=1}^{n}x_{i}\cdot z^{i}\right) \mod q$$
In altre parole $\kappa_{q,z}(x)$ è un polinomio di grado $n$, con coefficienti $x_1,...,x_n$, valutato in $z$ e il tutto modulo $q$.

Osserviamo che questo hash richiede solamente $\Theta(\log{q})$ bit di spazio.

> [!lemma] Proprietà 1
> $\kappa_{q,z}(x \circ x_{n+1}) = (\kappa_{q,z}(x) + x_{n+1}\cdot z^{n+1}) \mod q$

> [!lemma] Proprietà 1 (Generalizzata)
> Siano $x,y$ due stringhe.
> Allora $$\kappa_{q,z}(x \circ y) = (\kappa_{q,z}(x) + \kappa_{q,z}(y)^{|x|}) \mod q$$

> [!lemma] Porprietà 2
> Sia la string $x_1 \dots x_n$ e la sua sottostringa $x_1 \dots x_i$, con $i < n$.
> Allora
> $$\begin{align}
\frac{\kappa_{q,z}(x_1\dots x_n) - \kappa_{q,z}(x_1\dots x_i)}{z^i}
&= \frac{x_{i+1}z^{i+1} + x_{i+2}z^{i+2} +  \dots + x_nz^n}{z^i}\\
&= x_iz + x_{i+1}z^2 + \dots x_nz^{n-i} = \phi(x_{i+1} \dots x_n)
> \end{align}$$

> [!theorem]
> Siano due stringhe $x \neq y$, con $\max\{ |x|, |y|\} = n$.
> Allora $$P(\kappa_{q,z}(x) =\kappa_{q,z}(y)) \leq \frac{n}{q}$$

`\begin{proof}`
Dato che siamo in $\mathbb{Z}_q$ allora abbiamo che $$\kappa_{q,z}(x) =\kappa_{q,z}(y) \iff \kappa_{q,z}(x) - \kappa_{q,z}(y) \equiv_q 0$$
Indichiamo con $(x-y)$ la stringa tale che $(x-y)_i = (x_i - y_i) \mod q$ (aggiungiamo degli $0$ alla stringa più corta in modo che le due stringhe abbiamo dimensione uguale).
È facie osservare che $$(\kappa_{q,z}(x) - \kappa_{q,z}(y) \mod q) = \kappa_{q,z}(x-y)$$
Perciò siamo interessati a calcolare la probabilità di $P(\kappa_{q,z}(x-y) \equiv_q 0)$.

Dato che $x \neq y$ allora $\kappa_{q,r}(x-y)$ è un polinomio di grado **al più** $n$ in $\mathbb{Z}_q$, valutato in $z$.
Ogni polinomio di grado $n$ ad una variabile ha al più $n$ **radici** se definita su di un **campo**.
Dato che $q$ è primo allora $\mathbb{Z}_q$ è un campo, perciò esistono al più $n$ valori di $z$ che sono radici di $\kappa_{q,r}(x-y)$.
In conclusione, dato che campioniamo $z$ in maniera **uniforme** su $\left[ 0,q \right)$ allora $$P(\kappa_{q,z}(x-y) \equiv_q 0) \leq \frac{n}{q}$$
`\end{proof}`

> [!corollary]
> Data una qualsiasi costante $c$, scegliamo un primo $n^{c+1} \leq q \leq 2n^{c+1}$.
> Allora $\kappa_{q,z}(x)$ userà $\Theta(\log{n})$ bit di spazio e per ogni coppia di stringhe differenti $x \neq y$ avremo che $$P(\kappa_{q,z}(x) = \kappa_{q,z}(y)) \leq n^{-c}$$