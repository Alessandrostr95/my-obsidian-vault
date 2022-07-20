$$
\begin{align*}
\text{expr} &\to \text{sub\_expr} \;\; \text{resolve\_row} \;\; \text{expr} \;\; \vert \;\; \text{sub\_expr} \;\; \text{resolve\_row} \;\; \text{final}\\

\\

\text{sub\_expr} &\to \text{row} \;\; \vert \;\; \text{row} \;\; \text{sub\_expr}\\

\\

\text{row} &\to \text{numb} \;\; \text{operator}\\
\text{operator} &\to + \; \vert \; - \; \vert \; \times \; \vert \; \div \\
\text{numb} &\to \text{atom} \;\; \cdot \; \text{atom}\\
\text{atom} &\to 0 \; \vert \; 1 \; \vert \; 2 \; \vert \; 3 \; \vert \; 4 \; \vert \; 5 \; \vert \; 6 \; \vert \; 7 \; \vert \; 8 \; \vert \; 9\\

\\

\text{resolve\_row} &\to 0 \cdot 0 \;\; =\\
\text{final} &\to \text{numb} \;\; *
\end{align*}
$$