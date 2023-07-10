Un **albero di decision** (o **decision tree**) è un classificatore che **ricorsivamente** partiziona lo spazio delle features.
Ogni nodo del decision tree rappresenta una **regione** dello spazio.
Ogni nodo interno ha quindi due figli, i quali rappresentano la **pratizione** della regione del nodo padre.
Alla fine, le foglie rappresentano una **partizione** di tutto lo spazio, e a ciascuna di essere è associata una classe.
Perciò per fare la classificazione di una qualsiasi input $x$, basta percorrere l'albero dalla radice e vedere a quale foglia appartiene.

## Example
![[ML/img/ML_12_1.png]]

![[ML/img/ML_12_2.png]]

![[ML/img/ML_12_3.png]]

![[ML/img/ML_12_4.png]]

![[ML/img/ML_12_5.png]]

![[ML/img/ML_12_6.png]]

![[ML/img/ML_12_7.png]]



