# Type Casting

## For Built-in Types

Arithmetic types are always mutally convertible:

$$
\frac{
\Gamma\vdash e: \mathrm{S}
\quad
\mathrm{T} \in \mathrm{Arithm}
\quad
\mathrm{S} \in \mathrm{Arithm}
}{
e\ \text{as}\ \mathrm{T}: \mathrm{T}
}
$$

$$
\frac{
\text{typeof}(v) = S
\quad
\mathrm{T} \in \mathrm{Arithm}
\quad
\mathrm{S} \in \mathrm{Arithm}
\quad
v_1 \rightarrow v_2
}{
v_1\ \text{as}\ \mathrm{T} \rightarrow v_2\ \text{as}\ \mathrm{T}
}
$$

## For Objects

Conversion between objects type may produce either results: the object itself or `null`.

$$
\frac{
\Gamma\vdash e: \mathrm{S}
\quad
\mathrm{T} <: \mathrm{P}
\quad
\mathrm{S} <: \mathrm{P}
\quad
\mathrm{P} <: \mathrm{object}
\quad
\mathrm{S} <: \mathrm{T}
}{
e\ \text{as}\ \mathrm{T}: \mathrm{T}
}
$$

$$
\frac{
\Gamma\vdash e: \mathrm{S}
\quad
\mathrm{T} <: \mathrm{P}
\quad
\mathrm{S} <: \mathrm{P}
\quad
\mathrm{P} <: \mathrm{object}
\quad
\mathrm{S} \not<: \mathrm{T}
\quad
\mathrm{T} <: \mathrm{S}
}{
e\ \text{as}\ \mathrm{T}: \mathrm{T?}
}
$$

```slake
derived1 as Base; // OK
derived1 as Derived2; // Error
derived1 as Base as Derived2; // OK but returns null
```

If the object's type is subtype of the target type, the expression returns the object itself.

$$
\frac{
\text{typeof}(v): \mathrm{S}
\quad
\mathrm{S} <: \mathrm{P}
\quad
\mathrm{T} <: \mathrm{P}
\quad
\mathrm{P} <: \mathrm{object}
\quad
\mathrm{S} <: \mathrm{T}
}{
v\ \text{as}\ \mathrm{T} \rightarrow v
}
$$

or, if not (e.g. the types are unrelated or the actual type is less derived than the target type), the expression returns `null`.

$$
\frac{
\text{typeof}(v): \mathrm{S}
\quad
\mathrm{S} <: \mathrm{P}
\quad
\mathrm{T} <: \mathrm{P}
\quad
\mathrm{P} <: \mathrm{object}
\quad
\mathrm{S} \not<: \mathrm{T}
\quad
\mathrm{T} <: \mathrm{S}
}{
v\ \text{as}\ \mathrm{T} \rightarrow \text{null}
}
$$