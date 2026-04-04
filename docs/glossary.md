# Glossary

$\text{\_\_name}$ - Built-in identifier

$\mathrm{T}$ - Non-nullable type

$\mathrm{T?}$ - Nullable type

$\Gamma$ - Type context

$\Gamma\vdash \mathrm{T}\ \mathrm{type}$ - In context $\Gamma$, $\mathrm{T}$ is a well-formed type.

$\Gamma\vdash t: \mathrm{T}$ - In context $\Gamma$, term $t$ has type $\mathrm{T}$

$\Gamma\triangleright\Gamma'$ - Context $\Gamma$ updates to $\Gamma'$

$\mathrm{A} <: \mathrm{B}$ - A is a subtype of B

$\mathrm{A} \not<: \mathrm{B}$ - A is not a subtype of B

$t_0 \triangleq t_1$ - $t_0$ is equivalent to/can be rewritten in $t_1$

$s_0 \rightarrow s_1$ - Configuration $s_0$ is changed to $s_1$

$e$ - Configuration that included current expression/value $e$, for pure expressions.

$e \mid \gamma \mid \sigma \mid \mu$ - Configuration that included current expression/value $e$ with local variable bindings $\gamma$, stack $\sigma$ and global state $\mu$. This implies the operation does not change the coroutine state and the execution mode.

$e \mid \gamma \mid \sigma \mid \mu \mid s \mid m$ - Configuration that included current expression/value $e$ with local variable bindings $\gamma$, stack $\sigma$, global state $\mu$, coroutine state $s$ and the execution mode $m$, where $m \in \{\text{Normal}, \text{Coro}\}$, where $\text{Normal}$ represents executing in a normal/regular function, $\text{Coro}$ represents executing in a coroutine.

$GLB$ - Greatest lower bound, see [Type System](type_system.md).

$LUB$ - Least upper bound, see [Type System](type_system.md).