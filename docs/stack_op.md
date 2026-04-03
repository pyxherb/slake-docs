# Stack Operations

$$
\frac{
    \Gamma\vdash \mathrm{T}\ \mathrm{type}
    \quad
    \text{\_\_stack\_depth}(\mu) + \text{\_\_sizeof}(\mathrm{T}) < \text{\_\_stack\_max}(\mu)
}{
    \text{alloca}\ \mathrm{T} \mid \mu \rightarrow \text{\_\_alloca}(\mu, \mathrm{T}) \mid \mu
}
$$

$$
\frac{
    \Gamma\vdash \mathrm{T}\ \mathrm{type}
    \quad
    \text{\_\_stack\_depth}(\mu) + \text{\_\_sizeof}(\mathrm{T}) \ge \text{\_\_stack\_max}(\mu)
}{
    \text{alloca}\ \mathrm{T} \mid \mu \rightarrow \text{throw}\ \text{new}\ \text{StackOverflowError}() \mid \mu
}
$$
