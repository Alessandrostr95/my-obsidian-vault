# Classi
Anche in OWL una **classe** rappresenta una **raccolta** di oggetti.
Una classe può essere una **sottoclasse** di un'altra, ereditando le caratteristiche dalla sua **superclasse**.
In notazione **description logic** (o **DL**) con $A \sqsubseteq B$ che $A$ è una sottoclasse di $B$.

Tutte le classi sono sottoclassi di `owl:Thing`, la **superclasse radice**, denotata con $\top$.
Perciò in DL $A \sqsubseteq \top$ à una tautologia.
```xml
<!-- Definizione esplicita di classe -->
<owl:Class rdf:ID="Person" />

<!-- Modo per descrivere le istanze di una classe -->
<Person rdf:ID="manuel" />

<owl:Thing rdf:ID="manuel">
	<rdf:type rdf:resource="#Person" />
</owl:Thing>
```

# Proprietà
Una **proprietà** è una caratteristica di una classe, ovvero una **relazione binaria** diretta che specifica un attributo per tutte le istanze di quella classe.
Le proprietà possono avere caratteristiche logiche, come ad esempio ad esempio la transitività, la simmetria, l'inversa ecc...
Le proprietà possono anche avere [[RDF Schema#^70b359|domini]] e [[RDF Schema#^e58a30|intervalli]].

## Datatype Property
Le **datatype property** sono relazioni tra **istanze** di classi e **valori letterali** RDF o tipi di dati dello schema XML.
```xml
<!-- proprietà che mette in relazione le istanze di tipo persona con le stringhe (ovvero i nomi delle persone) -->
<owl:DatatypeProperty rdf:ID="nome">
	<rdfs:domain rdf:resource="#Persona" />
	<rdfs:range rdf:resource="&xsd;string" />
</owl:DatatypeProperty>

<!-- classe Range, dove gli estremi di un intervallo sono dei float -->
<owl:Class rdf:ID="Range" />

 <owl:DatatypeProperty rdf:ID="min">
	  <rdfs:domain rdf:resource="#Range"/>
	  <rdfs:range rdf:resource="http://www.w3.org/2001/XMLSchema#float"/>
 </owl:DatatypeProperty>

 <owl:DatatypeProperty rdf:ID="max">
	  <rdfs:domain rdf:resource="#Range"/>
	  <rdfs:range rdf:resource="http://www.w3.org/2001/XMLSchema#float"/>
 </owl:DatatypeProperty>
```

## Object Property
Le **object property sono relazioni tra istanze di due classi**.
Ad esempio, OwnedBy può essere una proprietà del tipo di oggetto della classe Vehicle e può avere un intervallo che è la classe Person.
```xml
<owl:Class rdf:ID="Persona" />

<rdfs:Property rdf:ID="conosce" />
	<rdfs:domain rdf:resource="#Persona" />
	<rdfs:range rdf:resource="#Persona" />
<rdfs:Property/>

<owl:ObjectProperty rdf:ID="ama">
	<rdfs:domain rdf:resource="#Persona" />
	<rdfs:range rdf:resource="#Persona" />
	<rdfs:subPropertyOf rdf:resource="#conosce" />
</owl:ObjectProperty>
```


