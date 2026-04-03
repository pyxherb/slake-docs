# Statement

A single statement performs a (maybe nested) instruction included inside.

## Expression Statement

Expression statement evaluates the inner expression and then discards the result immediately after the expression is evaluated.

$$
\frac{
    e_1 \mid \mu_0 \rightarrow v_1 \mid \mu_1
}{
    e_1; \mid \mu_0 \rightarrow \text{void} \mid \mu_1
}
(\text{E-ExprStmt})
$$

$$
\frac{
    \Gamma\vdash t : \mathrm{T}
}{
    \Gamma\vdash t; : \mathrm{void}
}
(\text{T-ExprStmt})
$$

## While Statement

`while` statement is the simplest form of loop.

$$
\frac{
    e_1 \mid \mu_0 \rightarrow \text{true} \mid \mu_1
}{
    \text{while}(e_1)\ b \mid \mu_0 \rightarrow \{b\}; \text{while}(e_1)\ b \mid \mu_1
}
(\text{E-WhileStmtTrue})
$$

$$
\frac{
    e_1 \mid \mu_0 \rightarrow \text{false} \mid \mu_1
}{
    \text{while}(e_1)\ b \mid \mu_0 \rightarrow \text{void} \mid \mu_1
}
(\text{E-WhileStmtFalse})
$$

## Do-while Statement

Just like `while`, but `do`-`while` executes the body first.

$$
\frac{
}{
    \text{do}\ b\ \text{while}(e_1) \mid \mu \rightarrow \{b;\} \text{while}(e_1)\ b \mid \mu
}
(\text{E-DoWhileStmt})
$$

## Let Statement

Let statement defines one or more local variables.

$$
\frac{
    \Gamma\vdash t: \mathrm{T}
    \quad
    x \notin \text{dom}(\Gamma)
}{
    \Gamma\vdash \text{let}\ x = t;\ \triangleright\ \Gamma[\text{x}\mapsto\mathrm{T}]
}
$$

$$
\frac{
    \Gamma\vdash\mathrm{T}\ \mathrm{type}
    \quad
    \Gamma\vdash t: \mathrm{T}
}{
    \Gamma\vdash \text{let}\ x: \mathrm{T};\ \triangleright\ \Gamma[\text{x}\mapsto\mathrm{T}]
}
$$

$$
\frac{
    \Gamma\vdash t: \mathrm{T}
    \quad
    x \notin \text{dom}(\Gamma)
}{
    \Gamma\vdash \text{let}\ x: \mathrm{T} = t;\ \triangleright\ \Gamma[\text{x}\mapsto\mathrm{T}]
}
$$

$$
\frac{
    m = \text{Normal}
}{
    \text{let}\ x: \mathrm{T}; \mid \mu \mid \_ \mid m \rightarrow \text{void} \mid \mu[x \mapsto \text{\_\_defaultof}(\mathrm{T})] \mid \_ \mid m
}
$$

$$
\frac{
    m = \text{Normal}
    \quad
    e \mid \mu \mid \_ \mid m \rightarrow v \mid \mu' \mid \_ \mid m
}{
    \text{let}\ x: \mathrm{T} = e; \mid \mu \mid \_ \mid m \rightarrow \text{void} \mid \mu[x \mapsto v] \mid \_ \mid m
}
$$