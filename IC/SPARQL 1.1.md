SPARQL 1.1 (introdotto nel 2013) è un'evoluzione di [[SPARQL|SPARQL 1.0]], totalmente **retrocompatibile**, che introduce però nuove funzionalità.

Tra le nuove feature più importanti introdotte ci sono:
- Aggregazione
- Subquery  
- Negazione  
- Property Path
- Assegnazione
- Update

# Aggregazione
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

Possiamo anche definire dei **selettori**.
Con **HAVING** si può **filtrare** sui gruppi (in modo simile a [[SPARQL#FILTER|FILTER]]).
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

Tra le funzioni di aggregazione abbiamo:
- **COUNT**: restituisce il numero di elementi di un gruppo.
- **SUM**: somma gli elementi di un gruppo (qualora appartengano a un semigruppo).
- **MIN**: restituisce il **minimo** di un gruppo (qualora sia parzialmente ordinato).
- **MAX**: restituisce il **minimo** di un gruppo (qualora sia parzialmente ordinato).
- **AVG**: restituisce la **media** degli elementi di un gruppo (qualora appartengano a un semigruppo).
- **GROUP\_CONCAT**: concatena gli elementi di un gruppo, specificando opportunamente un **separatore**.
- **SAMPLE**: restituisce un elemento a caso da un gruppo.

In tutte queste funzioni è possibile specificare la keyword **DISTINCT** per considerare solamente gli elementi distinti di un gruppo.
```sparql
GROUP_CONCAT (DISTINCT ?name, SEPARATOR=", ")
```

# Subquery
Le **subquery** sono un modo in SPARQL di **incapsulare** query dentro altre query.
Si usano le sottoquery per accedere a informazioni che generalmente non possono essere accesse in maniera diretta, se non tramite un altra query.

Verrà sempre eseguita la query più interna per poi procedere verso quelle più esterne.
Questo può anche essere utile per **forzare** l'esecuzione in un determinato ordine.

Le sottoquery sono query a tutti gli effetti, periò possono anche utilizzare gli [[#Aggregazione|operatori di aggregazione]].

```turtle
# DATA
@prefix : <http://people.example/> .

:alice :name "Alice", "Alice Foo", "A. Foo" .
:alice :knows :bob, :carol .
:bob :name "Bob", "Bob Bar", "B. Bar" .
:carol :name "Carol", "Carol Baz", "C. Baz" .
```

Si vuole il nome (il primo in ordine alfabetico) per tutte le persone che:
- conoscono Alice.
- hanno un nome.

```sparql
PREFIX : <http://people.example/> .

SELECT ?y ?minName 
WHERE {
	:alice :knows ?y . # tutti gli y che conosce Alice
	{
		SELECT ?y (MIN(?name) as ?minName) # prendo il minimo di tutti e lo chiamo `minName`
		WHERE {
			?y :name ?name . # tutti i nomi di y
		} GROUP BY ?y # raggruppati per y
	}
}
```

# Negazione
In [[SPARQL|SPARQL 1.0]] si poteva implementare la **negazione** tramite [[SPARQL#OPTIONAL|OPTIONAL]] + [[SPARQL#^1873d3|!BOUND]].

In SPARQL 1.1 sono stati introdotti altre due modi:
- **NOT EXISTS**: da usare dentro [[SPARQL#FILTER|FLITER]], verifica che un graph pattern **non unifica** (dato il valore assegnato alle variabili), e in tal caso restituisce true.
- **MINUS**: vengono sottratte alla soluzione tutte le triple che unificano il graph pattern dentro la clausola MINUS.

`NOT EXISTS`e `MINUS` reppresentano due modi di pensare alla **negazione**, uno basato sul testere se un pattern non esiste nei dati risultanti da un query, l'altro basato sul rimuovere quello che non mi serve di un risultato.

## NOT EXISTS
```turtle
@prefix : <http://example/> .
@prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
@prefix foaf: <http://xmlns.com/foaf/0.1/> .

:alice rdf:type foaf:Person .
:alice foaf:name "Alice" .
:bob rdf:type foaf:Person .
```

```sparql
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX foaf: <http://xmlns.com/foaf/0.1/>

SELECT ?person
WHERE {
	?person rdf:type foaf:Person . # tutte le entità foaf:Person
	FILTER NOT EXISTS { ?person foaf:name ?name } # le quali non unificano foaf:name
}
```

person | 
---|---
`<http://example/bob>` |

## MINUS
```turtle
@prefix : <http://example/> .
@prefix foaf: <http://xmlns.com/foaf/0.1/> .

:alice foaf:givenName "Alice" ;
		foaf:familyName "Smith" .

:bob foaf:givenName "Bob" ;
		foaf:familyName "Jones" .

:carol foaf:givenName "Carol" ;
		foaf:familyName "Smith" .
```

```sparql
PREFIX : <http://example/>
PREFIX foaf: <http://xmlns.com/foaf/0.1/>

SELECT DISTINCT ?s as ?person
WHERE {
	?s ?p ?o .
	MINUS {
		?s foaf:givenName "Bob" .
	}
}
```

person | 
---|---
`<http://example/alice>` |
`<http://example/carol>` |

## MINUS vs NOT EXISTS
Anche se [[#MINUS]] e [[#NOT EXISTS]] vengono usati per generare risultati uguali, potrebbe capitare che ritornino risultati completamente differenti.

Infatti MINUS e NOT EXISTS tornano gli stessi risultati quando almeno una variabile usata al loro interno viene usata anche nel resto della query (nella WHERE).

Consideriamo il seguente esempio
```turtle
@prefix : <http://example/> .
:a :b :c .
```

```sparql
# NOT EXISTS
SELECT *
WHERE { 
  ?s ?p ?o
  FILTER NOT EXISTS { ?x ?y ?z }
}
```
In questo caso `{ ?x ?y ?z }` non unifica con **nessuna** delle variabili `?s, ?p, ?o` del graph pattern.
Perciò `NOT EXISTS { ?x ?y ?z }` elimina tutte le triple della soluzione.

s | p | o
---|---|---
 | |

```sparql
# MINUS
SELECT *
WHERE { 
  ?s ?p ?o
  MINUS { ?x ?y ?z }
}
```
Invece con MINUS nessuna variabile di `{ ?x ?y ?z }` è condivisa con `?s, ?p, ?o`, perciò nulla verrà rimosso (viene restituito tutto).

s | p | o
---|---|---
`<http://example/a>`|`<http://example/b>`|`<http://example/c>`

# Property Path
Un **property path** è un possibile **cammino** di proprietà tra due nodi del grafo [[RDF]].
Una relazione può essere vista come un cammino di lunghezza esattamente 1.

I Property paths estendono l'espressività dei semplici **graph pattern** SPARQL e introducono anche l'abilità di verificare la connettività tra due risorse, anche con cammini di **lunghezza arbitraria**.

Syntax Form | Property Path Expression Name | Matches
---|---|---
`iri` | PredicatePath | An IRI. A path of length one.
`^elt` | InversePath | Inverse path (object to subject).
`elt1 / elt2` | SequencePath | A sequence path of _elt1_ followed by _elt2_.
`elt1` \| `elt2` | AlternativePath | A alternative path of _elt1_ or _elt2_ (all possibilities are tried).
`elt*` | ZeroOrMorePath | A path that connects the subject and object of the path by zero or more matches of _elt_.
`elt+` | OneOrMorePath | A path that connects the subject and object of the path by one or more matches of _elt_.
`elt?` | ZeroOrOnePath | A path that connects the subject and object of the path by zero or one matches of _elt_.
`!iri` or !(`iri_1`\| ...\|`iri_n`) | NegatedPropertySet | Negated property set. An IRI which is not one of _irii_. !_iri_ is short for !_(iri)_.
`!^iri` or !(`^iri_1`\| ...\|`^iri_n`) | NegatedPropertySet | Negated property set where the excluded matches are based on reversed path. That is, not one of _iri\_1_..._iri\_n_ as reverse paths. !^_iri_ is short for !(^_iri_).
!(`iri_1`\| ...\|`iri_j`\|`^iri_(j+1)`\| ...\|`^iri_n`) | NegatedPropertySet | A combination of forward and reverse properties in a negated property set.
`(elt)` | | A group path _elt_, brackets control precedence.

```sparql
# ESEMPIO 1
SELECT ?x ?name
WHERE {
	?x foaf:mbox <mailto:alice@example> .
	?x foaf:knows ?a1 .
	?a1 foaf:knows ?a2 .
	?a2 foaf:name ?name .
}

# EQUIVALENTE A
SELECT ?x ?name
WHERE {
	?x foaf:mbox <mailto:alice@example> .
	?x foaf:knows/foaf:knows/foaf:name ?name .
}
```

```sparql
# ESEMPIO 2
{
	?x foaf:knows ?a .
	?y foaf:knows ?a .
	FILTER (?x != ?y)
}

# EQUIVALENTE A
{
	?x foaf:knows/^foaf:knows ?y .
}
```

```sparql
# ESEMPIO 3
# tutti i foaf:name di tutte le persone conosciute (anche in maniera transifitiva) da alice
{
	?x foaf:name "Alice" .
	?x foaf:knows+/foaf:name ?name .
}
```

```sparql
# ESEMPIO 4
# tutte le risorse che hanno una proprietà che è sottoproprietà (anche in modo transitivo) di :my_property
{
	?x ?p ?v .
	?p rdfs:subPropertyOf* :my_property 
}
```

```sparql
# ESEMPIO 5
# tutte le riosre che sono tipo di una sottoclasse nella gerarchia di :MySuperClass
{
	?x rdf:type/rdfs:subClassOf* :MySuperClass.
}
```

# Assegnazione
In SPARQL è possibile assegnare il valore di una espressione ad una **variabile**.

## BIND
```sparql
# ritorna i titoli e i prezzi di tutti i libri tali che sotto sconto valgono meno di 20.
PREFIX dc: <http://purl.org/dc/elements/1.1/>
PREFIX ns: <http://example.org/ns#>

SELECT ?title ?price
WHERE {
	?x ns:price ?p .
	?x ns:discount ?discount .
	BIND (?p * (1 - ?discount) AS ?price)
	FILTER (?price < 20)
	?x dc:title ?title .
}
```

```sparql
PREFIX foaf: <http://xmlns.com/foaf/0.1/>

SELECT ?name
WHERE {
	?p foaf:givenName ?g ;
		foaf:surename ?s .
	BIND (CONCAT(?g, " ", ?s) AS ?name)
}
```

## VALUES
**VALUES** invece è come se fosse un BIND ma su molteplici valor.

```sparql
# ritorna i titoli e i prezzi dei soli libri book1 e book3.
PREFIX dc: <http://purl.org/dc/elements/1.1/>
PREFIX ns: <http://example.org/ns#>
PREFIX : <http://example.org/book/>

SELECT ?book ?title ?price
WHERE {
	VALUES ?book { :book1 :book3 }
	?book dc:title ?title ;
			ns:price ?price .
}
```