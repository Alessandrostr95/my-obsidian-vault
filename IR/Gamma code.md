Iniziamo con l'osservare che non possiamo rappresentare direttamente in binario una sequenza di numeri, altrimenti non sappiamo quando finisce la codifica di un numero e quando inizia quella di un altro.

Possiamo però sfruttare la [[Unary code|codifica unaria]] per rappresentare quanti bit sono necessari per la codifca del prossimo numero.

Consideriamo come esempio i numeri $9$ e $16$.
In binario essi sono codificati rispettivamente in $1001$ e $10000$.
Perciò la rappresentazine di tale sequenza sarà $$\overbrace{\underbrace{11110}_{\text{length}} \; \underbrace{1001}_{\text{binary code}}}^{9} \;\; \overbrace{\underbrace{111110}_{\text{length}} \; \underbrace{10000}_{\text{binary code}}}^{16}$$

Possiamo risparmiare un ulteriore bit, osservando il fatto che alla fine ogni numero (eccetto lo 0, ma tano non ci interessa) inizia **sempre** per 1.