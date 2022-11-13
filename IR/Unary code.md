Se $G$ è il valore **medio** dei gap, allora l'ideale sarebbe usare in **media** $\sim\log_2{G}$ bits.

Una prima codifica banale per rappresentare i [[Gap encoding|gap]] è quella di usare la **codifica unaria**.
Ovvero per rappresentare un generico intero positivo $n$ uso una serie di $n$ $1$, seguiti da uno $0$ che servità da **simbolo separatore**.

Finché abbiamo termini molto frequenti, i saranno molti gap piccoli, perciò come codifica è molto buona.

Consideriamo invece un termine con seguente posting list $\left[ 1, 10.001 \right]$.
La relativa codifica in [[Gap encoding|gap encoding]] sarà quindi $\left[ 1, 10.000 \right]$.
In codifica unaria saranno necessari per forza $10.003$ bits (2 bit per il primo numero, e 10.001 per il secondo).

