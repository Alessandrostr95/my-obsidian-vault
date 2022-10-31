Vedremo ora come **costruire** un [[Inverted Index]].
Questo processo è noto come **index construction** o **indexing** (**indicizzazione**).

Generalmente quando si progetta un sistema di IR lo si fa in base alle caratteristiche hardware sul quale dovrà essere implementato.

Symbol | Statistic | Value
:---:|---|---
$\sigma$ | average seek time | $5 \text{ ms} = 5 \times 10^{-3} \text{ s}$
$b$ | transfer time per byte | $0.02 \; \mu s = 2 \times 10^{-8} \text{ s}$
\ | processor's clock rate | $10^9 s^{-1}$
$p$ | lowlevel operation (e.g., compare & swap a word) | $0.01 \; \mu s = 10^{-8} \text{ s}$
\ | size of main memory | several GB
\ | size of disk space | 1 TB or more

