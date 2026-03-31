# Glossary

$\text{\_\_name}$ - Built-in identifier

$\mathrm{T}$ - Non-nullable type

$\mathrm{T?}$ - Nullable type

$\Gamma$ - Type context

$\Gamma\vdash \mathrm{T}\ \mathrm{type}$ - In context $\Gamma$, $\mathrm{T}$ is a well-formed type.

$\Gamma\vdash t: \mathrm{T}$ - In context $\Gamma$, term $t$ has type $\mathrm{T}$

$\mathrm{A} <: \mathrm{B}$ - A is a subtype of B

$\mathrm{A} \not<: \mathrm{B}$ - A is not a subtype of B

$e \rightarrow v$ - $e$ is evaluated to be $v$

$e \mid \mu_0 \rightarrow v \mid \mu_1$ - $e$ is evaluated to be $v$ with state $\mu_0$ transfers to $\mu_1$

$GLB$ - Greatest lower bound, see [Type System](type_system.md).

$LUB$ - Least upper bound, see [Type System](type_system.md).