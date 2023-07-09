L'approccio visto [[Support Vector Machines|sotto l'assunzione di separabilità]] non prevede soluzioni ammissibili nel caso in cui il dataset non sia linearmente separabile.
Infatti, in tal caso, non è possibile trovare una soluzione che soddisfi tutti i vincoli $$t_i(\mathbf{w} \cdot \phi(\mathbf{x}_i) + w_0) \geq 1$$

Perciò, per ottenere un separatore, è necessario **rilassare** questi vincoli, aggiungendo però un **errore** alla funzione da minimizzare.

Introduciamo delle **variabili di slack** $\xi_i$ per ogni vincolo, per fornire una misura di quanto il vincolo non è verificato. ^4c4a92

Definiamo quindi una nuova versione del problema di minimizzazione
$$\begin{align}
\min_{\mathbf{w}, \pmb\xi} &\;\;\frac{1}{2}\Vert \mathbf{w} \Vert^{2} + C \sum_{i=1}^{n}\xi_i\\
s.t. &\;\;\gamma_i = t_i(\mathbf{w}\cdot\mathbf{x}_i + w_0) \geq 1 - \xi_i&\forall i = 1, ..., n
\end{align}$$

Applicando un opportuno [[Support Vector Machines#Lagrangiano|moltiplicatore lagrangiano]] avremo il seguente problema duale
$$\max_{\pmb\lambda \,:\, 0 \leq \lambda_i \leq C} L(\pmb\lambda) = \max_{\pmb\lambda \,:\, 0 \leq \lambda_i \leq C} \left(\sum_{i=1}^{n}\lambda_i - \frac{1}{2}\sum_{i=1}^{n}\sum_{j=1}^{n}\lambda_i\lambda_jt_it_j(\mathbf{x}_i \cdot \mathbf{x}_j)\right)$$
Osservare che l'unica differenza tra il [[Support Vector Machines#^c9f4f7|duale nel caso di separabilità lineare]], sta nel fatto che $0 \leq \lambda_i \leq C$ e non semplicemente $\lambda_i \geq 0$.


## Item characterization
Sia $\mathbf{x}_i$ un qualsiasi elemento del dataset.
Allora abbiamo le seguenti condizioni:
1. se $\phi(\mathbf{x}_i)$ è posizionato nella metà corretta, al di fuori del margine, allora avremo che $\xi_i = 0, \lambda_i = 0$.
2. se $\phi(\mathbf{x}_i)$ è posizionato nella metà corretta, ma esattamente sul margine (ovvero $\phi(\mathbf{x}_i)$ è un vettore di supporto), allora avremo che $\xi_i = 0, 0 \leq \lambda_i \leq C$.
3. se $\phi(\mathbf{x}_i)$ è posizionato nella metà corretta, ma dentro il margine, allora avremo che $0 < \xi_i < 1, \lambda_i = C$.
4. se $\phi(\mathbf{x}_i)$ è posizionato esattamente sull'iperpiano di separazione allora avremo che $\xi_i = 1, \lambda_i = C$.
5. se $\phi(\mathbf{x}_i)$ è posizionato nella metà sbagliata allora avremo che $\xi_i > 1, \lambda_i = C$.

![[ML/img/ML_11_3.png]]