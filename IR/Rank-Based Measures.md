Misure come [[Set Based Measures|precision, recall ed F-measure]] sono basati solamente sugli **insiemi** dei documenti restituiti e di quelli target.

Per valutare i nostri sistemi di IR abbiamo bisogno di estendere questi concetti di misure anche agli **score** delle pagine.

----------
# Precision@K (P@K)
Per estendere il concetto di [[Set Based Measures#Precision and Recall|precision]] sui [[Scoring, term weighting & the vector space model|ranking]] procediamo nel seguente modo:
1. Prendiamo il risultato della nostra query.
2. Lo ordiniamo in base agli [[Scoring, term weighting & the vector space model|score]] delle pagine.
3. Prendiamo i primi $k$ documenti.
4. Definiamo la **precision** sull'insieme composto da quei soli $k$ documenti.

Questa quantità è detta **precision@k** o **p@k**.
In maniera analoga, possiamo definire anche la **recall@k** o **r@k**.


------
# Mean Average Precision (MAP)


-------
# Mean Reciprocal Rank (MRR)

