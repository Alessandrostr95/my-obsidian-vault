L'approccio visto [[Support Vector Machines|sotto l'assunzione di separabilità]] non prevede soluzioni ammissibili nel caso in cui il dataset non sia linearmente separabile.
Infatti, in tal caso, non è possibile trovare una soluzione che soddisfi tutti i vincoli $$t_i(\mathbf{w} \cdot \phi(\mathbf{x}_i) + w_0) \geq 1$$

Perciò, per ottenere un separatore, è necessario **rilassare** questi vincoli, aggiungendo però un **errore** alla funzione da minimizzare.

Introduciamo delle **variabili di slack** $\xi_i$ per ogni vincolo, per fornire una misura di quanto il vincolo non è verificato.

Definiamo quindi una nuova versione del problema di minimizzazione
$$\begin{align}
\min_{\mathbf{w}, \pmb\xi} &\;\;\frac{1}{2}\Vert \mathbf{w} \Vert^{2} + C \sum_{i=1}^{n}\xi_i\\
s.t. &\;\;\gamma_i = t_i(\mathbf{w}\cdot\mathbf{x}_i + w_0) \geq 1 - \xi_i&\forall i = 1, ..., n
\end{align}$$

Applicando un opportuno [[Support Vector Machines#Lagrangiano|moltiplicatore lagrangiano]] avremo il seguente problema duale
$$\max_{\pmb\lambda \,:\, 0 \leq \lambda_i \leq C} L(\pmb\lambda) = \max_{\pmb\lambda \,:\, 0 \leq \lambda_i \leq C} \left(\sum_{i=1}^{n}\lambda_i - \frac{1}{2}\sum_{i=1}^{n}\sum_{j=1}^{n}\lambda_i\lambda_jt_it_j(\mathbf{x}_i \cdot \mathbf{x}_j)\right)$$


