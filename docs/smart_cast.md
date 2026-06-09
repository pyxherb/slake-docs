# Smart Casting

## Assignments

<!--Incompleted yet.-->

$$
\frac{
    \Gamma,\Sigma\vdash e: \mathrm{T?}
    \quad
    \Gamma \vdash x: \mathrm{T?}
}{
    \llbracket x = \text{e} \rrbracket(\Gamma, \Sigma) = \Sigma[x \mapsto \mathrm{T?}]
}
$$

$$
\frac{
    \Gamma,\Sigma\vdash e: \mathrm{T}
    \quad
    \Gamma \vdash x: \mathrm{T?}
}{
    \llbracket x = \text{e} \rrbracket(\Gamma, \Sigma) = \Sigma[x \mapsto \mathrm{T}]
}
$$
