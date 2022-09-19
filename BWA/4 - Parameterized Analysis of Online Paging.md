# Parameterized Analysis of Online Paging
Ricordiamo da [[3 - Online Paging and Resource Augmentation]] che l'[[3 - Online Paging and Resource Augmentation#Competitive Analysis|analisi competitiva]] non risulta essere molto utile per soddisfare gli [[1 - Introduction#Goals of Analyzing Algorithms|obiettivi desiderati]].

Infatti per quanto riguarda il [[1 - Introduction#Goal 1 Performance Prediction|Goal 1]], abbiamo visto che il [[3 - Online Paging and Resource Augmentation#^2d11f5|competitive ratio]] non è una buona unità di misura delle prestazioni di un algoritmo online di paging, in quanto assumono valori [[3 - Online Paging and Resource Augmentation#^f725b1|troppo grandi]] per essere prese sul serio.

Per il [[1 - Introduction#Goal 2 Identify Optimal Algorithms|Goal 2]] abbiamo invece visto che l'analisi competitiva identifica [[3 - Online Paging and Resource Augmentation#^adc4cd|LRU come ottimo]], però identifica come ottimo anche algoritmi intuitivamente (e <u>empiricamente</u>) peggiori (come [[3 - Online Paging and Resource Augmentation#^cd1d79|FWF]]).

In seguito abbiamo introdotto la tecnica della [[3 - Online Paging and Resource Augmentation#Resource Augmentation|Resource Augmentation]].
Abbiamo visto che ci consentiva i fare un passo avanti rispeto al [[1 - Introduction#Goal 1 Performance Prediction|Goal 1]], infatti spiega come crescono le prestazioni (in termini di [[3 - Online Paging and Resource Augmentation#^2d11f5|competitive ratio]]) di un algoritmo online al crescere dell'**aumento di risorse** (ovvero la dimensione della *cache*).

Purtroppo, riguardo il [[1 - Introduction#Goal 2 Identify Optimal Algorithms|Goal 2]], la *resource augmentation* non ci aiuta, in quanto non distingue LRU, da FIFO da FWF.

Intuitivamente, poiché la *superiorità empirica* di LRU sembra essere guidata dalle **proprietà** dei dati del "*mondo reale*" (ovvero la **località di riferimento**) non possiamo aspettarci di riuscire a distinguere LRU da altri algoritmi di paging naturali (come FIFO) senza almeno articolare implicitamente tali proprietà.

-----------
# Parameterized Analysis
Informalmente l'**analisi parametrizzata** consiste in due *"step"*.
Il primo è quello di definire un **parametro naturale** per ogni input, il quale dovrebbe modellare la *"facilità"* o *"difficoltà"* intuitiva dell'istanza.
Ricordiamo infatti che siamo partiti dall'idea che esistono istanze più *"semplici"* da risolvere rispetto ad altre.

![|600](BWA_04_1.png)

Il secondo step è quello di analizzare le prestazioni di un algoritmo in funzione del parametro scelto.
Questo consente un'analisi più precisa rispetto alla classica analisi *worst-case*, la quale caratterizza le istanze **solo** in funzione della loro dimensione.

Idealmente, nell'applicazione dell'analisi parametrica il parametro dovrebbe essere **indipendente** dall'algoritmo.

```ad-example
title: Example: Running Time of the Kirkpatrick-Seidel Algorithm
Richiamando l'[[2 - Instance-Optimal Geometric Algorithms#The Kirkpatrick-Seidel Algorithm|algoritmo Kirkpatrick-Seidel]], 
abbiamo analizzato in [[2 - Instance-Optimal Geometric Algorithms]] le sue prestazioni rispetto ad ogni istanza, mostrando che è KS è un algorimo [[1 - Introduction#^67367e|instance optimal]].

Infatti siamo partiti analizzando l'algoritmo solamente rispetto al numero dei punti $n$, trovando un upperbound (poco preciso) di $O(n\log{n})$.

Abbiamo poi raffinato l'analisi, parametrizzando anche rispetto alla grandezza dell'output $h$, dando un upperbound migliore di $O(n\log{h})$ **per ogni input**.

In fine abbiamo definito una **parametrizzazione** della *"facilità"* (o *"difficoltà"*) degli input, che era **indipendente** dalla descrizione dell'algoritmo KS, e che dava un upperbound migliore.
```

-------------
# Quantifying Locality: The Working Set Model
[DA FINIRE]