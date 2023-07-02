Un altro approccio non parametri molto importante è la **Gaussian Processes Regression** (**GPR**).
L'idea di base è molto simile alla [[Fully Bayesian Approach]] per la regressione lineare.
Assumiamo di avere come **conoscenza a priori** una **distribuzione** di probabilità su una famiglia infinita di funzioni che descrivono i nostri dati.
Questa conoscenza a priori descrive come dovrebbe comportarsi la funzione target $f$ dato un qualsiasi input $x$, senza però aver fatto alcuna osservazione.
Quando iniziamo ad avere osservazioni (il dataset), invece di un numero infinito di funzioni, consideriamo solamente quelle funzioni che si adattano ai punti dati osservati.
Una volta fatte queste osservazioni, possiamo aggiornare la nostra conoscenza a priori per generare una **distribuzione a posteriori**.
Possiamo poi **iterare** questa procedura ogni volta che abbiamo nuove osservazioni.

