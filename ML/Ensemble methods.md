Fin ora abbiamo utilizzato un approccio che consisteva nel **addestrare** un singolo modelli, cercando di ridurre una [[Prediction Risk|misura di errore]].

Un approccio differente potrebbe essere quello di **combinare** in qualche maniera più modelli differenti, per cercare di avere migliori prestazioni.
Gli approcci possono essere due:
1. **Bagging**: si addestra un insieme di modelli, e per fare la predizione si fa una **media pesata** delle predizioni, pesandole in base alla qualità dei singoli.
2. **Boosting**: questo è un approccio iterativo, in cui la funzione di errore utilizzata per addestrare un modello è opportunamente modificata in base alla qualità del modello addestrato in precedenza: in pratica si cerca di "*aggiustare il tiro*".

-----
# [[Bagging]]
# [[Boosting]]