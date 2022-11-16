Abbiamo visto in precedenza che è utile rappresentare i documenti come:
- un [[Binary Term-Document Incidence Matrix|binary term vector]] in $\lbrace 0,1 \rbrace^{\vert V \vert}$
- un [[Bag of words model - Term Frequency tf#^6f3674|count term vector]] in $\mathbb{N}^{\vert V \vert}$

Alla luce di quanto visto fin ora, è più ragionevole definire una matrice di valori **reali**, dove alla cella $(i,j)$ è presente il [[TF-IDF weight|peso tf-idf]] $w_{i,j}$.