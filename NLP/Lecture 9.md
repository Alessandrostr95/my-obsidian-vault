---
date: 2022/11/10
draft: true
---

- CYK
- Earley Alg.
- PCFG
- Shift-Reduce Parsing

Prendo le parole (con il relativo pos-tag).

- **modalità a costituenti** [VEDI]
- **modalità a dipendenza** [VEDI]

---------------
## Livelli di grammatiche formali
- **Level 0**: o **regolari** $\vert \alpha \vert = 1$ e $\vert \beta \vert = 2$ $\rightarrow$ FSA Finite State Automa
- **Level 1**: o **context-free** $\vert \alpha \vert = 1$ e $\vert \beta \vert \geq 1$ $\rightarrow$ FSA + STACK: PDA Push Down Automa.
- **Level 2**: o **context-sensitive** $\vert \alpha \vert < \vert \beta \vert$ $\rightarrow$ Macchine di Turing 
- **Level 3**: o **libera**. (indecidibile)

## Pumping Lemma
