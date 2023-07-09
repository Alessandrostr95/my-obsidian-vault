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
![[ML/img/ML_10_5.png]]


# Multilayer Network Structure
Possiamo quindi vedere un modello di regressione o classificazione (nel caso più generale) come una sequenza di **trasformazioni lineari** (da apprendere) ai quali poi sono applicate delle **funzioni non lineari**.

A livello visivo, possiamo vedere una applicazione lineare come un **layer** di [[Perceptron|percettroni]].
![[ML/img/ML_10_6.png]]

## First Layer
Il primo layer è quello che ha in input l'elemento $\mathbf{x}$ dal nostro dominio $\mathbb{R}^d$.
Nel layer abbiamo $m_1$ [[Perceptron|percettroni]].

Per prima cosa abbiamo $m_1$ **applicazioni lineari** $a^{(1)}_1, ..., a^{(1)}_{m_1}$ dette anche **attivazioni**.
$$a^{(1)}_j = \sum_{i=1}^{d}w^{(1)}_{ji}x_i + w^{(2)}_{j0} = \mathbf{w}^{(1)}_j  \cdot \mathbf{x}$$
```ad-note
Il punto $\mathbf{x}$ è composto da $d$ elementi (x_1, ..., x_d).
Per semplicità di scrittura, per non scrivere $\mathbf{w}\mathbf{x} + w_0$ assumiamo che in realtà il punto $\mathbf{x}$ sia di dimensione $d+1$ con un 1 in test, ovvero $(1,x_1, ..., x_d)$.
Così possiamo inglobare $w_0$ dentro $\mathbf{w}$ e scrivere in forma compatta $\mathbf{w}^T \mathbf{x}$.
```

Per ogni attivazione $a^{(1)}_j$ applichiamo una trasformazione non lineare $h_1$ detta anche **funzione di attivazione**.
Indichiamo con $\mathbf{z}^{(1)} = (z^{(1)}_1, ..., z^{(1)}_{m_1})$ il vettore che indica l'output di queste funzioni di attivazione.
$$z^{(1)}_j = h_1(a^{(1)}) = h_1(\mathbf{w}^{(1)}_j \cdot \mathbf{x})$$
Osserviamo che tutto questo corrisponde all'applicazione di $m_1$ **generalized linear model** sull'input $\mathbf{x}$. ^4cc835

![[ML/img/ML_10_7.png]]

## Inner Layers
Il [[#^4cc835|vettore]] $\mathbf{z}^{(1)}$ può essere poi visto come l'input di un nuovo layer, detto **inner layer**.
Questo nuovo layer a sua volta applicherà $m_2$ generalized linear model al vettore in input $\mathbf{z}^{(1)}$.

Quindi come prima, abbiamo  $a^{(1)}_1, ..., a^{(2)}_{m_2}$ applicazioni lineari della forma $$a^{(2)}_j = \sum_{i=1}^{m_1}w^{(2)}_{ji}x_i + w^{(2)}_{j0} = \mathbf{w}^{(2)}_j  \cdot \mathbf{z}^{(1)}$$ e poi l'applicazione di una funzione non lineare $h_2$.
Il risultato sarà un nuovo vettore $\mathbf{z}^{(2)} = (z^{(2)}_1, ...,z^{(2)}_{m_2})$ dove $$z^{(2)}_j = h_2(a^{(2)}_j) = h_2(\mathbf{w}^{(2)}_j \cdot \mathbf{z}^{(1)})$$

Possiamo iterare questo passaggio anche più volte, ottenendo quindi una **serie di inner layer**.
Perciò al livello $r > 1$ riceviamo in input il vettore $\mathbf{z}^{(r-1)}$, applichiamo $m_{r}$ **attivazioni** $$a^{(r)}_j = \mathbf{w}^{(r-1)}_j  \cdot \mathbf{z}^{(r-1)}$$ per poi applicare la funzione di attivazione $h_r$, ottenendo il vettore $\mathbf{z}^{(r)}$ con elementi $$z^{(r)}_j = h_r(a^{(r)}_j)$$

![[ML/img/ML_10_8.png]]

## Output layer
Riguardo l'ultimo layer, diciamo $D$, esso avrà esattamente come tutti gli altri layer.
Ciò che cambia è la definizione di $m_D$ e della funzione $h_D$ che dipendono dal tipo di problema.
1. Per il problema della regressione abbiamo che $m_D = 1$ e che $h_D$ è la **funzione identità**. 
   ![[ML/img/ML_10_9.png]]
2. Per il problema della classificazione binaria avremo che $m_D = 1$ e che $h_D$ è la [[Naive Bayes Classifier#^8e0f70|funzione logistica]] $\sigma$ 
   ![[ML/img/ML_10_10.png]]
3. Per il problema della classificazione multiclasse invece avremo che $m_D = K$ dove $K$ è il numero di classi, mentre $h_D$ è la [[Naive Bayes Classifier#^3e232f|funzione softmax]] $s$
   ![[ML/img/ML_10_11.png]]

Di seguito un esempio di rete neurale a 3 livelli per la classificazione multiclasse
![[ML/img/ML_10_12.png]]

$$\mathbf{y} = \mathbf{z}^{(2)} = h_2\left( \mathbf{w}^{(2)} \cdot h_1(\mathbf{w}^{(1)} \cdot \mathbf{x}) \right)$$
$$y_j = z^{(2)}_j = s\left( \sum_{i=1}^{m_1} w_{ji}^{(2)} \cdot h_1\left( \sum_{k=1}^{d}w_{ik}^{(1)} \cdot x_k + w_{i0}^{(1)} \right)  + w_{j0}\right)$$ ^e5d110


```ad-info
È possibile dimostrare che con un multilayerd perceptron è possibile **approssimare** qualsiasi **funzione continua**.
La precisione dell'approssimazione può essere precisa a piacere, e migliora al crescere deil numero di livelli e della loro profondità.
```

# [[Backpropagation]]