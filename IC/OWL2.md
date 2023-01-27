OWL2 introduce una serie di miglioramenti ad [[OWL]], tra cui l'inserimento di alcuni **syntactic sugar** e un **aumento dell'espressività** della sua logica.

-------
# SYNTACTIC SUGAR

## DisjointUnion
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

## DisjointClasses
In [[OWL]] sono definiti i costruitti
- [[Vocabolario OWL#All Different|owl:AllDifferent]] che consente di asserire che più di due individui sono **differenti** (a due a due).
- [[Vocabolario OWL#Disgiunzione|owl:disjointWith]]  
