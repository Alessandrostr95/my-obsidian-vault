Il **clustering gerarchico** consiste nel partire da una partizione dove ogni elemento rappresenta un cluster, ovvero dove abbiamo $n$ **signleton**.
Interativamente **aggregare** le sottopartizioni in partizioni più grandi, ottenendo in fine un **albero** dove ogni nodo rappresenta una partizione e le foglie gli elementi che ne fanno parte.

Mentre per il [[Clustering Partizionale - K-mean clustering|k-mean]] avevamo bisogno di:
1. un'assegnazione iniziale degli elementi
2. un valore $K$
3. una misura di **distanza**
nel clustering gerarchico abbiamo solamente bisogno di una **funzione di separazione** tra insiemi, la quale si vuole **massimizzare**.

L'algoritmo è quindi molto semplice:

> 1. definisci $n$ *singleton*, uno per ogni elemento.
>2. ripeti finché non avremo ottenuto un albero:
>	1. calcola tutte le distanza tra i **cluster attuali**.
>	2. unisci i due cluster più **vicini**.


![[ML/img/ML_14_5.png]]


Abbiamo differenti misure di **similarità** tra clusters:
1. **Single Linkage**: ovvero si considerano la coppia di elementi più vicini $$d(C_1, C_2) = \min_{\mathbf{x}_1 \in C_1, \mathbf{x}_2 \in C_2} d(\mathbf{x}_1, \mathbf{x}_2)$$
1. **Complete Linkage**: ovvero si considerano la coppia di elementi più lontani $$d(C_1, C_2) = \max_{\mathbf{x}_1 \in C_1, \mathbf{x}_2 \in C_2} d(\mathbf{x}_1, \mathbf{x}_2)$$
1. **Gorup Avarage**: ovvero si considerano le distanze medie $$d(C_1, C_2) = \frac{1}{\vert C_1 \vert \cdot \vert C_2 \vert}\sum_{\mathbf{x}_1 \in C_1}\sum_{\mathbf{x}_2 \in C_2} d(\mathbf{x}_1, \mathbf{x}_2)$$
