Sia $N_d(v)$ l'insieme di tutti i nodi raggiungibili da $v$ a distanza al più $d$.
Osserviamo che dall'[[All-Distances Sketches|ADS]] di $v$ possiamo ricavare il [[MinHash Sketches|MinHash Sketch]] per tutti i nodi a distanza $d$.
Infatti esso equivale all'insieme
$$ASD(v) \cap \lbrace u | d_{u,v} \leq d \rbrace$$
