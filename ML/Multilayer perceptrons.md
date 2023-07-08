Abbiamo visto come nella [[Linear Classification|classificazione]] i modelli sono delle **specializzazioni** del [[Generalized Linear Model]] $$y(\mathbf{x}) = f(\mathbf{w}^T\phi(\mathbf{x}))$$ dove $\mathbf{w}$ è una **trasformazione lineare**, $\phi$ è una [[Some Base Function]] e $f$ è una **funzione non lineare**.

A livello visivo possiamo rappresentare nel seguente modo i modelli visti in precedenza.
- [[Linear Regression]] $y(\mathbf{x}) = \mathbf{w}^T\mathbf{x} + w_0$ 
  ![[ML/img/ML_10_1.png]]
- [[Naive Bayes Classifier#Deriving Posterior distribution $p(C_k vert d)$|Logistic Regression]] $y(\mathbf{x}) = \sigma(\mathbf{w}^T\mathbf{x} + w_0)$
  ![[ML_10_2.png]]
- [[Naive Bayes Classifier#Caso Multiclasse - Softmax Regression|Softmax Regression]] $y_i(\mathbf{x}) = s_i(\mathbf{w}_i^T\mathbf{x} + w_{i0})$ 
  ![[ML/img/ML_10_3.png]] 
  - Applicazione di [[Some Base Function|base function]] a modello lineare
    ![[ML/img/ML_10_4.png]]

Volendo, possiamo **iterare** l'applicazione di base function e di applicazioni lineari
![[ML_10_5.png]]