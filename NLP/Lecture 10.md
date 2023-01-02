---
date: 2022-11-17
draft: true
---
# Modello tradizionale
- Siamo nel contesto **context free** e vogliamo costruire un **parser**.

Vedi [CYK](https://www.xarg.org/tools/cyk-algorithm/)

```julia
# diagonale
"""
		0  1  2  3  4
	5 |__|__|__|__|__|
	4 |__|__|__|__|
	3 |__|__|__|
	2 |__|__|
	1 |__|
	    a  b  c  a  a 
"""
function CYK(n)
	for d=1:n
		for i=d:n
			j = i-d
			print("($i, $j) -> ")
			for s=(i-1):-1:(j+1)
				# do stuff
				print("[($i, $s)#($s, $j)], ")
			end
			println()
		end
	end
end
```

Il CYK vuole una grammatica in **Chomsly Normal Form**, però noi vogliamo fare il parsing di grammatiche Context Free.
Sappiamo che Context Free ed Chomsly Normal Form.

Purtroppo però, trasformare una grammatica CF in CNF potrebbe generare un albero delle interpretazioni che non è utile ai nostri scopi.

L'albero generato dal CYK (sulla grammatica CNF) è un albero **binario**.
Dobbiamo quindi applicare delle regole per generare l'albero CF, che non sempre è possibile recuperare per intero.