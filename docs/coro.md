# Coroutine

$$
\text{collect\_vars}(S) = \begin{cases}
\{x\} &S = (\text{let}\ x: \mathrm{T} = e)\\

\bigcup^{n}_{i = 0}\{s_i\} &S = \{ s_0, s_1, ..., s_n\}\\

\text{collect\_vars}(s) &S = \text{if}(e) s\\

\text{collect\_vars}(s)\cup\text{collect\_vars}(t) &S = \text{if}(e) s\ \text{else}\ t\\

\text{collect\_vars}(s) &S = \text{while}(e) s\\

\text{collect\_vars}(s) &S = \text{do}\ s\ \text{while}(e)\\

\emptyset &\text{otherwise}

\end{cases}
\\
$$
