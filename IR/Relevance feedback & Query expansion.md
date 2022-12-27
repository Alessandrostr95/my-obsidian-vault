Abbiamo [[Evaluation of IR systems|visto in precedenza]] come valutare la **qualità** di un sistema di IR, in particolare abbiamo visto:
- come valutare rispetto al [[Set Based Measures|numero di documenti rilevanti]] restituiti.
- come valutare rispetto al [[Rank-Based Measures|ranking]] dei documenti restituiti.

Abbiamo anche visto che per misurare la qualità di un sistema abbiamo bisogno di un **evaluation benchmark**, il quale è composto da tre elementi:
1. un sottocollezione di documenti su cui fare i test.
2. un insieme di test-query da sottoporre al sistema rispetto alla sottocollezione di documenti di test.
3. un **etichettamento** di rilevanza (fatto a priori) di ogni coppia *documento-query* del nostro benchmark.

Tra le misure più semplici viste abbiamo:
- La [[Set Based Measures#^e84cfa|Precision]] $P$, ovvero la frazione di documenti rilevanti nella risposta $$P = \frac{\# \text{relevant document retrieved}}{\# \text{retrieved document}} = \frac{tp}{tp+fp}$$
- La [[Set Based Measures#^eed895|Recall]] $R$, ovvero la frazione di documenti rilevanti restituiti in totale $$R = \frac{\# \text{relevant document retrieved}}{\# \text{relevant document}} = \frac{tp}{tp+fn}$$
- La [[Set Based Measures#F-Measure|F-Measure]] $F$, ovvero una **media armonica** tra precision e recall. Un caso particolare è la $F_1$ measure, dove precision e recall sono ugualmente pesate $$F_1 = \frac{2PR}{P+R}$$

