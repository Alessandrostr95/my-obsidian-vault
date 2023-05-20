La scelta del grado $m$ del polinomio determina l'**espressività** del nostro **modello**.
Infatti mentre con $m = 1$ abbiamo a disposizione la famiglia di tutte le **rette**, con $m =3$ abbiamo a disposizione tutti i polinomi di grado 3, inclusi anche le parabole e tutte le rette. ^cd025e

Ovviamente con $m$ grande si può stimare meglio la funzione reale che si desidera approssimare, però quando $m$ è molto grande si può incorrere in due problemi:
1. $m$ troppo grande implica un numero molto grande di parametri da apprendere, e quindi un costo eccessivo in termini di computazionali
2. quando $m$ è troppo grande rispetto alla dimensione $n$ del dataset $\mathcal{T}$ si rischia l'[[From Learning to Optimization#Problemi con $ mathcal{H}$ grande|overfitting]]. 

![[ML/img/ML_04_5.png]]


# Valutazione della generalizzazione
Supponiamo di generare un **test set** $\mathbf{X}_{\text{test}}$ di per esempio 100 nuovi elementi.

Un modo per selezionare un buon modello, ovvero un [[#^cd025e|buon valore di m]] è il seguente.

Per ogni valore di $m$:
1. deriviamo il miglior valore dei parametri $\mathbf{w}^*$ dal **training set** $\mathcal{T}$
2. computare l'[[Prediction Risk#^350fae|errore]] $E(\mathbf{w}^*, \mathbf{X}_{\text{test}})$ rispetto al **test set**, ovvero $$E(\mathbf{w}^*, \mathbf{X}_{\text{test}}) = \frac{1}{2} \sum_{\mathbf{x} \in \mathbf{X}_{\text{test}}} (y(\mathbf{x}, \mathbf{w}^*) - t)^2$$

Minori sono i valori di questo errore rispetto al test set, maggiore è il **livello di generalizzazione** del modello.

```ad-info
In alternativa, si utilizza la radice quadrata dell'errore medio, detto $E_{RMS}$.
$$E_{RMS}(\mathbf{w}^*, \mathbf{X}_{\text{test}}) = \sqrt{\frac{E(\mathbf{w}^*, \mathbf{X}_{\text{test}})}{\vert \mathbf{X}_{\text{test}} \vert}}$$
Tanto per monotonia, le stime non cambiano.
```

![[ML/img/ML_04_6.png]]

Osservare che al crescere di $m$ l'errore tende a decrescere, in quanto si hanno modelli più espressivi.
Però, da un certo punto in poi, quando $m$ diventa eccessivamente grande, l'errore sul training set tende a 0 (in quanto si va in [[From Learning to Optimization#Problemi con $ mathcal{H}$ grande|overfitting]]) mentre l'errore rispetto al test set peggiora drasticamente.
Ciò significa che il modello è troppo [[From Learning to Optimization#Bias vs Varianza|dipendente dai dati]] e quindi poco **generalizzato**.

Il **bias** rispetto ai dati non dipende solo dalla complessità del modello, bensì anche dalla **dimensione del dataset**.
Infatti, con dataset molto grandi è più difficile avere overfitting, quindi consentono di addestrare modelli **più complessi** (con valori di $m$ più grandi).

![[ML/img/ML_04_7.png]]
