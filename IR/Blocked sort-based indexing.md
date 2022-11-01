Per piccole collezioni di documenti possiamo costruire un intero [[Inverted Index|indice]] in memoria.
Però generalmente non capita mai di essere in grado di farlo, perché le collezioni di documenti sono troppo grandi, e di conseguenza c'è un numero enorme di termini nell'indice.
Per questo abbiamo bisongo di utilizzare il disco come memoria di appoggio.

```ad-note
Ovviamente stiamo assumendo che riusciamo a tenere per intero l'indice su disco.
Questo però non è sempre vero.
```

Un primo passo per rendere la costruzione dell'indice più efficiente è quella di rappresentare i termini come **termID** numerici, anzichè come stringhe.
Possiamo costruire la mappa `termine->termID` "*on the fly*" man mano che processiamo i documenti e costruiamo l'indice.
Oppure possiamo farlo con un approccio in "*2-fasi*", ovvero prima processiamo la mappa `termine->termID` e poi costruiamo l'indice.

Usiamo come dataset d'esempio

Symbol | Statistic | Value
:---:|---|---
$N$| number of documents | $800,000$
$T_{avg}$| avg. number of tokens per document | $200$
$M$ | terms | $400,000$
$T$ | number of tokens in the entire collection | $100,000,000$
\ | avg. # bytes per token (incl. spaces/punct.) | $6$
\ | avg. # bytes per token (without spaces/punct.) | $4.5$
\ | avg. # bytes per term | $7.5$

Dato che staimo assumendo che la memoria RAM non è sufficiente per costruire l'indice per intero, abbiamo bisogno di un **external sorting algorithm**, ovvero un algoritmo che usi il disco come memoria asuliaria.

```ad-important
Ovviamente per avere prestazioni accettabili, tale agloritmo deve **minimizzare** il numero di accessi su disco e il numero di operazioni di **seek**, tenendo conto del fatto che operazioni di lettura/scrittura su aree di memoria sequenziali sono più efficienti di operazioni di seek (come [[Index construction#^31e9e5|già discusso]]).
```

Una prima soluzione è il **blocked sort-based indexing algorithm** o **BSBI**, il quale ha il seguente funzionamento:
1. **partiziona** la collezione di documenti in blocchi di dimensione uguale, in modo tale che ciascun blocco può essere caricato in memoria.
2. sequenzialmente, ordina le coppie `termID->[docID]` per ogni blocco, caricandone uno per volta in memoria.
3. man mano salva su disco i blocchi ordinati.
4. a coppie **fonde** i blocchi intermedi, finché non verrà generato l'indice completo finale.

```julia
mutable struct Buffer
	buffsize::Int
	content::AbstracVector{Char}
	function Buffer(buff_size::Int)
		return new(buff_size, Vector{Char}(undef, buff_size))
	end
end

mutable struct Document
	io::IOStream
	current_buffer::IOBuffer
	name::String
	function Document(fname::String)
		io = open(fname, "r+")
		#current_buffer = read(io) |> IOBuffer
		return new(io, IOBuffer(), fname)
	end
end

current_block(document::Document)::IOBuffer = document.current_buffer
parse_current_block(document::Document)::String = document |> current_block |> take! |> String

function next_block!(document::Document; size=1024)::Bool
	document.current_buffer = read(document.io, size) |> IOBuffer
	return document.current_buffer.size > 0
end

function bsbi_index_construction(documents::AbstracVector{Document}; buff_size=1024)
	files = File[]
	for document ∈ documents
		while next_block!(document, size=buff_size)
			parsed_block::String = parse_current_block(document)
			inverted_block::Vector{Pair} = bsbi_invert!(parsed_block) # to do
			f = write_block_to_disk(inverted_block)
			push!(files, f)
		end
	end
	merged_blocks_file = merge_blocks(files)
	return merged_blocks_file
end
```

