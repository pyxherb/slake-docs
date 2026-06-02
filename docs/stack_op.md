# Stack Operations

## Auxiliary Definitions

$$
\frac{
    \text{\_\_stack\_depth}(\sigma) + \text{\_\_stack\_align\_diff}(\sigma, \mathrm{T}) + \text{\_\_sizeof}(\mathrm{T}) < \text{\_\_stack\_max}(\sigma)
}{
    \text{CheckStackBound}(\sigma, \mathrm{T}) = \text{true}
}
$$

$$
\frac{
    \text{\_\_stack\_depth}(\sigma) + \text{\_\_stack\_align\_diff}(\sigma, \mathrm{T}) + \text{\_\_sizeof}(\mathrm{T}) \ge \text{\_\_stack\_max}(\sigma)
}{
    \text{CheckStackBound}(\sigma, \mathrm{T}) = \text{false}
}
$$

## Structure Stack Allocation

$$
\frac{
    \mathrm{T} <: \mathrm{struct}
    \quad
    \text{CheckStackBound}(\sigma, \mathrm{T}) = \text{true}
    \quad
    \text{\_\_alloca}(\mathrm{T}, \sigma) \mid \gamma \mid \sigma \mid \mu \mid s \mid m \mid U
    =
    l \mid \gamma \mid \sigma' \mid \mu \mid s \mid m \mid U
}{
    \text{alloca}\ \mathrm{T} \mid \gamma \mid \sigma \mid \mu \mid s \mid m \mid U
    \rightarrow
    l \mid \gamma \mid \sigma'[l \mapsto \text{\_\_defaultof}(\mathrm{T})] \mid \mu \mid s \mid m \mid U
}
$$

$$
\frac{
    \mathrm{T} <: \mathrm{struct}
    \quad
    \text{CheckStackBound}(\sigma, \mathrm{T}) = \text{false}
}{
    \text{alloca}\ \mathrm{T} \mid \gamma \mid \sigma \mid \mu \mid s \mid m \mid U
    \rightarrow
    \text{throw}\ \text{new}\ \text{StackOverflowError}() \mid \gamma \mid \sigma \mid \mu \mid s \mid m \mid U
}
$$
