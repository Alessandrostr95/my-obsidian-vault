# Precision & Recall
Definiamo l'insieme $T$ di documenti **rilevanti**, detto anche **target**.
Sia invece $R$ l'insieme di documenti restituiti a seguito di una query.
Definiamo le seguenti misure

- **Recall**: la frazione di documenti rilevanti ritornati $$\text{recall} = \frac{\vert T \cap R \vert}{T} = \frac{tp}{tp + fn}$$
- **Precision**: la frazione di documenti rilevanti presenti nella mia risposta $$\text{precision} = \frac{\vert T \cap R \vert}{R} = \frac{tp}{fp + tp}$$

\ | **Relevant** | **Non Relevant**
---|---|---
**Retrieved** | true-positive: $tp = T \cap R$ | false-positive: $fp = R \setminus T$
**Not Retrieved** | false-negative: $fn = T \setminus R$ | true-negative: $tn = \lnot (T \cap R)$
