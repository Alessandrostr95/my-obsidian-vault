Come abbiamo visto, [[Blocked sort-based indexing]] ha eccellenti proprietà di scalabilità, ma purtroppo ha lo [[Blocked sort-based indexing#Svantaggi|svantaggio]] che necessità di avere una struttura dati per il mapping `term->termID`.

Un'alternativa più scalabile è il **single-pass in-memory indexing** (o in breve **SPIMI**).

La prima ottimizzazione che propone **SPIMI** si basa sull'idea che non c'è bisogno di preservare un unico dizionario per tutti i blocchi, semplicemente ne crea uno nuovo per ogni blocco e scrive su disco quelli vecchi.
Purtroppo però, per fare ciò, non si possono usare i `termID` nel dizionario, e quindi come chiavi useremo semplicemente i termini.

```julia
function SPIMI_Invert(token_stream::TokenStrem)
	output_file = File()
	dictionary = Dict{Term, Vector{Int}}()
	
	while FREE_MEMORY()
		token = next!(token_stream)
		
		postin_list = Int[]
		if term(token) ∈ keys(dictionary)
			postin_list = dictionary[term(token)]
		else 
			dictionary[term(token)] = postin_list
		end
		
		
	end
	
	sorted_terms = sort(keys(dictionary))
	write_block_to_disk!(output_file, dictionary, sorted_terms)
	return output_file
end
```