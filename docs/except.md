# Exception

A throw expression can interrupt current control flow catch an exception value (throw an exception) and unwind:

$$
(\text{throw}\ e),x_1,x_2,x_3,... \rightarrow \text{throw}\ e
$$

$$
x_1,...,x_n,(\text{throw}\ e),x_{n+2},...,x_{m} \rightarrow x_1, ..., x_n,\text{throw}\ e
$$

Calling throw as a function will evaluate all of the arguments and discard their results and then throw an exception value:

$$
(\text{throw}\ e)(x_1, x_2, x_3, ...) \rightarrow x_1,x_2,x_3,...,\text{throw}\ e
$$

When a `throw` expression is in a function call's argument list, the arguments on the right of the expression will not be evaluated but that which on the left of the expression will be, correspondingly, the function will not be called:

$$
f(\text{throw}\ e) \rightarrow \text{throw}\ e
$$

$$
f(x_0, (\text{throw}\ e)) \rightarrow x_0, \text{throw}\ e
$$

$$
f(x_0, x_1, (\text{throw}\ e)) \rightarrow x_0, x_1, \text{throw}\ e
$$

$$
f(x_0, ..., x_n,\ (\text{throw}\ e)) \rightarrow x_0, ..., x_n, \text{throw}\ e
$$

$$
f(x_0, ..., x_n,\ (\text{throw}\ e), x_{n+2}, ..., x_m) \rightarrow x_0, ..., x_n, \text{throw}\ e
$$

$$
\frac{
x_1 \rightarrow x_2
}{
\text{throw}\ x_1 \rightarrow \text{throw}\ x_2
}
$$

Throwing a nested `throw` expression directly evaluates the inner expression:

$$
\text{throw}\ (\text{throw}\ x) \rightarrow \text{throw}\ x
$$

The `throw` expression will always bring the type depends on the outer environment, note that the exception value of `throw` must be derived from `Exception`:

$$
\frac{
\Gamma\vdash x:\mathrm{E} \quad \mathrm{E} <: \mathrm{Excpetion}
}{
\Gamma\vdash \text{throw}\ x:\mathrm{T}
}
$$

And the user may use `try` and `catch` to handle the exception value thrown by the inner codes:

$$
\text{try}\ x\ \text{catch}\ h \rightarrow x
$$

$$
\text{try}\ (\text{throw}\ x)\ \text{catch}\ h \rightarrow h(x)
$$

$$
\frac{
x_1 \rightarrow x_2
}{
\text{try}\ x_1\ \text{catch}\ h \rightarrow \text{try}\ x_2\ \text{catch}\ h
}
$$

$$
\frac{
\Gamma\vdash x:\mathrm{T}
\quad
\Gamma\vdash h:\mathrm{T}
}{
\Gamma\vdash \text{try}\ x\ \text{catch}\ h:\mathrm{T}
}
$$
