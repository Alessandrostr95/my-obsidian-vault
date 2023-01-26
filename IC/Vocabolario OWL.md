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


## Annotation Property
Una **annotation property** è fuori dalla semantica dell’ontologia.
Servono solo per commentare l’ontologia.

## Esempio Descrizione 
```xml
<Persona rdf:ID="armando">  
	<conosce rdf:resource="#manuel" />  
	<nome rdf:datatype="&xsd;string">Armando</nome>
</Persona>

<owl:Thing rdf:ID="manuel">
	<rdf:type rdf:resource="#Persona" />
	<nome rdf:datatype="&xsd;string">Manuel</nome>
</owl:Thing >
```

# Different From
La proprietà `owl:differentForm` collega risorse a risorse, e **specifica** che due IRI si riferiscono a due risorse **differenti**, definendo quindi dei sottoinsiemi di risorse in cui <u>non</u> vale la [[OWL#^963225|no-unique-name assumption]].

```xml
<Opera rdf:ID="Don_Giovanni"/>

<Opera rdf:ID="Nozze_di_Figaro">
	<owl:differentFrom rdf:resource="#Don_Giovanni"/>
</Opera>

<Opera rdf:ID="Cosi_fan_tutte">
	<owl:differentFrom rdf:resource="#Don_Giovanni"/>
	<owl:differentFrom rdf:resource="#Nozze_di_Figaro"/>
</Opera>
```

```xml
<Persona rdf:ID="armando">  
	<owl:differentFrom rdf:resource="#manuel" />
	<owl:differentFrom rdf:resource="#andrea" />
</Persona>

<owl:Thing rdf:ID="manuel">
	<owl:differentFrom rdf:resource="#andrea" />
</owl:Thing>

<owl:Thing rdf:ID="andrea" />
```

# All Different
Nei casi in cui le risorse **distinte** sono molto è sconveniente usare `owl:differentFrom`.
OWL offre quindi la classe `owl:AllDifferent`, nella quale è definita la [[#Object Property|object property]] `owl:distinctMembers`.
La proprietà `owl:distinctMembers` mette in relazione entità della classe `owl:AllDifferent`, vincolando che sono differenti risorse, (come [[#Different From|owl:differentFrom]]).

```xml
<owl:AllDifferent>
	<owl:distinctMembers rdf:parseType="Collection">
		<Opera rdf:about="#Don_Giovanni" />
		<Opera rdf:about="#Nozze_di_Figaro" />
		<Opera rdf:about="#Cosi_fan_tutte" />
		<Opera rdf:about="#Tosca" />
	    <Opera rdf:about="#Turandot" />
	    <Opera rdf:about="#Salome" />
	</owl:distinctMembers>
</owl:AllDifferent>
```

```xml
<owl:AllDifferent>
	<owl:distinctMembers rdf:parseType="Collection">
		<owl:Thing rdf:about="#armando" />
		<owl:Thing rdf:about="#manuel" />
		<owl:Thing rdf:about="#andrea" />
	</owl:distinctMembers>
</owl:AllDifferent>
```

# Descrizione di classi
Ci sono molteplici modi per definire una [[#Classi|classe]] in OWL.
-   In maniera implicita come se fosse una risorsa, usando un **IRI**.
-   In maniera esplicita, enumerando in maniera esaustiva l'insieme delle sue istanza.
-   Definendo una **restrizione** sulle sue proprietà.
-   Tramite **operatori insiemistici** tra classi (unione, intersezione e complemento).

## Descrizione tramite nome
Il modo più semplice per definire una classe è in maniera implicita, come se fosse una risorsa
```xml
<owl:Class rdf:ID="Human"
```

```turtle
<Human> rdf:type owl:Class .
```

In DL $$\text{Human}$$

## Descrizione tramite enumerazione
Un altro per definire una classe è tramite un'**enumerazione** esaustiva di tutte le sue istanze.
```xml
<owl:Class rdf:ID="Continente">
	<owl:oneOf rdf:parseType="Collection">
		<owl:Thing rdf:about="#Europa"/>
		<owl:Thing rdf:about="#Africa"/>
		<owl:Thing rdf:about="#Asia"/>
		<owl:Thing rdf:about="#America"/>
		<owl:Thing rdf:about="#Oceania"/>
		<owl:Thing rdf:about="#Antartide"/>
	</owl:oneOf>
</owl:Class>
```

```turtle
:Continente rdf:type owl:Class ;
	owl:oneOf (:Europa :Africa :Asia :America :Oceania :Antartide) .
```

In DL $$\lbrace \text{Europa, Africa, Asia, America, Oceania, Antartide} \rbrace$$

## Descrizione tramite restrizioni su proprietà
Definiamo una classe come l'insieme di tutti gli individui che soddisfano certe restrizioni sull’uso di una proprietà.
Abbiamo due tipi di vincoli:
- vincoli sul **valore** di una proprietà.
- vincoli sulla **cardinalità** di una proprietà.

### Restrizioni sul valore
#### Quantificatore universale owl:allValuesFrom
Possiamo definire una classe come tutte quelle istanze tali che i valori di una certa proprietà (nell'esempio `hasParent`) appartengono **tutte** a una classe o datatype specificato.
```xml
<!-- tutte le istanze di quegli individui i cui genitori sono tutti umani -->
<owl:Restriction>
	<owl:onProperty rdf:resource="#hasParent"/>
	<owl:allValueFrom rdf:resource="#Human"/>
</owl:Restriction>

<!-- equivalente a RDFS -->
<rdfs:Class>
	<rdfs:Property rdf:ID="hasParent">
		<rdfs:range rdf:resource="#Human"/>
	</rdfs:Property>
</rdfs:Class>
```

```turtle
:x owl:equivalentClass [
	rdf:type owl:Restriction ;
	owl:onProperty :hasParent ;
	owl:allValuesFrom :Human
] .
```

In DL $$\forall \text{ hasParent } . \text{ Human}$$
#### Quantificatore esistenziale owl:someValuesFrom
Possiamo definire una classe come tutte quelle istanze tali che i **alcuni** valori di una certa proprietà (nell'esempio `hasParent`) appartengono a una classe o datatype specificato.
```xml
<!-- un insieme di tutte quelle istanze tali che hano almeno un genitore fisico -->
<owl:Restriction>
	<owl:onProperty rdf:resource="#hasParent"/>
	<owl:someValuesFrom rdf:resource="#Physician"/>
</owl:Restriction>

<!-- non esiste equivalente in RDFS -->
```

```turtle
:x owl:equivalentClass [
	rdf:type owl:Restriction ;
	owl:onProperty :hasParent ;
	owl:someValuesFrom :Physician
] .
```

In DL $$\exists \text{ hasParent } . \text{ Physician}$$

#### Uguaglianza sui valori
Possiamo definire una classe come tutte quelle istanze tali che una certa proprietà (nell'esempio `hasParent`) ha almeno un valore semanticamente uguale a quello indicato (nel nostro esempio `clinton`).
```xml
<owl:Restriction>  
	<owl:onProperty rdf:resource="#hasParent" />
	<owl:hasValue rdf:resource="#clinton" />
</owl:Restriction>
```

```turtle
:x owl:equivalentClass [
	rdf:type owl:Restriction ;
	owl:onProperty :hasParent ;
	owl:hasValue :clinton
] .
```

In DL $$\text{hasParent} \ni \text{clinton}$$


### Restrizioni sulla cardinalità

