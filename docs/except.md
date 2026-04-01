# Exception

## Throw Expression

$$
ThrowExpr ::= \text{throw}\ Expr
$$

A throw expression can interrupt current control flow catch an exception value (throw an exception) and unwind:

$$
\frac{
}{
    e_1,...,e_n,(\text{throw}\ e_{err}),e_{n+2},...,e_{m} \rightarrow e_1, ..., e_n,\text{throw}\ e_{err}
}
(\text{E-ThrowInterrupt})
$$

Calling throw as a function will evaluate all of the arguments and discard their results and then throw an exception value:

$$
\frac{
}{
(\text{throw}\ e_{err})(e_1, e_2, e_3, ...) \rightarrow e_1,e_2,e_3,...,\text{throw}\ e_{err}
}
(\text{E-ThrowInvoke})
$$

When a `throw` expression is in a function call's argument list, the arguments on the right of the expression will not be evaluated but that which on the left of the expression will be, correspondingly, the function will not be called:

$$
\frac{
}{
    f(e_0, ..., e_n,\ (\text{throw}\ e_{err}), e_{n+2}, ..., e_m) \rightarrow e_0, ..., e_n, \text{throw}\ e_{err}
}
(\text{E-ThrowArgsInterrupt})
$$

Throwing a nested `throw` expression directly evaluates the inner expression:

$$
\frac{
}{
    \text{throw}\ (\text{throw}\ e) \rightarrow \text{throw}\ e
}
(\text{E-ThrowFromInner})
$$

The `throw` expression will always bring the type depends on the outer environment, note that the exception value of `throw` must be derived from `Exception`:

$$
\frac{
    \Gamma\vdash e:\mathrm{E} \quad \mathrm{E} <: \mathrm{Exception}
}{
    \Gamma\vdash \text{throw}\ e:\mathrm{T}
}
(\text{T-Throw})
$$

## Try and Catch Statement

$$
CatchHandler ::= \text{catch}\ \text{(}\ LetBinding\ \text{)}\ Stmt\\
FinalHandler ::= \text{final}\ Stmt\\
TryStmt ::= \text{try}\ Stmt\ CatchHandler^{+}\ FinalHandler^{?}
$$

User may use `try` and `catch` statement to handle the exception value thrown by the inner codes, and optionally use `final` handler to perform the final cleanups.
