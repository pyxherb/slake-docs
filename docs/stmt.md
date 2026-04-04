# Statement

A single statement performs a (maybe nested) instruction included inside.

## Expression Statement

Expression statement evaluates the inner expression and then discards the result immediately after the expression is evaluated.

$$
\frac{
    e_1 \mid \mu_0 \rightarrow v_1 \mid \mu_1
}{
    e_1;\ \overline{s} \mid \mu_0 \rightarrow \overline{s} \mid \mu_1
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
    \text{while}(e_1)\ b\ \overline{s} \mid \mu_0 \rightarrow \{b\}; \text{while}(e_1)\ b\ \overline{s} \mid \mu_1
}
(\text{E-WhileStmtTrue})
$$

$$
\frac{
    e_1 \mid \mu_0 \rightarrow \text{false} \mid \mu_1
}{
    \text{while}(e_1)\ b\ \overline{s} \mid \mu_0 \rightarrow \overline{s} \mid \mu_1
}
(\text{E-WhileStmtFalse})
$$

## Do-while Statement

Just like `while`, but `do`-`while` executes the body first.

$$
\frac{
}{
    \text{do}\ b\ \text{while}(e_1);\ \overline{s} \mid \mu \rightarrow \{b;\} \text{while}(e_1)\ b;\ \overline{s} \mid \mu
}
(\text{E-DoWhileStmt})
$$

## Let Statement

Let statement defines one or more local variables.

$$
\frac{
    \Gamma\vdash\mathrm{T}\ \mathrm{type}
    \quad
    x \notin \text{dom}(\Gamma)
    \quad
    \Gamma, x: \mathrm{T}\vdash \overline{s}: \mathrm{void}
}{
    \Gamma\vdash \text{let}\ x: \mathrm{T}; \overline{s}: \mathrm{void}
}
(\text{T-LetTyped})
$$

$$
\frac{
    \Gamma\vdash t: \mathrm{T_1}
    \quad
    x \notin \text{dom}(\Gamma)
    \quad
    \Gamma, x: \mathrm{T_2}\vdash \overline{s}: \mathrm{void}
    \quad
    \mathrm{T_1} <: \mathrm{T_2}
}{
    \Gamma\vdash \text{let}\ x: \mathrm{T_2} = t; \overline{s}: \mathrm{void}
}
(\text{T-LetTypedInit})
$$

$$
\frac{
    \Gamma\vdash e: \mathrm{T}
}{
    \Gamma\vdash\text{let}\ x = e
    \triangleq
    \text{let}\ x: \mathrm{T} = e
}
$$

$$
\frac{
    m = \text{Normal}
    \quad
    e \mid \mu \mid \_ \mid m \rightarrow e' \mid \mu' \mid \_ \mid m
    \quad
    e\ \text{is not a value}
}{
    \text{let}\ x: \mathrm{T} = e; \overline{s} \mid \mu \mid \_ \mid m \rightarrow \text{let}\ x: \mathrm{T} = e'; \overline{s} \mid \mu' \mid \_ \mid m
}
(\text{E-LetTypedInitReduce-Normal})
$$

$$
\frac{
    m = \text{Normal}
    \quad
    v\ \text{is a value}
}{
    \text{let}\ x: \mathrm{T} = v; \overline{s} \mid \mu \mid \_ \mid m \rightarrow \overline{s} \mid \mu[x \mapsto v] \mid \_ \mid m
}
(\text{E-LetTypedInit-Normal})
$$

$$
\frac{
    m = \text{Normal}
}{
    \text{let}\ x: \mathrm{T}; \overline{s} \mid \mu \mid \_ \mid m \rightarrow \text{let}\ x: \mathrm{T} = \text{\_\_defaultof}(); \overline{s} \mid \mu \mid \_ \mid m
}
(\text{E-LetTyped-Normal})
$$
