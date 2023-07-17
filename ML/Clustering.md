Il problema del **clustering** consiste nel classificare, o identificare un pattern nei dati, senza però avere a disposizione degli esempi.
In termini di dataset, abbiamo a disposizione solamente gli elementi di $\mathbf{X}$ senza però avere a disposizione i rispettivi target $\mathbf{t}$.

Formalmente, dato un insieme di punti $\mathbf{X} \subseteq \mathbb{R}^d$ lo vogliamo **partizionare** in sottoinsiemi $C_1, ..., C_k$ di **elementi simili** tra loro, detti **clusters**.
Ogni cluster può essere rappresentato da dei rispettivi **prototipi** $(m_1, ..., m_k)$.
Perciò ogni elemento $\mathbf{x} \in \mathbf{X}$ è assegnato al cluster del prototipo ad esso più simile, o vicino. ^39c993

Possiamo quindi definire una codifica **one-hot**, dove $r_{ij} = 1$ se e solo se l'elemento $\mathbf{x}_i$ è assegnato al cluster $C_j$, altrimenti $r_{ij} = 0$.

Esistono due tipologie di clustering:
1. **Clustering Partizionale**: nel quale vogliamo assegnare ogni elemento di $\mathbf{X}$ ad una partizione in modo tale massimizzare (o minimizzare) una funzione di costo $J$. Il numero di partizioni $k$ può essere dato o computato.
2. **Clustering Gerarchico**: si parte da una partizione in cui ogni elemento di $\mathbf{X}$ è una partizione a se stante (un **singleton**). A partire da queste trovare partizioni con più elementi composto da altre sottopartizioni.

# [[Clustering Partizionale - K-mean clustering]]
# [[Clustering Gerarchico]]