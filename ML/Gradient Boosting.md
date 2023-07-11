Il **Gradient Boosting** è una tecnica di [[Boosting]], che segue la seguente idea:
1. facciamo il fitting di un [[Adaboost#Additive Model|modello additivo]] $\sum_{j=1}^{m}\alpha_j y_j(x)$.
2. ad ogni stage, ingroduciamo un **weak learner** che compensi le **carenze** di quelli precedenti.
3. le carenze sono identificate da tutti quei punti che hanno avuto un **alto peso**.

