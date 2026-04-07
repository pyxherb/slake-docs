# Garbage Collection

$$
\begin{aligned}
&\text{gc}(\mu) = \\
&\quad \text{let}\ U = \text{\_\_unreachable}(\text{\_\_roots}(\mu), \mu)\\
&\quad \bold{if}\ U \ne \empty\ \bold{then}\\
&\quad\quad \text{let}\ L = \text{\_\_sort\_by\_dtor\_prio}(U)\\
&\quad\quad \mu'=\text{\_\_finalize}(L_0, \mu)\\
&\quad\quad \text{gc}(\mu')\\
&\quad \bold{else}\\
&\quad\quad \text{let}\ R = \text{\_\_restrict\_alive}(\mu, \text{\_\_reachable}(\text{\_\_roots}(\mu)))\\
\end{aligned}
$$