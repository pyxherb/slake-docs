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
    \text{while}(e_1)\ b \mid \mu_0 \rightarrow b; \text{while}(e_1)\ b \mid \mu_1
}
(\text{E-WhileTrue})
$$

$$
\frac{
    e_1 \mid \mu_0 \rightarrow \text{false} \mid \mu_1
}{
    \text{while}(e_1)\ b \mid \mu_0 \rightarrow \text{void} \mid \mu_1
}
(\text{E-WhileFalse})
$$

## Do-while Statement

Just like `while`, but `do`-`while` executes the body first.

$$
\frac{
}{
    \text{do}\ b\ \text{while}(e_1) \mid \mu \rightarrow b; \text{while}(e_1)\ b \mid \mu
}
(\text{E-DoWhile})
$$