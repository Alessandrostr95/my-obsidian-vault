# Binary Term-Document Incidence Matrix
Supponiamodi avere una grande collezione con tutte le opere di Shakespeare.
Supponiamo di voler identificare quali opere contengono le parole
- *Brutus* **AND** *Caesar*
- **NOT** *Calpurnia*

Un primo approccio è quello di leggere tutti i documenti, **selezionando** quelli che contengono le parole *Brutus* e *Caesar* e **scartando** quelli che contengono *Calpurnia*.
Questa alla fine è una sorta di **scansione lineare** tra i documenti, ritornando i soli documenti che rispettano l'**espressione logica** desiderata.
Questo processo è noto come **grep** dei documenti.

Finché siamo in un solo computer di un utente, il comando `grep` va più che bene.
Invece, su sistemi con un'enorme quantità di dati (come il *web*), si richiede di più:
1. Processare un'enorme quantità **molto** velocemente (<u>frazione di secondi</u>). E purtroppo sul web un approccio lineare come il *grepping* indurrebbe risultati disastrosi.
2. La possibilità di avere operazioni più sofisticati. Per esempio abbiamo l'espressione "*Romans* **NEAR** *countrymen*" dove l'operatore **NEAR** può significare "*entro 5 parole*" oppure "*nella stessa frase*". Questo operatore è impraticabile con il grepping.
3. Ottenere un **ranking** del risultato della query, in base alla "*significatibilità*" dei documenti rispetto a quanto richiesto.

Un primo modo per evitare uno **scanning lineare** dei documenti è quello di **indicizzare** a priori ogni documento.
Per ogni documento a disposizione identifichiamo l'inseme di tutte le parole, o **termini**.
Costruiamo poi una matrice $M \times N$ dove ad ogni riga è associato un termine, e ad ogni colonna un documento.
In tale matrice, poniamo la cella $(i,j)$ pari a $1$ se il termine $i$-esimo è contenuto nel documento $j$-esimo, $0$ altrimenti.
Questa è detta **matrice d'incidenza**.

\\ | **Antony and Cleopatra** | **Julius Caesar** | **The Tempest** | **Hamlet** | **Othello** | **Macbeth** | ...
---|---|---|---|---|---|---|---
**Antony** | 1 | 1 | 0 | 0 | 0 | 1 | ...
**Brutus** | 1 | 1 | 0 | 1 | 0 | 0 | ...
**Caesar** | 1 | 1 | 0 | 1 | 1 | 1 | ...
**Calpurnia** | 0 | 1 | 0 | 0 | 0 | 0 | ...
**Cleopatra** | 1 | 0 | 0 | 0 | 0 | 0 | ...
**mercy** | 1 | 0 | 1 | 1 | 1 | 1 | ...
**worser** | 1 | 0 | 1 | 1 | 1 | 0 | ...
... | ... | ... | ... | ... | ... | ... | ...

Perciò, per ottenere i documenti che rispettano l'espressione $$\text{Brutus } AND \text{ Caesar } AND \; NOT \text{ Calpurnia}$$ basta prendere la riga **indicizzata** dai termini `Brutus` e `Caesar`, poi prendere la riga indicizzata da `Calpurnia` ma **complementata**, per poi fare il **bitwise AND**
$$\begin{align}
&&110100\\
\land &&110111\\
\land &&101111\\
\hline
= &&100100
\end{align}$$
Alla fine i documenti interessati saranno quelli per i quali il relativo bit è pari ad 1, nel nostro caso i documenti `Antony and Cleopatra` e `Hamlet`.

Questo approccio è detto **Boolean retrieval model**.
In tale modello le query sono della forma di **espressioni booleane di termini**, dove i termini sono combinati tramite gli **opeartori logici** `AND`, `OR` e `NOT`.

```julia
M::BitMatrix

brutus, caesar, calpurnia = M[2,:], M[3,:], M[4,:]

return brutus .& caesar .& (.! calpurnia) 
```

## Efficienza - Tempo
L'applicazione di un **operatore** booleano è **lineare** nel numero $N$ di documenti.
La risoluzione di una espressione ha quindi una complessità di $O(kN)$, dove $k$ è il numero di operatori nella query.

Per quanto riguarda invece la ricerca delle righe necessarie per la queri è lineare nel numero di termini $M$.
Perciò abbiamo una complessità di $O(kNM)$.

