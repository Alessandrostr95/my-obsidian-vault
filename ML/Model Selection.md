Abbiamo visto che nella [[Linear Regression|regressione]] la scelta del grado $m$ del polinomio determina l'**espressività** del nostro **modello**.
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

# Limitare la complessità del Modello - Ridge Regression
Quindi quello che si vuole è trovare un valore abbastanza alto di $m$ senza però perdere di generalità del modello.

Un possibile approccio è quello di **regolare** la [[Prediction Risk#^9cd1a0|funzione di costo]], riscrivendola come una funzione del tipo $$E(\mathbf{w}) = E_D(\mathbf{w}) + \lambda E_W(\mathbf{w})$$ dove $E_D(\mathbf{w})$ dipende dai parametri e dal dataset, mentre $E_W(\mathbf{w})$ dipende dai **soli parametri**.

Il coefficiente $\lambda \geq 0$ indica l'**importanza** che si vuole dare ai due valori, ed è detto **coefficiente di regolarizzazione**. ^49f3ad

Una possibile funzione $E_W(\mathbf{w})$ è $$E_W(\mathbf{w}) = \frac{1}{2} \mathbf{w}^T\mathbf{w} = \frac{1}{2}\sum_{j=0}^{m} w_j^2 = \frac{1}{2}\Vert \mathbf{w} \Vert^2$$

Se per esempio usiamo la [[Some Loss Functions#Quadratic Loss|quadratic loss]] per $E_D(\mathbf{w})$ otterremo una funzione di costo del tipo $$E(\mathbf{w}) = \frac{1}{2}\sum_{i=1}^{n}(t_i - \mathbf{w}^T\Phi(\mathbf{x_i}))^2 + \frac{\lambda}{2}\mathbf{w}^T\mathbf{w} = \frac{1}{2}(\mathbf{\Phi w } - \mathbf{t})^T(\mathbf{\Phi w } - \mathbf{t}) + \frac{\lambda}{2}\mathbf{w}^T\mathbf{w}$$ con soluzione ottima $$\mathbf{w}^* = (\lambda \mathbf{I} + \mathbf{\Phi}^T\mathbf{\Phi})^{-1}\mathbf{\Phi}^T \mathbf{t}$$

Questo tipo di regressione è anche nota come **Ridge Regression**.

> [!tldr] Lasso
> Una versione **generalizzata** di $E_W(\mathbf{w})$ è la seguente $$E_W(\mathbf{w}) = \frac{1}{2}\sum_{j=0}^{m}\vert w_j \vert^q$$
> Quando $q=1$, tale funzione è denotata come **lasso**. In questo caso sono favoriti **modelli sparsi**, ovvero con molti parametri posti a 0.
> 

L'idea della regolarizzazione è quella che quando ci sono molti parametri è facile andare in overfitting sul training set, perciò si penalizzano soluzioni con eccessivi parametri.
Molti di essi infatti tenderanno a 0.

Quindi abbiamo che la qualità della soluzione dipenderà dall'**iper-parametri** $\lambda$.

![[ML/img/ML_04_8.png]]

Ci si aspetta quindi che:
- per valori troppo **piccoli** di $\lambda$ c'è troppa dipendenza dai dati, e quindi si tende all'overfitting.
- per valori troppo **grandi** di $\lambda$ si è troppo **indipendenti** dai dati, tendo quindi all'**underfitting**.

![[ML/img/ML_04_9.png]] ^20d9ca

Infatti come si può vedere dalla [[#^20d9ca|figura]], il modello è **poco generale** sia per valori troppo piccoli che troppo grandi di $\lambda$ (fissato un $m$).

## [[Esempio utilizzo Regolarizzazione]]