---
date: 2022-11-02
draft: true
---

# POS tagging
Data una frase, voglia generare una sequenza di **tag** che identificano le **parti del discorso**.

Come punto di partenza, posso scorrere tutta la frase ed assegnare, nella maniera piùragionevole possibile, i pos tag.
Dopodiché, a sottosequenze di dimensione fissa, cerco di raffinare i tag.

	ART-NOME-NOME-PREP-NOME-NOME-VERB-NDO-AVV
	ART-NOME-ADJ-PREP-NOME-ADJ-VERB-NDO-AVV
	ART-NOME-VERB-PREP-NOME-ADJ-VERB-NDO-AVV

Per esempio, nella prima *"raffinatura"*, abbiamo applicato la regola
$$N \; N \to  N \; Adj$$

Questo è anche detto **Brill tagger**.

[VEDI BRILL TAGGER DAL LIBRO]

## Modello discriminativo
Supponiamo di avere un **corpo annotato** (vedi [[NLP/Lecture 5#^d49e53|Lecture 5]] e [[NLP/Lecture 6|Lecture 6]]).

Abbiamo la sequenza di parole
$$W = w_1, w_2, ..., w_n$$ e un **corpus annotato** sull'intera fase.

Si vuole trovare una sequenza di tag $$T = t_1, ..., t_n$$ tale che massimizzi la probabilità
$$P(t_1, ..., t_n \vert w_1, ..., w_n) = P(T \vert W)$$

Come prima cosa, se la frase $W$ fa già parte dell'annotazione, abbiamo già $T$ a disposizione.

Come facciamo a trovare un $T$ se invece non abbiamo $W$?

```ad-tldr
title: Chain rule
Ricordare che $$P(A \cap B \cap C) = P(A \vert B \cap C)P(B \vert C)P(C)$$

Analogamente per qualsiasi sequenza di qualsiasi lunghezza.
```

Perciò avremo che 
$$\begin{align}
P(t_1, ..., t_n \vert w_1, ..., w_n)
&= P(t_1 \vert t_2, ..., t_n, w_1, ..., w_n)P(t_2, ..., t_n \vert w_1, ..., w_n)\\
&= P(t_1\vert t_2^nw_1^n)P(t_2\vert t_3w_1^n)P(t_3^n\vert w_1^n)\\
&= P(t_1\vert t_2^nw_1^n)P(t_2\vert t_3w_1^n)P(t_3\vert t_4^n w_1^n)P(t_4^n \vert t_5^nw_1^n)\\
&\vdots\\
&= \dots P(t_n \vert w_1,...,w_n)
\end{align}$$

Essendo troppo complesso stimare una probabilità tale, possiamo **per semplificare** assumere che un tag $t_i$ dipende solamente dal tag succesivo $t_{i+1}$ e da due parole $w_i, w_{i+1}$.


Dopodiché, per stimare $P(t_i \vert t_{i+1}, w_i, w_{i+1})$ possiamo stimare in maniera **frequentista** come 
$$P(t_i \vert t_{i+1}, w_i, w_{i+1}) \approx \frac{\# \lbrace (w_i, t_i), (w_{i+1}, t_{i+1}) \rbrace}{\# \lbrace (w_i, x), (w_{i+1}, t_{i+1}) \vert \forall x = t_1, ..., t_n \rbrace}$$

## Modello generativo
$$P(T \vert W) \propto P(W \vert T) P(T)$$

### Hidden Markov model
[...]
$$P(W \vert T) P(T) \approx \prod_{i=1}^{n} \hat{P}(w_i \vert t_i) \hat{P}(t_i \vert t_{i+1})$$
