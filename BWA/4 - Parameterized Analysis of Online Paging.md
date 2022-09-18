# Parameterized Analysis of Online Paging
Ricordiamo da [[3 - Online Paging and Resource Augmentation]] che l'[[3 - Online Paging and Resource Augmentation#Competitive Analysis|analisi competitiva]] non risulta essere molto utile per soddisfare gli [[1 - Introduction#Goals of Analyzing Algorithms|obiettivi desiderati]].

Infatti per quanto riguarda il [[1 - Introduction#Goal 1 Performance Prediction|Goal 1]], abbiamo visto che il [[3 - Online Paging and Resource Augmentation#^2d11f5|competitive ratio]] non è una buona unità di misura delle prestazioni di un algoritmo online di paging, in quanto assumono valori [[3 - Online Paging and Resource Augmentation#^f725b1|troppo grandi]] per essere prese sul serio.

Per il [[1 - Introduction#Goal 2 Identify Optimal Algorithms|Goal 2]] abbiamo invece visto che l'analisi competitiva identifica [[3 - Online Paging and Resource Augmentation#^adc4cd|LRU come ottimo]], però identifica come ottimo anche algoritmi intuitivamente (e <u>empiricamente</u>) peggiori (come [[3 - Online Paging and Resource Augmentation#^cd1d79|FWF]]).

In seguito abbiamo introdotto la tecnica della [[3 - Online Paging and Resource Augmentation#Resource Augmentation|Resource Augmentation]].
Abbiamo visto che ci consentiva i fare un passo avanti rispeto al [[1 - Introduction#Goal 1 Performance Prediction|Goal 1]], infatti spiega come crescono le prestazioni (in termini di [[3 - Online Paging and Resource Augmentation#^2d11f5|competitive ratio]]) di un algoritmo online al crescere dell'**aumento di risorse** (ovvero la dimensione della *cache*).

Purtroppo, riguardo il [[1 - Introduction#Goal 2 Identify Optimal Algorithms|Goal 2]], la *resource augmentation* non ci aiuta, in quanto non distingue LRU, da FIFO da FWF.

Intuitivamente, poiché la *superiorità empirica* di LRU sembra essere guidata dalle **proprietà** dei dati del "*mondo reale*" (ovvero la **località di riferimento**) non possiamo aspettarci di riuscire a distinguere LRU da altri algoritmi di paging naturali (come FIFO) senza almeno articolare implicitamente tali proprietà.

# Parameterized Analysis
