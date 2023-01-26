Per cercare di ovviare alle [[RDF Schema#Limitazioni di RDFS|limitazioni di RDFS]], il Web-Ontology Working Group ha sviluppato il linguaggio **OWL** (**Web Ontology Language**).

La semantica RDF/OWL è detta di tipo **inferenziale**.
In una semantica classica (o di tipo **constraint checking**), data una teoria del mondo, vengono verificate esclusivamente che gli oggetti della nostra base di conoscenza soddisfino i **vincoli imposti** da tale teoria.
In una semantica ti tipo inferenziale invece è possibile **aggiungere** (**inferire**) nuova informazione alla teoria e/o alla descrizione degli oggetti, per far sì che questi siano ancora un modello per la teoria.
Le nuove informazioni vengono inferite in modo rigorosamente **monotono**, ovvero senza ritrattare/modificare informazione già esistente.

# Closed World Assumption & Negation-as-Failure
Le basi di dati classiche si basano sulla **closed world assumption** (**CWA**), la quale ci dice che data una formula atomica $A$ non può essere derivata dai dati in una base di conoscenza, allora $A$ è **falsa**.

I linguaggi logici come il Prolog sono invece caratterizzati dal principio **negatio-ad-failure** (**NF**), il quale ci dice che se una formula atomica $A$ è falsa nella nostra base di conoscenza, allora non può essere derivata come vera dai dati al suo interno.

Per esempio abbiamo un database con un solo fatto
```prolog
woman(marilynMonroe).
```
Eseguendo le seguenti query in prolog otterremo
```prolog
?- woman(marilynMonroe).
true.

?- man(marilynMonroe).
flase.    % per CWA e NF
```
Solamente aggiornando il database possiamo asserire che
```prolog
man(marilynMonroe).

?- man(marilynMonroe).
true.
```

# Open World Assumption & No Unique Name Assumption

^963225

OWL invece si basa su due principi:
- **Open World Assumption** (**OWA**), ovvero l'assunzione secondo cui la **verità** di una dichiarazione $A$ può essere positivo **indipendentemente** dal fatto che $A$ sia un fatto conosciuto essere vero nella base di dati in questione. Ovvero il fatto che non posso dedurre $A$ dalla mia base di conoscenza, non significa che sia falso.
- **No Unique Name Assumption**

### Esempio 1
```turtle
eg:Document rdf:type owl:Class;
	rdfs:subClassOf [ a owl:Restriction;
						owl:onProperty dc:author;
						owl:minCardinality 1^^xsd:integer].

eg:myDoc rdf:type eg:Document .
```

Abbiamo definito la classe `Document` specificando la **restrizione** che ogni documento deve avere **almeno un autore**.
Però nella definizione dell'istanza `eg:myDoc` non viene rispettata la restrizione di minimia cardinalità 1.
Nonostante ciò la definizione risulta corretta, in quanto l'autore potrebbe essere definito da qualche altra parte del mondo (**open world assumption**).

### Esempio 2
```turtle
eg:Document rdf:type owl:Class;
	rdfs:subClassOf [ a owl:Restriction;
						owl:onProperty eg:copyrightHolder;
						owl:maxCardinality 1^^xsd:integer].

eg:myDoc rdf:type eg:Document ;
	eg:copyrightHolder eg:institute1 ;
	eg:copyrightHolder eg:institute2 .
```
In questo esempio abbiamo invece definito la restrizione che un doumento può avere **al massimo** un *copyright holder*.
Nella definizione di `eg:myDoc` però sono stati definiti due copyright holder: `intitute1` e `institute2`.
Questo però non da problemi, in quanto grazie alla **no unique name assumption** potrebbero esistere due nomi per indicare la stessa cosa, così **inferiamo** che `institute1` e `institute2` denotano **lo stesso oggetto**.

### Esempio 3
```turtle
eg:Document rdf:type owl:Class;
	rdfs:subClassOf [ a owl:Restriction;
						owl:onProperty eg:author ;
						owl:allValuesFrom eg:Person].

eg:myDoc rdf:type eg:Document ;
	eg:author eg:Daffy.
eg:Daffy rdf:type eg:Duck.

eg:myDoc2 eg:author eg:Dave .
eg:Dave rdf:type eg:Person.
```
In questo esempio invece abbiamo vincolato gli autori dei documenti ad essere tutti della classe `eg:Person`.
Però `myDoc` ha come autore `Duffy`, il quale è un'istanza della classe `eg:Duck`.
Si inferisce che `Duffy` è **anche** una `eg:Person`.

Possiamo invece dire che `myDoc2` è un documento dato che come autore ha una `eg:Person`?
Per OWA non possiamo saperlo.