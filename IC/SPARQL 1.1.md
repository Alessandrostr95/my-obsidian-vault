SPARQL 1.1 (introdotto nel 2013) è un'evoluzione di [[SPARQL|SPARQL 1.0]], totalmente **retrocompatibile**, che introduce però nuove funzionalità.

Tra le nuove feature più importanti introdotte ci sono:
- Aggregazione
- Subquery  
- Negazione  
- Property Path
- Assegnazione
- Update

# Aggegazione
Come nelle classiche basi di dati, SPARQL 1.1 consente di **aggregare** informazioni ed elaborarle in maniera unica.
Per esempio, consideriamo i seguenti dati.
```turtle
@prefix : <http://example.com/data/#> .

:x :price 1, 2, 3, 4 .
:y :price 5, 6.0, 7.0 .
```

Se volessimo avere il valore medio, o la somma dei valori delle proprietà `:p` in SPARQL 1.1 possiamo farlo come
```sparql
# seleziona il prezzo medio e la somma di tutti prezzi.
PREFIX : <http://example.com/data/#>
SELECT (AVG(?h) AS ?avg) (SUM(?h) AS ?sum)
WHERE {
  ?g :price ?h .
}
```

avg | sum
---|---
4.0 | 28

Possiamo anche fare aggregazione **raggruppando** rispetto a determinate **variabili** date.
```sparql
# seleziona il prezzo medio e la somma di tutti prezzi, raggruppati per ogni valore che unifica la variabile ?g.
PREFIX : <http://example.com/data/#>
SELECT ?g (AVG(?h) AS ?avg) (SUM(?h) AS ?sum)
WHERE {
  ?g :price ?h .
}
GROUP BY ?g
```

g | avg | sum
---|---|---
`<http://example.com/data/#x>` | 2.5 | 10
`<http://example.com/data/#y>` | 6.0 | 18

Possiamo anche definire dei **selettori** 
```sparql
# seleziona il prezzo medio e la somma di tutti prezzi, raggruppati per ogni valore che unifica la variabile ?g, tali che il prezzo medio è almeno 5.
PREFIX : <http://example.com/data/#>
SELECT ?g (AVG(?h) AS ?avg) (SUM(?h) AS ?sum)
WHERE {
  ?g :price ?h .
}
GROUP BY ?g
HAVING (AVG(?avg) > 5)
```

g | avg | sum
---|---|---
`<http://example.com/data/#y>` | 6.0 | 18