Tale complessità non è per niente ragionevole, in quanto $M$ può crescere a dismisura.

Perciò si può pensare di **ordinare** i termini in fase di costruzione della matrice d'incidenza.
I tal caso si può effettuare una **ricerca dicotomica** sui termini, ottenendo una complessità di $O(kN\log{M})$ (decisamente più ragionevole).

## Efficienza - Spazio
Purtroppo in termini di **spazio** tale struttura dati non è praticabile.
Infatti per larghe collezioni di documenti la matrice quasi sicuramente potrebbe non entrare in RAM, mentre per piccole collezioni conviene fare il *grepping*.
Infatti, supponiamo di avere $N = 500K$ documenti ed $M = 1M$ di termini.
Se per ogni cella abbiamo bisogno di un bit, avremo una matrice grande $NM = 500,000,000,000$ di bit, ovvero $62,500,000,000B \approx 58GB$.

-------
# Inverted Index 
Un'osservazione che si può fare è che la [[#Binary Term-Document Incidence Matrix|matrice d'incidenza]] per molti **termini d'interesse** (nomi, luoghi, ecc...) è piena di zeri, ovvero è **sparsa**.

```ad-note
Certamente per molti termini, come gli articoli, ci sono molti 1.
Però generalemte non si fanno query in cui richiedo tutti i documenti con il termine `the`, perché certamente tutti i documenti saranno compresi nella risposta.
```

Perciò, quello che si può fare per ottimizzare, possiamo pensare di usare un **dizionario** di termini (detto anche **vocabolario** o **lexicon**), dove per ogni termine conserviamo una **lista di documenti** nei quali occorrono.
Tale lista è anche detta **posting list**.

![](./img/IR_boolean_retrieval_1.png)

```ad-important
Nella costruzione di un indice inverso, assumiamo che viene costruito in modo tale che sia i termini sia le loro posting lists sono mantenute **oridnate**.
```

```ad-note
Anche se in termini di efficienza è dispendioso mantenere l'ordinamento a fronte di **inserimenti** e **rimozioni**.

Però nell'almbito dell IR non si vuole gestire un numero elevato di inserimenti, rimozioni o update.
```


## Inverted Index Construction
### Tokenization
Data la sequanza di caratteri che compongono un documento, il task della **tokenizzazione** è il task di *"spezzare"* tale sequenza in pezzi (detti **toke**).
Generalmente questi token sono le parole che compongono le frasi.

- **Input**: *Friends, Romans, Countrymen, lend me your ears;*
- **Output**: `Friends`, `Romans`, `Countrymen`, `lend`, `me`, `your`, `ears`

Un **token** è un'istanza di una sequenza di caratteri in un particolare documento che sono raggruppati come un'utile unità semantica per l'elaborazione.
Per esempio *"Michael Jackson"* è meglio trattarlo come un **singolo token** `Michael Jackson`  perché è più utile al task di IR.

Un **tipo** è la classe di tutti i token che contengono la stessa sequenza di caratteri.
Nel nostro caso possiamo avere il tipo:
- `Michael Jackson`, `Michael jackson`, `michael Jackson`, `michael jackson`, `MICHAEL JACKSON`, ...

Un **termine** è invece un elemento di un tipo, generalmente **normalizzato**, il quale viene usato nel nostro motore di IR.

Il problema del task della tokenizzazione è:

> Qual è il miglio modo di generare token?

Consideriamo il seguente esempio

	Mr. O'Neill thinks that the boys' stories about Chile's capital aren't amusing.

Come trattiamo il cognome `O'Neil`?
Quale dei seguenti modi è più corretto?
- `neil`  
- `oneil`  
- `o'neil`  
- `o'`, `neil`  
- `o`, `neil`  

E per `aren't`?
- `aren't`
- `arent`
- `are`, `n't`
- `aren`, `t`

Oppure, se tokenizzo rimuovendo tutti i punti, potrebbe capitare che la parola "*U.S.A.*" venga decomposta in tre token inutili `u`, `s` e `a`.

Questo è un processo delicato, e bisogna tenere in considerazione tutti i casi particolare che si desidera gestire.

