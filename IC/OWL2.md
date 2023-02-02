OWL2 introduce una serie di miglioramenti ad [[OWL]], tra cui l'inserimento di alcuni **syntactic sugar** e un **aumento dell'espressività** della sua logica.

-------
# SYNTACTIC SUGAR

## Disjoint Union
Consideriamo la classe `:CarDoor`.
Le porte delle macchine si possono suddividere in `:FrontDoor` (*porta anteriore*), `:RearDoor` (*porta posteriore*) e `:TrunkDoor` (*porta del bagliaio*).
Ovviamente queste tre tipologie di porte **partizionano** la classe `:CarDoor`, perciò:
- $$(\text{FrontDoor } \sqcup \text{ RearDoor } \sqcup \text{ TrunkDoor}) \equiv \text{CarDoor}$$
- $$(\text{FrontDoor } \sqcap \text{ RearDoor}) \equiv \perp$$
- $$(\text{FrontDoor } \sqcap \text{ TrunkDoor}) \equiv \perp$$
- $$(\text{RearDoor } \sqcap \text{ TrunkDoor}) \equiv \perp$$


Questo concetto in [[OWL]] è espresso come
```turtle
:CarDoor rdf:type owl:Class;
	owl:equivalentClass [
		rdf:type owl:Class ;
		owl:unionOf (:FrontDoor :RearDoor :TrunkDoor)
	] .

:FrontDoor rdf:type owl:Class ;
	owl:disjointWith :RearDoor, :TrunkDoor .

:RearDoor a owl:Class ;
	owl:disjointWith :FrontDoor, :TrunkDoor .

:TrunkDoor a owl:Class ;
	owl:disjointWith :FrontDoor, :RearDoor .
```

```xml
<owl:Class rdf:ID="CarDoor">
	<owl:equivalentClass>
        <owl:Class>
			<owl:unionOf rdf:parseType="Collection">
				<owl:Class rdf:about="#FrontDoor" />
				<owl:Class rdf:about="#RearDoor" />
				<owl:Class rdf:about="#TrunkDoor" />
			</owl:unionOf>
		</owl:Class>
	</owl:equivalentClass>
</owl:Class>

<owl:Class rdf:ID="FrontDoor">
	<owl:disjointWith rdf:resource="#RearDoor" />
	<owl:disjointWith rdf:resource="#TrunkDoor" />
</owl:Class>

<owl:Class rdf:ID="RearDoor">
	<owl:disjointWith rdf:resource="#FrontDoor" />
	<owl:disjointWith rdf:resource="#TrunkDoor" />
</owl:Class>

<owl:Class rdf:ID="TrunkDoor">
	<owl:disjointWith rdf:resource="#FrontDoor" />
	<owl:disjointWith rdf:resource="#RearDoor" />
</owl:Class>
```

