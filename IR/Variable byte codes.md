Un buon compromesso tra spazio impiegato e efficienza di operazioni è la  **variable byte code**.

Invece di rappresentare un numero con una **sequenza variabile di bit** possiamo rappresentarlo come una **sequenza variablie di byte**.

Sappiamo che 1 byte è composto da 8 bits.
Di questi 8 ne uso 7 bit per la codifica del numero (o di parte di esso) e con l'ottavo bit indico se continuare a codificare considerando il byte successivo, o se fermarmi nella codifica.
Generalmente:
- se vale 0 continuo a interpretare il numero col prossimo byte.
- se vale 1 ho finito di interpretare il numero nel byte corrente.

Esempio:
**docIDs** | 824 | 829 | 215406
---|---|---|---
**gaps** | 824 | 5 | 214577
**VB code** | 00000110 10111000 | 10000101 | 00001101 00001100 10110001