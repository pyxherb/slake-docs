# Statement

A single statement performs a (maybe nested) instruction included inside.

## Expression statement

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