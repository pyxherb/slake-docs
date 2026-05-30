# Stack Operations

$$
\frac{
    \Gamma\vdash \mathrm{T}\ \mathrm{type}
    \quad
    \text{\_\_stack\_depth}(\mu) + \text{\_\_stack\_align\_diff}(\mu, \mathrm{T}) + \text{\_\_sizeof}(\mathrm{T}) < \text{\_\_stack\_max}(\mu)
}{
    \text{alloca}\ \mathrm{T} \mid \gamma \mid \sigma \mid \mu \mid U
    \rightarrow
    \text{\_\_alloca}(\mathrm{T}, \mu) \mid \gamma \mid \sigma \mid \mu \mid U
}
$$

$$
\frac{
    \Gamma\vdash \mathrm{T}\ \mathrm{type}
    \quad
    \text{\_\_stack\_depth}(\mu) + \text{\_\_stack\_align\_diff}(\mu, \mathrm{T}) + \text{\_\_sizeof}(\mathrm{T}) \ge \text{\_\_stack\_max}(\mu)
}{
    \text{alloca}\ \mathrm{T} \mid \gamma \mid \sigma \mid \mu \mid U
    \rightarrow
    \text{throw}\ \text{new}\ \text{StackOverflowError}() \mid \gamma \mid \sigma \mid \mu \mid U
}
$$