Usando quindi i costrutti [[Vocabolario OWL#Unione|owl:unionOf]] e [[Vocabolario OWL#Disgiunzione|owl:disjointWith]].
In OWL2 tutto ciò può essere semplificato col costrutto `owl:disjointUnionOf`.

```turtle
:CarDoor rdf:type owl:Class ;
	owl:disjointUnionOf (:FrontDoor :RearDoor :TrunkDoor) .
```

```xml
<owl:Class rdf:ID="CarDoor">
	<owl:disjointUnionOf rdf:parseType="Collection">
		<owl:Class rdf:about="#FrontDoor" />
		<owl:Class rdf:about="#RearDoor" />
		<owl:Class rdf:about="#TrunkDoor" />
	</owl:disjointUnionOf>
</owl:Class>
```

## Disjoint Classes
In [[OWL]] sono definiti i costruitti
- [[Vocabolario OWL#All Different|owl:AllDifferent]] che consente di asserire che più di due individui sono **differenti** (a due a due).
- [[Vocabolario OWL#Disgiunzione|owl:disjointWith]]  

OWL2 introduce il costrutto `owl:AllDisjointClasses` per indicare che un insieme di *classi* sono disgiunte tra loro, a due a due.
```turtle
_:x rdf:type owl:AllDisjointClasses ;
	owl:members ( :UpperLobeOfLung :MiddleLobeOfLung :LowerLobeOfLung ) .
```

```xml
<owl:AllDisjointClasses>
	<owl:members rdf:parseType="Collection">
		<owl:Class rdf:about="#UpperLobeOfLung" />
		<owl:Class rdf:about="#MiddleLobeOfLung" />
		<owl:Class rdf:about="#LowerLobeOfLung" />
	</owl:members>
</owl:AllDisjointClasses>
```

## Negative Property Assertion
### Negative Object Property Assertion
OWL2 introduce la **Negative Object Property Assertion**, la quale asserisce esplicitamente che un dato individuo $w$ **non** è collegato al dato individuo $z$ tramite una certa [[Vocabolario OWL#Object Property|object property]] $y$.

```turtle
_:x rdf:type owl:NegativePropertyAssertion ;
	owl:sourceIndividual w ;
	owl:assertionProperty y ;  
	owl:targetIndividual "z"^^ .
```

```xml
<owl:NegativePropertyAssertion>
	<owl:sourceIndividual rdf:resource="#w" />
	<owl:assertionProperty rdf:resource="#y" />
	<owl:targetIndividual rdf:resource="#z" />
</owl:NegativePropertyAssertion>
```

### Negative Data Property Assertion
La **Negative Data Property Assertion** invece asserisce esplicitamente che un dato individuo $w$ non ha un dato valore $z$ per una data [[Vocabolario OWL#Datatype Property|datatype property]] $y$.
```turtle
_:x rdf:type owl:NegativePropertyAssertion ;
	owl:sourceIndividual w ;
	owl:assertionProperty y ;  
	owl:targetValue "z"^^xsd:string .
```

```xml
<owl:NegativePropertyAssertion>
	<owl:sourceIndividual rdf:resource="#w" />
	<owl:assertionProperty rdf:resource="#y" />
	<owl:targetValue rdf:datatype="&xsd;string">
		z
	</owl:targetValue>
</owl:NegativePropertyAssertion>
```

-----------
# Expressiveness Improvements
## Self-restriction
OWL2 consente di descrivere la **classe anonima** (mediante [[Vocabolario OWL#Descrizione tramite restrizioni su proprietà|descrizione tramite restrizioni su proprietà]]) di tutte quelle entità che sono collegate a se stesse attraverso una certa proprietà.
Queste classi anonime possono poi essere usate negli [[Vocabolario OWL#Assiomi sulle classi|assiomi sulle classi]] come segue:
```turtle
:AutoRegulatingProcess rdfs:subClassOf [  
	a owl:Restriction ;
	owl:onProperty :regulate ;
	owl:hasSelf "true"^^xsd:boolean
	].
```

```xml
<owl:Class rdf:ID="AutoRegulatingProcess">
	<rdfs:subClassOf>
		<owl:Restriction>
			<owl:onProperty rdf:resource="regulate" />
			<owl:hasSelf rdf:datatype="&xsd;boolean">
				true
			</owl:hasSelf>
		</owl:Restriction>
	</rdfs:subClassOf>
</owl:Class>
```

## Qualified Cardinality Restrictions
In OWL2, una **qualified cardinality restriction** consente di esprimere la classe di tutte quelle entità che hanno <u>per una data proprietà</u>, **almeno**, **al più** o **esattamente** un certo numero di valori appartententi a una data classe/tipo.

```turtle
:Vehicle owl:equivalentClass [
	rdf:type owl:Restriction ;  
	owl:onProperty :wheels ;
	owl:minQualifiedCardinality "2"^^xsd:nonNegativeInteger ;
	owl:onClass :Wheel
] .

:Human owl:equivalentClass [
	rdf:type owl:Restriction ;  
	owl:onProperty :hasParent ;
	owl:qualifiedCardinality "2"^^xsd:nonNegativeInteger ;
	owl:onClass :Human
] .

:Human owl:equivalentClass [
	rdf:type owl:Restriction ;  
	owl:onProperty :nTooth ;
	owl:maxQualifiedCardinality "32"^^xsd:nonNegativeInteger ;
	owl:onDataRange xsd:nonNegativeInteger
] .
```

Mentre in OWL1 avevamo [[Vocabolario OWL#Massima cardinalità|owl:maxCardinality]], [[Vocabolario OWL#Minima cardinalità|owl:minCardinality]] e [[Vocabolario OWL#Specifica cardinalità|owl:cardinality]] che spicificavano solamente la restrizione sul numero delle relazioni, la **qualified cardinality restriction** introduce un vincolo anche sul tipo/classe.

## Disjoint Properties
In [[OWL|OWL1]] è possibile asserire che due classi sono [[Vocabolario OWL#Disgiunzione|disgiunte]].
In OWL2 è possibile asserire lo stesso anche per *proprietà*.

Per esempio abbiamo la classe `:Evento` con due relazioni verso la classe `:Time` per gli orari di inizio e di fine, rispettivamente `:startTime` e `:endTime`.
Ovviamente vogliamo che le relazioni `:startTime` ed `:startTime` siano **disgiunte**, ovvero ciò che è un tempo di inizio non può essere un tempo di fine, e viceversa.

```turtle
:Time rdf:type owl:Class .

:startTime rdf:type rdfs:Property ;
	rdfs:domain :Eveno ;
	rdfs:range :Time .

:endTime rdf:type rdfs:Property ;
	rdfs:domain :Eveno ;
	rdfs:range :Time .

:startTime owl:propertyDisjointWith :endTime .

:Evento owl:equivalentClass [
	rdf:type owl:Class ;
	owl:unionOf (
		[
			rdf:type owl:Restriction ;  
			owl:onProperty :startTime ;
			owl:cardinality "1"^^xsd:nonNegativeInteger 
		]
		[
			rdf:type owl:Restriction ;  
			owl:onProperty :endTime ;
			owl:cardinality "1"^^xsd:nonNegativeInteger 
		]
	)
] .
```

```xml
<owl:Class rdf:ID="Time" />

<rdfs:Property rdf:ID="startTime" >
	<rdfs:domain rdf:resource="#Evento" />
	<rdfs:range rdf:resource="#Time" />
	<owl:propertyDisjointWith rdf:resource="#endTime" />
</rdfs:Property>

<rdfs:Property rdf:ID="endTime" >
	<rdfs:domain rdf:resource="#Evento" />
	<rdfs:range rdf:resource="#Time" />
	<owl:propertyDisjointWith rdf:resource="#startTime" />
</rdfs:Property>

<owl:Class rdf:ID="Evento">
	<owl:equivalentClass>
		<owl:Class>
			<owl:unionOf rdf:parseType="Collection">
				<owl:Restriction>
					<owl:onProperty rdf:resource="#startTime" />
					<owl:cardinality rdf:datatype="&xsd;nonNegativeInteger">
						1
					</owl:cardinality>
				</owl:Restriction>
				<owl:Restriction>
					<owl:onProperty rdf:resource="#endTime" />
					<owl:cardinality rdf:datatype="&xsd;nonNegativeInteger">
						1
					</owl:cardinality>
				</owl:Restriction>
			</owl:unionOf>
		</owl:Class>
	</owl:equivalentClass>
</owl:Class>
```

## Riflessibilità, Irriflessibilità e Asimmetria
In OWL2 è stata aggiunta la possibilità definire una proprietà **riflessiva** o **irriflessiva**.

Se ogni per esempio ogni elemento $x$ è `parte-di` se stesso, allora trale proprietà **riflessiva**.
```turtle
:part_of rdf:type owl:ReflexiveProperty  .
```

Se invece $x$ può essere parte di un'altra entità $y$ ma <u>non</u> di se stesso, allora la proprietà è **irriflessiva**.
```turtle
:part_of rdf:type owl:IrreflexiveProperty  .
```

Come in OWL1 è definita la [[Vocabolario OWL#Simmetria|simmetria]], in OWL2 una proprietà può essere **antisimmetrica**.
Per esempio, prendiamo la classe `:Scatola` e la relazione `:contiene`.
Se $x$ contiene $y$ non è vero che $y$ contiene $x$.
```turtle
:contiene rdf:type owl:AsymmetricProperty   .
```

```ad-note
Ovviamente se una proprietà è **antisimmetrica** allora è anche **irriflesivva**.
```


## Property Chain
In OWL2 è possibile definire una proprietà come la composizione di altre proprietà.

Per esempio, se $x$ ha come padre $y$ e $y$ ha come padre $z$, allora $x$ ha come nonno $z$.
```turtle
:hasGrandFather owl:propertyChainAxiom ( :hasFather :hasFather ) .
```

Oppure se $x$ è locato in $y$ e $y$ è parte di $z$, allora $x$ è anche locato in $z$.
```turtle
:locatedIn owl:propertyChainAxiom ( :locatedIn :partOf ) .
```

## Keys
In OWL2 è possibile associare ad una classe un'insieme di proprietà che identificano **univocamente** le porprie istanze.
Tali proprietà sono anche dette **chiavi**.

Per esempio sappiamo che gli studenti sono univocamente identificati dalla loro matricola.
```turtle
:Studente rdf:type owl:Class ;
	owl:Keys ( :matricola ) .
```

-----
## Altro
In OWL2 le espressioni che definiscono le proprietà possono incluse direttamente nell'espressione che definisce una classe.

```turtle
:KnownByMark owl:equivalentClass [
	rdf:type owl:Restriction ;
	owl:onProperty [   # proprietà anonima
		owl:inverseOf :knows
	];
	owl:hasValue :mark
] .
```