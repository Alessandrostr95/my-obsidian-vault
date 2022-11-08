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

