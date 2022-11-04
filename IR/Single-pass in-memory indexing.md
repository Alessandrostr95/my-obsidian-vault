Come abbiamo visto, [[Blocked sort-based indexing]] ha eccellenti proprietà di scalabilità, ma purtroppo ha lo [[Blocked sort-based indexing#Svantaggi|svantaggio]] che necessità di avere una struttura dati per il mapping `term->termID`.

Un'alternativa più scalabile è il **single-pass in-memory indexing** (o in breve **SPIMI**).

La prima ottimizzazione che propone **SPIMI** si basa sull'idea che non c'è bisogno di preservare un unico dizionario per tutti i blocchi, semplicemente ne crea uno nuovo per ogni blocco e scrive su disco quelli vecchi.
Purtroppo però, per effettuare il merge dei dizionari in seguito, non si possono usare i `termID` come chiavi del dizionario, bensì useremo i termini stessi.

```julia
dictionary = Dict{Int, Vector{Int}}()

function SPIMI_invert!(document::Document)
	global dictionary
	
	parsed_block::String = parse_current_block(document)
	tokens = parsed_block |> split .|> lowercase .|> string |> sort
	
	output_files = File[]
	
	for token ∈ tokens
		token = next!(tokens)
		if !IS_MEMORY_FULL
			if term(token) ∈ keys(dictionary)
				push!(dictionary[term(token)], id(document)) 
			else 
				dictionary[term(token)] = [id(document)]
			end
		else
			f = write_block_to_disk!(dictionary)
			push!(output_files, f)
			empty!(dictionary)
		end
	end
	
	f = write_block_to_disk!(output_file, dictionary)
	push!(output_files, f)
	return output_files
end


function spimi_index_construction(documents::AbstracVector{Document}; buff_size=1024)
	files = File[] # saves blocks as files
	for document ∈ documents
		while next_block!(document, size=buff_size)
			inverted_block = BSBI_invert!(document)
			output_files = write_block_to_disk(inverted_block)
			extend!(files, output_files)
		end
	end
	merged_blocks_file = merge_blocks(files)
	return merged_blocks_file
end
```

I vantaggi di tale ottimizzazione sono:
- l'algoritmo è più efficiente in termini di computazione, in quanto non è necessario effettuare operazioni di **sorting**.
- l'algoritmo è più efficiente in termini di spazio, in quanto
	1. non abbiamo bisogno di preservare una mappa `term->termID`, abbiamo direttamente il riferimento alla posting list per ogni termine.
	2. per termini molto frequenti, il [[Blocked sort-based indexing|BSBI]] scriveva un sacco di coppi `termID->docID`. Infatti, se il termine `the` è presente in tutti i documenti della mia collezione, prima di effettuare la fase merge avevamo che una coppia relativa a `the` per ogni documento. Nel **SPIMI** invece abbiamo direttamente la posting list associata al termine `the`.

```ad-note
Osserviamo che anche in questo caso l'ordine delle posting list ci è dato "*gratis*".
Infatti, enumerando i documenti in ordine di arrivo, abbiamo che basterà **appendere** un posting ad una posting list.
```