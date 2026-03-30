# Statement

A single statement performs a (maybe nested) instruction included inside.

## Expression Statement

Expression statement evaluates the inner expression and then discards the result immediately after the expression is evaluated.

$$
\frac{
    e_1 \mid s_0 \rightarrow v_1 \mid s_1
}{
    e_1; \mid s_0 \rightarrow \text{void} \mid s_1
}
$$

$$
\frac{
    \Gamma\vdash t : \mathrm{T}
}{
    \Gamma\vdash t; : \mathrm{void}
}
$$

## While Statement

`while` statement is the simplest form of loop.

$$
\frac{
    e_1 \mid s_0 \rightarrow \text{true} \mid s_1
}{
    \text{while}(e_1)\ b \mid s_0 \rightarrow b; \text{while}(e_1)\ b \mid s_1
}
$$

$$
\frac{
    e_1 \mid s_0 \rightarrow \text{false} \mid s_1
}{
    \text{while}(e_1)\ b \mid s_0 \rightarrow \text{void} \mid s_1
}
$$

## Do-while Statement

Just like `while`, but `do`-`while` executes the body first.

$$
\frac{
}{
    \text{do}\ b\ \text{while}(e_1) \mid s \rightarrow b; \text{while}(e_1)\ b \mid s
}
$$