Lo **Shift-Reduce Parser** è un algoritmo di [[Parsing]] per [[Grammatiche Formali#Tipo 2 - Grammatiche Context-Free|grammatiche context-free]].

Tale algoritmo prende in input:
- una [[Grammatiche Formali#Tipo 2 - Grammatiche Context-Free|grammatica context-free]] $G = \langle N, \Sigma, P, S \rangle$ (<u>non necessariamente</u> espressa in [[Grammatiche Formali#Grammatiche CNF - Chomsky Normal Form|CNF]] come richiesto dall'algoritmo [[CYK]]).
- una stringa $I \in \Sigma^*$.
e **decide** se la strigna $I$ è una stringa del linguaggio $L(G)$ generato dalla grammatica $G$.

```ad-note
Verrà descritto solamente la versione **decisionale** dell'algoritmo, ma quest'ultima è facile poi estenderla per generare un **albero sintattico**.
```

Inizialmente l'algoritmo crea una struttura dati **stack** $\mathcal{S}$, il quale inizialmente avrà al suo interno il solo [[Grammatiche Formali#Grammatica Formale|assioma]].

Durante l'esecuzione dell'algoritmo vengono eseguite delle operazioni di **shift**, vengono inserite in cima allo stack $\mathcal{S}$ le parole di $I$ una ad una, da *sinistra verso destra*.

Se il **simbolo** $\beta$ in cima ad $\mathcal{S}$ rispetta una **produzione** $\alpha \to \beta$ , allora $\beta$ verrà rimosso e sostituito col simbolo $\alpha$.
Questa è detta operazione di **reduce**.

```ad-important
Importante osservare che il simbolo $\alpha$ della produzione $\alpha \to \beta$ è necessariamente un simbolo **non terminale** $\alpha \in N$, dato che $G$ è una [[Grammatiche Formali#Tipo 2 - Grammatiche Context-Free|grammatica context-free]].
```

Se dopo aver *consumato* tutte le parole di $I$ e aver fatto tutte le *riduzioni* possibili nello stack $\mathcal{S}$ è rimasto il solo **assioma** di $G$, allora possiamo dire che $I \in \mathcal{L}(G)$ (ovvero $I$ è **generata** dalla grammatica $G$).

[VEDI ESEMPIO](http://cons.mit.edu/sp13/slides/S13-lecture-03.pdf)