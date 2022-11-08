[[NLP/Lecture 7|vedi]] $\leftarrow$

Abbiamo visto in [[NLP/Lecture 7]] il nostro **stimatore** della distribuzione dei tag
$$P(t_1^n \vert w_1^n)\propto \prod_{i=1}^{n} \hat{P}(t_i \vert t_{i+1}) \hat{P}(w_i \vert t_i)$$

Vogliamo ora trovare il **vettore di tag** che **massimizza** tale probabilità
$$\arg \max_{t_1^n \in T^n} P(t_1^n \vert w_1^n) \propto \arg \max_{t_1^n \in T^n} \prod_{i=1}^{n} \hat{P}(t_i \vert t_{i+1}) \hat{P}(w_i \vert t_i)$$

Posso modellizzare il problema con un markov chain, dove ogni abbiamo $n$ **livelli** (uno per ogni parola).
In goni livello ci sono i nodi che rappresentano tutti i possibili **tag**.
Le transazioni sono **dense** tra i livelli, e le probabilità sono esattamente $\hat{P}(t_i \vert t_{i+1}) \hat{P}(w_i \vert t_i)$.
Alla fine il valore di un cammino è esattamente il prodotto delle transazioni.

[Algoritmo di Viterbi] -> algoritmo non ottimale, perché trova i massimi locali.


-----
## sfot-max function
$$\text{soft-max}(z) = \left( \frac{e^{z_i}}{\sum_{j=1}^{n} e^{z_j}} \right)_{i=1,...,n}$$

Innanzitutto, essendo la funzione **monotona**, il massimo di $\text{soft-max}(z)$ equivale al massimno di $z$.

La proprietà di tale funzione è che i valori massimi tendono ad 1, e gli altri a 0 (per via della crescita esponenziale).
Perciò sarà un vettore di elementi tutti quasi pari a 0 e tutti quasi par a 1.

```ad-attention
La funzione non funziona bene quando tutti i valori sono compresi tra 0 e 1 (avrà valori di circa 0.5).
```


Per estrarre il massimo di $z$ (tramite i soli operatori **vettoriali**) basta fare $$\max{(z)} \approx z \text{ .* } \text{soft-max}(z)$$

Creo quindi una matrice `TAG x PAROLE`, dove ad ogni colonna $i$ ci sta la distribuzione dei tag della parola $i$-esima.

Per prendere la colla della parola `maria`, moltiplico la matrice $M$ per il vettore **one-hot** $w$ dove ci sta un 1 nell'indice `maria`, e 0 in tutte le altre posizioni.

Infine per trovare il tag più probabile per la parola `maria`, applico come prima la softmax.