```julia
# my document
document::String = "Mr. O'Neill thinks that the boys' stories about Chile's capital aren't amusing."

# trivial tokenization
tokens::Vector{String} = document |> split |> (d -> replace.(d, "." => "", "'" => "")) .|> lowercase
```

### Dropping common terms: stop words
Ci sono termini che sono contenuti in **ogni** documento, come pre esempio gli articoli, le congiunzioni, le proposizioni, ecc...

Per esempio, se ricerco la frase *"President of the United States"* gli unici termini realmente utili alla ricerca sono *"President"* e *"United States"*.
Infatti, molto probabilmente la mia ricerca sarà un query del tipo $$\text{president } \land \text{ united states}$$
Perciò, possiamo pensare di rimuovere tutte quelle parole con altissima frequenza in una lingua, e che non hanno utilità ai fini di una ricerca.
Tali parole sono dette **stop words**.

```julia
stop_words = ["the", "that", "an", "a", "an", "at"]

filter!(t -> t ∉ stop_words, tokens)
```

### Normalization (equivalence classing of terms)
La **normalizzazione** dei token è il processo di *canonizzazione* dei token, ovvero si riducono i token si cerca di raggruppare token differenti che rappresentano una stessa cosa in un unico token.

Per esempio, io voglio che se ricerco `U.S.A.` oppure ricerco `USA` ottengo lo stesso risultato dal mio motore di IR.

Il modo più comune di standardizzare è quello di definire **classi di equivalenza** tra termini.
Se per esempio entrambi i termini `anti-discriminatory` e `antidiscriminatory` sono entrambi *mappati* nel termine `antidiscriminatory`, allora tutte le query che li comprendono avranno come risultato lo stesso insieme di documenti.

```julia
equivalen_class = Dict(
           "Windows" => ["Windows"],
           "windows" => ["Windows", "windows", "window"],
           "window" => ["window", "windows"],
           "u.s.a." => ["usa", "united states"],
           "usa" => ["usa", "united states"],
           "anti-discriminatory" => ["antidiscriminatory"],
           "antidiscriminatory" => ["antidiscriminatory"],
           ...
           )
```

### Stemming and Lemmatization
[TODO]

## Intersection
```julia
function is_sorted(a::AbstractVector{T})::Bool where T
	for i=1:length(a)-1
		a[i] > a[i+1] && return false
	end
	true
end
```

```julia
function intersect(
		L₁::AbstractVector{T},
		L₂::AbstractVector{T}
	)::AbstractVector{T} where T
	
	@assert is_sorted(L₁) && is_sorted(L₂) "Posting lists MUST be sorted!"
	
	result = T[]
	i, j = 1, 1
	while i ≤ length(L₁) && j ≤ length(L₂)
		if L₁[i] == L₂[j]
			push!(result, L₁[i])
			i += 1
			j += 1
		elseif L₁[i] < L₂[j]
			i += 1
		else
			j += 1
		end
	end
	
	result
end
```

## Union
```julia
function union(
		L₁::AbstractVector{T},
		L₂::AbstractVector{T}
	)::AbstractVector{T} where T
	
	@assert is_sorted(L₁) && is_sorted(L₂) "Posting lists MUST be sorted!"
	
	result = T[]
	i, j = 1, 1
	while i ≤ length(L₁) && j ≤ length(L₂)
		if L₁[i] == L₂[j]
			push!(result, L₁[i])
			i += 1
			j += 1
		elseif L₁[i] > L₂[j]
			push!(result, L₁[i])
			i += 1
		else
			push!(result, L₂[j])
			j += 1
		end
	end
	
	if i > length(L₁)
		append!(result, L₂[j:end])
	elseif j > length(L₂)
		append!(result, L₁[i:end])
	end
	
	result
end
```

## Difference
```julia
function difference(
		L₁::AbstractVector{T},
		L₂::AbstractVector{T}
	)::AbstractVector{T} where T
	
	@assert is_sorted(L₁) && is_sorted(L₂) "Posting lists MUST be sorted!"
	
	result = T[]
	i, j = 1, 1
	while i ≤ length(L₁) && j ≤ length(L₂)
		if L₁[i] < L₂[j]
			push!(result, L₁[i])
			i += 1
		elseif L₁[i] > L₂[j]
			j += 1
		else
			i += 1
			j += 1
		end
	end
	
	j > length(L₂) && append!(result, L₁[i:end])
	
	result
end
```

