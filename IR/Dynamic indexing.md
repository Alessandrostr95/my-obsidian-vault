Finora abbiamo assunto di avere collezioni di documenti del tutto statiche o che cambiano raramente.
Molte collezioni di documenti però cambiano frequentemente, con molte operazioni di inserimento, rimozione e modifica dei documenti.
Ciò significa che bisogna aggiungere nuovi termini all'indice, oppure che bisogna modificare posting list già esistenti.

Un primo approccio semplice per l'implementazione di un indice **dinamico** è quello di ricostruire **periodicamente** l'indice da zero.
Tale approccio va bene se abbiamo poche operazioni di modifica della collezione, e se non abbiamo necessità di indicizzare un nuovo documento nel brevissimo termine.

