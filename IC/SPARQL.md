**SPARQL** (**Simple Protocol And RDF Query Language**) è un linguaggio di **interrogazione** per [[RDF|repository RDF]].
Anche se i modelli di dati sottostanta sono totalmente differenti, SPARQL ricorda vagamente la sintassi **SQL**.

# Graph Pattern
Un **Graph Pattern** è uno **schema** (o **pattern**) che indica come **unificare le triple** dal grafo di riferimento.
```sparql
# si chiedono tutti gli oggetti del dominio che sono legati alla risorsa `tor_vergata` tramite la proprietà `works_in` (ovvero tutti i dipendenti di Tor Vergata).
?x :worksIn :tor_vergata .
```
Un Graph Pattern è unificato rispetto a uno sorgente dati RDF.
Per ogni unificazione del pattern viene generata una soluzione.

# Struttura Query
Le query sono tutte della froma
```sparql
PREFIX stx: <http://art.uniroma2.it/ontologies/st_example#>
SELECT ?person  
FROM <http://art.uniroma2.it/ontologies/st_example>
WHERE { ?person stx:name ?name. }
```
- **PREFIX**: assegna un prefisso ad un **namespace**. Tale prefisso può essere utilizzato per riferire tramite un qname compatto (rappresentato da `<prefisso>:<resourcename>`) le risorse presenti nel grafo da interrogare.
- **SELECT**: fornisce l'elenco delle **variabili** da riportare nel risultato.
- **FROM**: indica la sorgente dei dati da interrogare (ad esempio, come in questo caso, un file owl presente su web). È un indirizzo ad una risorsa fisica (il file contenente le triple).
- **WHERE**: nella clausola WHERE viene definito il **graph pattern**.

```ad-tldr
In realtà esistono operatori alternativi alla SELECT quali CONSTRUCT, DESCRIBE o ASK.
In base all'operatore utilizzato, il risultato viene posto in maniera differente.
```

# Matching su datatype
Si può fare matching sui tidi di dato in differenti modi.

Per esempio, molto spesso le stringhe sono espresse in **differenti lingue**, ognuna delle quali specificate da un **language tag** (`@en` per l'inglese, `@it` per l'italiano, ecc...).
Perciò la stringa `"cat"@en` sarà equivalente a `"gatto"@it`. ^428ecd
```sparql
SELECT ?v WHERE { ?v ?p "cat"@en }
```

Alcune volte può essere necessario specificare un **specifico tipo di dato** (identificato dal prorpio IRI).
```sparql
SELECT ?v WHERE {
	?v ?p "abc"^^<http://example.org/datatype#specialDatatype> 
}
```

Per quanto riguarda i valori numerici, possono essere espressi direttamente nel loro tipo più immediato oppure tramite un tipo specifico.
```sparql
SELECT ?v WHERE {
	?v :age "42"^^<http://www.w3.org/2001/XMLSchema#integer> .
	?v :n_car 2 .
}
```

# Blanck nodes
Il risultato di una query può contenere dei **[[RDF#Componenti di una Tripla|blank node]]**.
I blank node sono scritti con un **prefisso vuoto** `_:` seguito da una **label anonima**.

```ad-attention
Se uso una stessa label anonima in una stessa sessione/query, allora unificheranno la **stessa risorsa**.
Se invece utilizzate in sessioni differenti non determina alcun tipo di legame!
```

```sparql
@prefix foaf: <http://xmlns.com/foaf/0.1/> .

_:a foaf:name "Alice" .
_:b foaf:name "Bob" .
```

Ci sono altre sintassi **equivalenti** alla label anonima
```sparql
_:b57 :p "v" . # sintassi con label anonima b57

[ ] :p "v" # prima sintassi alternativa
[:p "v"] # prima sintassi alternativa
```

Queste possono essere combinate
```sparql
:x :q [:p "v"] .

# equivalente a :
:x :q _:b57 .
_:b57 :p "v" .
```

# FILTER
È possibile **filtrare** il matching delle triple utilizzando differenti *operatori*, *funzioni*, *espressioni regolari*, ...

```sparql
# SELEZIONA I NODI DEI MAGGIORENNI.
SELECT ?n WHERE {
	_:p :name ?n .
	_:p :age ?x .
	FILTER (?x > 18) .
}

# SELEZIONA TUTTI GLI UTENTI CON EMAIL GMAIL
SELECT ?u WHERE {
	?u :mail ?m .
	FILTER (regex(?m , "[a-zA-Z0-9]+@gmail.com") ) .
}
```

In genere i valori primitivi su cui definire vincoli sono quelli dell'**XML Schema Definition** (o **XSD**).
Tra i più importanti:
- `^^xsd:boolean`
- `^^xsd:string`
- `^^xsd:integer`
- `^^xsd:decimal`
- `^^xsd:float`
- `^^xsd:double`
- `^^xsd:dateTime`

Tra le **funzioni booleane** che si interrogano sullo **stato**/**natura** delle **variabili** abbiamo:
- `BOUND`: true se la variabile è istanziata con un valore.
- `ISURI` o `ISIRI`: ritorna true se la variabile è un iri o uri.
- `ISLITERAL`: ritorna true se la variabile è un literal.
- `ISBLANCK`: ritorna true se la variabile è un bnode.

Tra le funzioni che **analizzano** una variabile abbiamo:
- `REGEX`: ovvero [[Grammatiche Formali#Espressione Regolare|espressioni regolari]].
- `LANGMATCHES`: verifica che il [[#^428ecd|language tag]]  di un letterale unifichi con il pattern proposto.
- `LANG`: restituisce il [[#^428ecd|language tag]] di un letterale se questo è presente, altrimenti restituisce la  **stringa vuota**.
- `DATATYPE`: restituisce il tipo di dato di una risorsa.
- `STR`: restituisce una rappresentazione letterale di una risorsa.

# OPTIONAL
Un Graph Pattern può avere dei componenti opzionali, espressi tramite la keyword **OPTIONAL**.
```sparql
PREFIX foaf: <http://xmlns.com/foaf/0.1/>
SELECT ?name ?mbox  
WHERE {
	?x foaf:name ?name .
	OPTIONAL { ?x foaf:mbox ?mbox }
}
```
In questo caso verranno restitute tutte le risorse che avranno `foaf:name` e se lo hanno ne verrà riportato anche il `foaf:mbox`.

# UNION
È possibile **combinare** diversi [[#Graph Pattern|Graph Pattern]] di modo che almeno uno di essi unifichi con il grafo interrogato.

```sparql
PREFIX foaf: <http://xmlns.com/foaf/0.1/>  
PREFIX stx: <http://art.uniroma2.it/ontologies/st_example#>

SELECT ?guy  
WHERE {
	{ ?guy a foaf:Person }
		UNION
	{ ?guy a stx:Person }
}
```
In questo caso verranno restituite tutte le risorse definite come appartenenti alla classe `stx:Person` o anche alla classe `foaf:Person`.

