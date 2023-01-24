Purtroppo [[RDF]] non consente di definire le relazioni a un livello di astrazione più alta.
Per esempio non consente di specificare **categorie** di oggetti con le rispettive **proprietà**, **restrizioni** e **relazioni tra le categorie**.

**RDF Schema** (o **RDFS**) estende RDF, consentendo di definire **Classi**, **Sottoclassi**, **Prorpietà**, **Sottoproprietà**, **Vincoli**, **range**, **domini**, ...

Grazie a questa estensione, RDF può essere considerato a tutti gli effeti un linguaggio per la rappresentazione della conoscenza, perché consente descrivere le entità, le relazioni tra loro, e tutte le entità astratte con relativa proprietà e relazioni.

Il vocabolario principale di RDFS introduce i seguenti termini principali:
- **rdfs:Resource** Tutte le cose descritte attraverso espressioni RDF sono risorse e sono considerate istanze della classe rdfs:Resource. rdfs:Resource può essere vista una **superclasse** di tutte le classi.
- **rdfs:Class** Rappresenta il generico concetto di **tipo** o **categoria**. Le classi sono univocamente definite dall'insieme delle proprie istanze. Perciò due classi che hanno lo stesso insieme di stanze sono definite **equivalenti**.
- **rdf:type** Questo elemento esiste già nel vocabolario RDF, ma in RDFS lega le risorse alle categorie (classi) a cui appartengono. È analogo al costrutto **instanceOf** del design orientato agli oggetti (OO).
- **rdf:Property** Anche questo termine viene dal vocabolario RDF, e rappresenta il sottoinsieme di tutte le risorse RDF che sono proprietà di un'altra risorsa.
- **rdfs:subClassOf** Questa è una relazione di **incusione** tra class. Se $A$ è `rdfs:subClassOf` $B$ allora $A$ è un **sottoinsieme** di $B$. È una proprietà transitiva.
- **rdfs:subPropertyOf** Questa proprietà è usata per indicare che una proprietà è una specializzazione di un'altra proprietà.
- **rdfs:domain** Dichiara che tutte le risorse caratterizzate da una certa proprietà appartengono ad una data classe. In parole semplici indicano la classe **dominio** di una proprietà.
- **rdfs:range** Dichiara che tutti i valori che può assumere una certa proprietà appartengono ad una data classe. In parole semplici indicano la classe **codominio** di una proprietà.


Esistono anche dei termini utili per commentare/documentare una base di conoscenza, anche se non sono particolarmente utili per la semantica.
- **rdfs:comment**: il modo più generale per commentare qualcosa. In genere fornisce una definizione in linguaggio naturale della risorsa che la contiene.
- **rdfs:label**: fornisce (singoli) termini per descrivere una risorsa.
- **rdfs:seeAlso**: contiene un puntatore ad un'altra risorsa che contiene ulteriori informazioni circa il soggetto di tale proprietà
- **rdfs:isDefinedBy**: è una sottoproprietà di **rdfs:seeAlso** e punta alla risorsa che descrive la proprietà soggetto.

Per esempio, supponiamo di avere la classe `Person` con sottoclasse `Provider`.
La codifica in RDF/XML sarà
```xml
<rdfs:Class rdf:ID="Provider">
	<rdfs:subClassOf rdf:resource="#Person"/>
<rdfs:Class />
```

## Schema di esempio
![](./img/rdfs_1.png)

```turtle
@prefix : <http:/example.com/> .
@prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
@prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#> .

:Vehicle a rdfs:Class ;
	rdfs:subClassOf rdfs:Resource .

:LandVehicle a rdfs:Class ;
	rdfs:subClassOf :Vehicle .

:SeaVehicle a rdfs:Class ;
	rdfs:subClassOf :Vehicle .

:Hovercraft a rdfs:Class ;
	rdfs:subClassOf :LandVehicle, :SeaVehicle .

:numberOfEngines a rdfs:Property ;
	rdfs:domain :Hovercraft ;
	rdfs:range xsd:integer ;
	rdfs:comment "This property states how many engines the hovercraft has"@en .

:Company a rdfs:Class ;
	rdfs:subClassOf rdfs:Resource .

:producedBy a rdfs:Property ;
	rdfs:domain :Vehicle ;
	rdfs:range :Company ;
	rdfs:label "Vehicle Producer"@en .
```

# Limitazioni di RDFS
Purtroppo RDFS non cattura con precisione alcune caratteristiche importanti.

Innanzitutto non consente di definire in maniera **contestuale** i vincoli di range/domain.
```ad-example
Per esempio, consideriamo la proprietà `hasChild`.
Se applicata alla classe `Person` vorrei il vincolo che `range` sia `Person`.
Se invece applicata alla classe `Elephant` vorrei il vincolo che `range` sia `Elephant`.
Si potrebbe pensare di assegnare la superclasse `Animal` a `Elephant` e a `Person`, e dire che un `Aniaml` ha `hasChild` un altro `Animal`.
Purtroppo però potrei avere inconsistenze del tipo una persona che ha come figlio un elefante...
Perciò è necessario definire le due proprietà distinte `hasChildPerson` e `hasChildElephant`.
```

Dopodichè non da nessun vincolo **quantificativo** (esistenzialità/cardinalità).
```ad-example
Non si può dire che tutte le istanze di `Person` hanno una madre che è anche una `Person`, o che tutte le persone hanno esattamente due genitori.
```

Ancora, nessuna proprietà **transitive**, **inverse** or **simmetriche**.
```ad-example
- Non si può affermare che la proprietà `isPartOf` è una proprietà **transitiva**: se $A$ ha un parte $B$, e $B$ ha una parte $C$, allora $A$ ha come parte anche $C$.

- Non si può nemmeno dire che `hasPart` è l'**inversa** di `isPartOf`: se $A$ ha come parte $B$, allora $B$ è una parte di $A$.

- Non si può definire che la proprietà `touches` è **simmetica**: se $A$ è in contatto con $B$ allora anche $B$ è in contatto con $A$.
```
