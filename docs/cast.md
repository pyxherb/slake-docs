# Type Casting

Type casting means convert a value to another value with another type.

$$
\frac{
    v_1 \rightarrow v_2
}{
    v_1\ \text{as}\ \mathrm{T} \rightarrow v_2\ \text{as}\ \mathrm{T}
}
$$

## Operations

$\text{IsConvertible}(A, B)$ checks if A can be converted into B.

$$
\frac{
    \Gamma\vdash\mathrm{A}\ \mathrm{type}
    \quad
    \Gamma\vdash\mathrm{B}\ \mathrm{type}
    \quad
    \mathrm{A} \in \mathrm{Arithm}
    \quad
    \mathrm{B} \in \mathrm{Arithm}
}{
    \text{IsConvertible}(\mathrm{A}, \mathrm{B}) = \text{true}
}
$$

$$
\frac{
    \Gamma\vdash\mathrm{A}\ \mathrm{type}
    \quad
    \mathrm{A} \in \mathrm{Arithm}
}{
    \text{IsConvertible}(\mathrm{A}, \mathrm{bool}) = \text{true}
}
$$

$$
\frac{
    \Gamma\vdash\mathrm{B}\ \mathrm{type}
    \quad
    \mathrm{B} \in \mathrm{Arithm}
}{
    \text{IsConvertible}(\mathrm{bool}, \mathrm{B}) = \text{true}
}
$$

$$
\frac{
    \Gamma\vdash\mathrm{A}\ \mathrm{type}
    \quad
    \Gamma\vdash\mathrm{B}\ \mathrm{type}
    \quad
    \mathrm{A} <: \mathrm{B}
    \vee
    \mathrm{B} <: \mathrm{A}
}{
    \text{IsConvertible}(\mathrm{A}, \mathrm{B}) = \text{true}
}
$$

$$
\frac{
    \text{otherwise}
}{
    \text{IsConvertible}(\mathrm{A}, \mathrm{B}) = \text{false}
}
$$

$\text{RemoveRef}(T)$ removes the reference modifier if $T$ is a reference type.

$$
\frac{
    \Gamma\vdash\mathrm{U}\ \mathrm{type}
    \quad
    \Gamma\vdash\mathrm{T} = \mathrm{U\And}
}{
    \text{RemoveRef}(\mathrm{T}) = \text{U}
}
$$

$$
\frac{
    \Gamma\vdash\mathrm{U}\ \mathrm{type}
    \quad
    \Gamma\vdash\mathrm{T} = \mathrm{U}
}{
    \text{RemoveRef}(\mathrm{T}) = \text{U}
}
$$

## Functions and Operations

We define following functions and operations for the following explanation, note that the implementation may be different but the final result and side effects (if specified) must be the same to the specification (for pre-defined built-in operations, see [Built-in Operations](builtin_ops.md)).

$\text{int\_to\_width}(v, n)$ - Converts $v$ into an unsigned uinteger value with specific width, where `v` must be an $\mathrm{Integer}$ typed value:

$$
\frac{
    \text{\_\_typeof}(v) = \mathrm{T}
    \quad
    \mathrm{T} \in \mathrm{SignedInteger}
    \quad
    \text{\_\_int\_width}(\mathrm{T}) < n
}{
    \text{int\_to\_width}(v, n) \rightarrow \text{widen\_int}(v, n)
}
$$

$$
\frac{
    \text{\_\_typeof}(v) = \mathrm{T}
    \quad
    \mathrm{T} \in \mathrm{SignedInteger}
    \quad
    \text{\_\_int\_width}(\mathrm{T}) = n
}{
    \text{int\_to\_width}(v, n) \rightarrow v
}
$$

$$
\frac{
    \text{\_\_typeof}(v) = \mathrm{T}
    \quad
    \mathrm{T} \in \mathrm{SignedInteger}
    \quad
    \text{\_\_int\_width}(\mathrm{T}) > n
}{
    \text{int\_to\_width}(v, n) \rightarrow \text{narrow\_int}(v, n)
}
$$

$$
\frac{
    \text{\_\_typeof}(v) = \mathrm{T}
    \quad
    \mathrm{T} \in \mathrm{UnsignedInteger}
}{
    \text{int\_to\_width}(v, n) \rightarrow \text{int\_to\_width}(\text{\_\_to\_signed}(v), n)
}
$$

$$
\frac{
    \text{\_\_typeof}(v) = \mathrm{T}
    \quad
    \mathrm{T} \in \mathrm{UnsignedInteger}
    \quad
    \text{\_\_int\_width}(\mathrm{T}) < n
}{
    \text{int\_to\_width}(v, n) \rightarrow \text{widen\_uint}(v, n)
}
$$

$$
\frac{
    \text{\_\_typeof}(v) = \mathrm{T}
    \quad
    \mathrm{T} \in \mathrm{UnsignedInteger}
    \quad
    \text{\_\_int\_width}(\mathrm{T}) = n
}{
    \text{int\_to\_width}(v, n) \rightarrow v
}
$$

$$
\frac{
    \text{\_\_typeof}(v) = \mathrm{T}
    \quad
    \mathrm{T} \in \mathrm{UnsignedInteger}
    \quad
    \text{\_\_int\_width}(\mathrm{T}) > n
}{
    \text{int\_to\_width}(v, n) \rightarrow \text{narrow\_uint}(v, n)
}
$$

$$
\frac{
    \text{\_\_typeof}(v) = \mathrm{T}
    \quad
    \mathrm{T} \in \mathrm{SignedInteger}
}{
    \text{int\_to\_width}(v, n) \rightarrow \text{int\_to\_width}(\text{\_\_to\_unsigned}(v), n)
}
$$

$\text{int\_to\_i8}(v)$ - Converts $v$ into an `i8` value, where `v` must be an $\mathrm{Integer}$ typed value, widening, narrowing or signed conversion may be applied.

$$
\frac{
    \text{\_\_typeof}(v) = \mathrm{T}
    \quad
    \mathrm{T} \in \mathrm{Integer}
}{
    \text{int\_to\_i8}(v, n) \rightarrow \text{int\_to\_width}(v, 8)
}
$$

$\text{int\_to\_i16}(v)$ - Converts $v$ into an `i16` value, where `v` must be an $\mathrm{Integer}$ typed value, widening, narrowing or signed conversion may be applied.

$$
\frac{
    \text{\_\_typeof}(v) = \mathrm{T}
    \quad
    \mathrm{T} \in \mathrm{Integer}
}{
    \text{int\_to\_i16}(v, n) \rightarrow \text{int\_to\_width}(v, 16)
}
$$

$\text{int\_to\_i32}(v)$ - Converts $v$ into an `i32` value, where `v` must be an $\mathrm{Integer}$ typed value, widening, narrowing or signed conversion may be applied.

$$
\frac{
    \text{\_\_typeof}(v) = \mathrm{T}
    \quad
    \mathrm{T} \in \mathrm{Integer}
}{
    \text{int\_to\_i32}(v, n) \rightarrow \text{int\_to\_width}(v, 32)
}
$$

$\text{int\_to\_i64}(v)$ - Converts $v$ into an `i64` value, where `v` must be an $\mathrm{Integer}$ typed value, widening or signed conversion may be applied.

$$
\frac{
    \text{\_\_typeof}(v) = \mathrm{T}
    \quad
    \mathrm{T} \in \mathrm{Integer}
}{
    \text{int\_to\_i64}(v, n) \rightarrow \text{int\_to\_width}(v, 64)
}
$$

$\text{int\_to\_u8}(v)$ - Converts $v$ into an `u8` value, where `v` must be an $\mathrm{Integer}$ typed value, widening, narrowing or signed conversion may be applied.

$$
\frac{
    \text{\_\_typeof}(v) = \mathrm{T}
    \quad
    \mathrm{T} \in \mathrm{Integer}
}{
    \text{int\_to\_u8}(v, n) \rightarrow \text{int\_to\_width}(v, 8)
}
$$

$\text{int\_to\_u16}(v)$ - Converts $v$ into an `u16` value, where `v` must be an $\mathrm{Integer}$ typed value, widening, narrowing or signed conversion may be applied.

$$
\frac{
    \text{\_\_typeof}(v) = \mathrm{T}
    \quad
    \mathrm{T} \in \mathrm{Integer}
}{
    \text{int\_to\_u16}(v, n) \rightarrow \text{int\_to\_width}(v, 16)
}
$$

$\text{int\_to\_u32}(v)$ - Converts $v$ into an `u32` value, where `v` must be an $\mathrm{Integer}$ typed value, widening, narrowing or signed conversion may be applied.

$$
\frac{
    \text{\_\_typeof}(v) = \mathrm{T}
    \quad
    \mathrm{T} \in \mathrm{Integer}
}{
    \text{int\_to\_u32}(v, n) \rightarrow \text{int\_to\_width}(v, 32)
}
$$

$\text{int\_to\_u64}(v)$ - Converts $v$ into an `u64` value, where `v` must be an $\mathrm{Integer}$ typed value, widening or signed conversion may be applied.

$$
\frac{
    \text{\_\_typeof}(v) = \mathrm{T}
    \quad
    \mathrm{T} \in \mathrm{Integer}
}{
    \text{int\_to\_u64}(v, n) \rightarrow \text{int\_to\_width}(v, 64)
}
$$

$\text{to\_i8}(v)$ - Converts $v$ into an `i8` value, where `v` must be an $Arithm$ typed value.

$$
\frac{
    \text{\_\_typeof}(v) = \mathrm{T}
    \quad
    \mathrm{T} \in \mathrm{Integer}
}{
    \text{to\_i8}(v, n) \rightarrow \text{int\_to\_i8}(v, n)
}
$$

$$
\frac{
    \text{\_\_typeof}(v) = \mathrm{T}
    \quad
    \mathrm{T} \in \mathrm{Float}
}{
    \text{to\_i8}(v, n) \rightarrow \text{\_\_float\_to\_i8}(v, n)
}
$$

$\text{to\_i16}(v)$ - Converts $v$ into an `i16` value, where `v` must be an $Arithm$ typed value.

$$
\frac{
    \text{\_\_typeof}(v) = \mathrm{T}
    \quad
    \mathrm{T} \in \mathrm{Integer}
}{
    \text{to\_i16}(v, n) \rightarrow \text{int\_to\_i16}(v, n)
}
$$

$$
\frac{
    \text{\_\_typeof}(v) = \mathrm{T}
    \quad
    \mathrm{T} \in \mathrm{Float}
}{
    \text{to\_i16}(v, n) \rightarrow \text{\_\_float\_to\_i16}(v, n)
}
$$

$\text{to\_i32}(v)$ - Converts $v$ into an `i32` value, where `v` must be an $Arithm$ typed value.

$$
\frac{
    \text{\_\_typeof}(v) = \mathrm{T}
    \quad
    \mathrm{T} \in \mathrm{Integer}
}{
    \text{to\_i32}(v, n) \rightarrow \text{int\_to\_i32}(v, n)
}
$$

$$
\frac{
    \text{\_\_typeof}(v) = \mathrm{T}
    \quad
    \mathrm{T} \in \mathrm{Float}
}{
    \text{to\_i32}(v, n) \rightarrow \text{\_\_float\_to\_i32}(v, n)
}
$$

$\text{to\_i64}(v)$ - Converts $v$ into an `i64` value, where `v` must be an $Arithm$ typed value.
$$
\frac{
    \text{\_\_typeof}(v) = \mathrm{T}
    \quad
    \mathrm{T} \in \mathrm{Integer}
}{
    \text{to\_i64}(v, n) \rightarrow \text{int\_to\_i64}(v, n)
}
$$

$$
\frac{
    \text{\_\_typeof}(v) = \mathrm{T}
    \quad
    \mathrm{T} \in \mathrm{Float}
}{
    \text{to\_i64}(v, n) \rightarrow \text{\_\_float\_to\_i64}(v, n)
}
$$

$\text{to\_u8}(v)$ - Converts $v$ into an `u8` value, where `v` must be an $Arithm$ typed value.

$$
\frac{
    \text{\_\_typeof}(v) = \mathrm{T}
    \quad
    \mathrm{T} \in \mathrm{Integer}
}{
    \text{to\_u8}(v, n) \rightarrow \text{int\_to\_u8}(v, n)
}
$$

$$
\frac{
    \text{\_\_typeof}(v) = \mathrm{T}
    \quad
    \mathrm{T} \in \mathrm{Float}
}{
    \text{to\_u8}(v, n) \rightarrow \text{\_\_float\_to\_u8}(v, n)
}
$$

$\text{to\_u16}(v)$ - Converts $v$ into an `u16` value, where `v` must be an $Arithm$ typed value.

$$
\frac{
    \text{\_\_typeof}(v) = \mathrm{T}
    \quad
    \mathrm{T} \in \mathrm{Integer}
}{
    \text{to\_u16}(v, n) \rightarrow \text{int\_to\_u16}(v, n)
}
$$

$$
\frac{
    \text{\_\_typeof}(v) = \mathrm{T}
    \quad
    \mathrm{T} \in \mathrm{Float}
}{
    \text{to\_u16}(v, n) \rightarrow \text{\_\_float\_to\_u16}(v, n)
}
$$

$\text{to\_u32}(v)$ - Converts $v$ into an `u32` value, where `v` must be an $Arithm$ typed value.

$$
\frac{
    \text{\_\_typeof}(v) = \mathrm{T}
    \quad
    \mathrm{T} \in \mathrm{Integer}
}{
    \text{to\_u32}(v, n) \rightarrow \text{int\_to\_u32}(v, n)
}
$$

$$
\frac{
    \text{\_\_typeof}(v) = \mathrm{T}
    \quad
    \mathrm{T} \in \mathrm{Float}
}{
    \text{to\_u32}(v, n) \rightarrow \text{\_\_float\_to\_u32}(v, n)
}
$$

$\text{to\_u64}(v)$ - Converts $v$ into an `u64` value, where `v` must be an $Arithm$ typed value.

$$
\frac{
    \text{\_\_typeof}(v) = \mathrm{T}
    \quad
    \mathrm{T} \in \mathrm{Integer}
}{
    \text{to\_u64}(v, n) \rightarrow \text{int\_to\_u64}(v, n)
}
$$

$$
\frac{
    \text{\_\_typeof}(v) = \mathrm{T}
    \quad
    \mathrm{T} \in \mathrm{Float}
}{
    \text{to\_u64}(v, n) \rightarrow \text{\_\_float\_to\_u64}(v, n)
}
$$

$\text{to\_f32}(v)$ - Converts $v$ into an `f32` value, where `v` must be an $Arithm$ typed value.

$$
\frac{
    \text{\_\_typeof}(v) = \mathrm{T}
    \quad
    \mathrm{T} \in \mathrm{Arithm}
}{
    \text{to\_f32}(v, n) \rightarrow \text{\_\_to\_f32}(v, n)
}
$$

$\text{to\_f64}(v)$ - Converts $v$ into an `f64` value, where `v` must be an $Arithm$ typed value.

$$
\frac{
    \text{\_\_typeof}(v) = \mathrm{T}
    \quad
    \mathrm{T} \in \mathrm{Arithm}
}{
    \text{to\_f64}(v, n) \rightarrow \text{\_\_to\_f64}(v, n)
}
$$

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
(\text{T-ArithmCastAsArithm})
$$

$$
\frac{
    \text{\_\_typeof}(v) = \mathrm{S}
    \quad
    \mathrm{S} \in \mathrm{Arithm}
}{
    v\ \text{as}\ \mathrm{i8} \rightarrow \text{to\_i8}(v)
}
(\text{E-CastAsI8})
$$

$$
\frac{
    \text{\_\_typeof}(v) = \mathrm{S}
    \quad
    \mathrm{S} \in \mathrm{Arithm}
}{
    v\ \text{as}\ \mathrm{i16} \rightarrow \text{to\_i16}(v)
}
(\text{E-CastAsI16})
$$

$$
\frac{
    \text{\_\_typeof}(v) = \mathrm{S}
    \quad
    \mathrm{S} \in \mathrm{Arithm}
}{
    v\ \text{as}\ \mathrm{i32} \rightarrow \text{to\_i32}(v)
}
(\text{E-CastAsI32})
$$

$$
\frac{
    \text{\_\_typeof}(v) = \mathrm{S}
    \quad
    \mathrm{S} \in \mathrm{Arithm}
}{
    v\ \text{as}\ \mathrm{i64} \rightarrow \text{to\_i64}(v)
}
(\text{E-CastAsI64})
$$

$$
\frac{
    \text{\_\_typeof}(v) = \mathrm{S}
    \quad
    \mathrm{S} \in \mathrm{Arithm}
}{
    v\ \text{as}\ \mathrm{u8} \rightarrow \text{to\_u8}(v)
}
(\text{E-CastAsU8})
$$

$$
\frac{
    \text{\_\_typeof}(v) = \mathrm{S}
    \quad
    \mathrm{S} \in \mathrm{Arithm}
}{
    v\ \text{as}\ \mathrm{u16} \rightarrow \text{to\_u16}(v)
}
(\text{E-CastAsU16})
$$

$$
\frac{
    \text{\_\_typeof}(v) = \mathrm{S}
    \quad
    \mathrm{S} \in \mathrm{Arithm}
}{
    v\ \text{as}\ \mathrm{u32} \rightarrow \text{to\_u32}(v)
}
(\text{E-CastAsU32})
$$

$$
\frac{
    \text{\_\_typeof}(v) = \mathrm{S}
    \quad
    \mathrm{S} \in \mathrm{Arithm}
}{
    v\ \text{as}\ \mathrm{u64} \rightarrow \text{to\_u64}(v)
}
(\text{E-CastAsU64})
$$

$$
\frac{
    \text{\_\_typeof}(v) = \mathrm{S}
    \quad
    \mathrm{S} \in \mathrm{Arithm}
}{
    v\ \text{as}\ \mathrm{f32} \rightarrow \text{to\_f32}(v)
}
(\text{E-CastAsF32})
$$

$$
\frac{
    \text{\_\_typeof}(v) = \mathrm{S}
    \quad
    \mathrm{S} \in \mathrm{Arithm}
}{
    v\ \text{as}\ \mathrm{f64} \rightarrow \text{to\_f64}(v)
}
(\text{E-CastAsF64})
$$

`bool` type can be converted to integer types, but cannot be converted into floating-point types, or a `TypeCastException` instance should be thrown by the runtime:

$$
\frac{
    \Gamma\vdash e: \mathrm{bool}
    \quad
    \mathrm{S} \in \mathrm{\mathrm{Integer}}
}{
    e\ \text{as}\ \mathrm{T}: \mathrm{T}
}
$$

$$
\frac{
    v = \text{false}
}{
    v\ \text{as}\ \mathrm{i8} \rightarrow \text{0}\ \text{as}\ \mathrm{i8}
}
(\text{E-FalseAsI8})
\quad
\frac{
    v = \text{true}
}{
    v\ \text{as}\ \mathrm{i8} \rightarrow \text{1}\ \text{as}\ \mathrm{i8}
}
(\text{E-TrueAsI8})
$$

$$
\frac{
    v = \text{false}
}{
    v\ \text{as}\ \mathrm{i16} \rightarrow \text{0}\ \text{as}\ \mathrm{i16}
}
(\text{E-FalseAsI16})
\quad
\frac{
    v = \text{true}
}{
    v\ \text{as}\ \mathrm{i16} \rightarrow \text{1}\ \text{as}\ \mathrm{i16}
}
(\text{E-TrueAsI16})
$$

$$
\frac{
    v = \text{false}
}{
    v\ \text{as}\ \mathrm{i32} \rightarrow \text{0}\ \text{as}\ \mathrm{i32}
}
(\text{E-FalseAsI32})
\quad
\frac{
    v = \text{true}
}{
    v\ \text{as}\ \mathrm{i32} \rightarrow \text{1}\ \text{as}\ \mathrm{i32}
}
(\text{E-TrueAsI32})
$$

$$
\frac{
    v = \text{false}
}{
    v\ \text{as}\ \mathrm{i64} \rightarrow \text{0}\ \text{as}\ \mathrm{i64}
}
(\text{E-FalseAsI64})
\quad
\frac{
    v = \text{true}
}{
    v\ \text{as}\ \mathrm{i64} \rightarrow \text{1}\ \text{as}\ \mathrm{i64}
}
(\text{E-TrueAsI64})
$$

$$
\frac{
    v = \text{false}
}{
    v\ \text{as}\ \mathrm{u8} \rightarrow \text{0}\ \text{as}\ \mathrm{u8}
}
(\text{E-FalseAsU8})
\quad
\frac{
    v = \text{true}
}{
    v\ \text{as}\ \mathrm{u8} \rightarrow \text{1}\ \text{as}\ \mathrm{u8}
}
(\text{E-TrueAsU8})
$$

$$
\frac{
    v = \text{false}
}{
    v\ \text{as}\ \mathrm{u16} \rightarrow \text{0}\ \text{as}\ \mathrm{u16}
}
(\text{E-FalseAsU16})
\quad
\frac{
    v = \text{true}
}{
    v\ \text{as}\ \mathrm{u16} \rightarrow \text{1}\ \text{as}\ \mathrm{u16}
}
(\text{E-TrueAsU16})
$$

$$
\frac{
    v = \text{false}
}{
    v\ \text{as}\ \mathrm{u32} \rightarrow \text{0}\ \text{as}\ \mathrm{u32}
}
(\text{E-FalseAsU32})
\quad
\frac{
    v = \text{true}
}{
    v\ \text{as}\ \mathrm{u32} \rightarrow \text{1}\ \text{as}\ \mathrm{u32}
}
(\text{E-TrueAsU32})
$$

$$
\frac{
    v = \text{false}
}{
    v\ \text{as}\ \mathrm{u64} \rightarrow \text{0}\ \text{as}\ \mathrm{u64}
}
(\text{E-FalseAsU64})
\quad
\frac{
    v = \text{true}
}{
    v\ \text{as}\ \mathrm{u64} \rightarrow \text{1}\ \text{as}\ \mathrm{u64}
}
(\text{E-TrueAsU64})
$$

## For Objects

A normal type cast between object types produces a value with expected type. Note that both operands must be in the same subtype relationship chain:

$$
\frac{
    \Gamma\vdash e: \mathrm{S}
    \quad
    \mathrm{T} <: \mathrm{object?}
    \quad
    \mathrm{S} <: \mathrm{object}
    \quad
    \mathrm{S} <: \mathrm{T}
}{
    e\ \text{as}\ \mathrm{T}: \mathrm{T}
}
(\text{T-ObjectUpCast})
$$

$$
\frac{
    \Gamma\vdash e: \mathrm{S}
    \quad
    \mathrm{T} <: \mathrm{object?}
    \quad
    \mathrm{S} <: \mathrm{object}
    \quad
    \mathrm{S} \not<: \mathrm{T}
    \quad
    \mathrm{T} <: \mathrm{S}
}{
    e\ \text{as}\ \mathrm{T}: \mathrm{T}
}
(\text{T-ObjectDownCast})
$$

```slake
class Base {}
class Derived1(Base) {}
class Derived2(Base) {}
class Unrelated {}

fn test() {
    let d: Derived1 = new Derived1();
    d as Unrelated; // Error: the types are unrelated.
    d as Derived2;  // Error: Derived1 and Derived2 are not in the same subtype chain.
    d as Base;      // OK
}
```

The runtime will return the object itself or throw a `TypeCastException` if the actual type of the object does not match the expected type:

$$
\frac{
    \text{\_\_typeof}(v) = \mathrm{S}
    \quad
    \mathrm{S} <: \mathrm{object}
    \quad
    \mathrm{T} <: \mathrm{object}
    \quad
    \mathrm{S} <: \mathrm{T}
}{
    v\ \text{as}\ \mathrm{T} \rightarrow v
}
(\text{E-ObjectCast})
$$

$$
\frac{
    \text{\_\_typeof}(v) = \mathrm{S}
    \quad
    \mathrm{S} <: \mathrm{object}
    \quad
    \mathrm{T} <: \mathrm{object}
    \quad
    \mathrm{S} \not<: \mathrm{T}
    \quad
    \mathrm{T} <: \mathrm{S}
}{
    v\ \text{as}\ \mathrm{T} \rightarrow \text{throw}\ \text{new}\ \mathrm{TypeCastException}()
}
(\text{E-ObjectCastFailed})
$$

```slake
class Base {}
class Derived1(Base) {}
class Derived2(Base) {}

fn test() {
    let a: Base1 = new Derived1();
    a as Derived2; // Will throw an exception here.
}
```

Note that the nullable version does not throw an error, it returns a `null` value instead. The nullable type cast must be with a non-nullable object type.

$$
\frac{
    \Gamma\vdash e: \mathrm{S}
    \quad
    \mathrm{T} <: \mathrm{object?}
    \quad
    \mathrm{S} <: \mathrm{object}
    \quad
    \mathrm{S} <: \mathrm{T}
}{
    e\ \text{as?}\ \mathrm{T}: \mathrm{T}
}
(\text{T-NullableObjectUpCast})
$$

$$
\frac{
    \Gamma\vdash e: \mathrm{S}
    \quad
    \mathrm{T} <: \mathrm{object?}
    \quad
    \mathrm{S} <: \mathrm{object}
    \quad
    \mathrm{S} \not<: \mathrm{T}
    \quad
    \mathrm{T} <: \mathrm{S}
}{
    e\ \text{as?}\ \mathrm{T}: \mathrm{T?}
}
(\text{T-NullableObjectDownCast})
$$

$$
\frac{
    \text{\_\_typeof}(v) = \mathrm{S}
    \quad
    \mathrm{S} <: \mathrm{object?}
    \quad
    \mathrm{T} <: \mathrm{object}
    \quad
    \mathrm{S} <: \mathrm{T}
}{
    v\ \text{as?}\ \mathrm{T} \rightarrow v
}
(\text{E-NullableObjectCast})
$$

$$
\frac{
    \text{\_\_typeof}(v) = \mathrm{S}
    \quad
    \mathrm{S} <: \mathrm{object?}
    \quad
    \mathrm{T} <: \mathrm{object}
    \quad
    \mathrm{S} \not<: \mathrm{T}
    \quad
    \mathrm{T} <: \mathrm{S}
}{
    v\ \text{as?}\ \mathrm{T} \rightarrow \text{null}
}
(\text{E-NullableObjectCastFailed})
$$

```slake
class Base {}
class Derived1(Base) {}
class Derived2(Base) {}

fn test() {
    let a: Base1 = new Derived1();
    if (a as? Derived2 == null)     // Will return null here.
        throw TypeCastException();  // Will throw an exception here.
}
```

The runtime should throw a `TypeCastException` instance when converting an object reference value to an unrelated type (even the user cannot create examples that pass the compilation in the normal way):

$$
\frac{
    \text{\_\_typeof}(v) = \mathrm{S}
    \quad
    \mathrm{S} <: \mathrm{object?}
    \quad
    \mathrm{T} <: \mathrm{object?}
    \quad
    \mathrm{S} \not<: \mathrm{T}
    \quad
    \mathrm{T} \not<: \mathrm{S}
}{
    v\ \text{as}\ \mathrm{T} \rightarrow \text{throw}\ \text{new}\ \mathrm{TypeCastException}()
}
(\text{E-ObjectCastUnrelated})
$$

$$
\frac{
    \text{\_\_typeof}(v) = \mathrm{S}
    \quad
    \mathrm{S} <: \mathrm{object?}
    \quad
    \mathrm{T} <: \mathrm{object?}
    \quad
    \mathrm{S} \not<: \mathrm{T}
    \quad
    \mathrm{T} \not<: \mathrm{S}
}{
    v\ \text{as?}\ \mathrm{T} \rightarrow \text{throw}\ \text{new}\ \mathrm{TypeCastException}()
}
(\text{E-NullableObjectCastUnrelated})
$$

```slake
class Base1 {}
class Base2 {}

fn test() {
    let a: Base1 = new Base1();
    a as Base2; // Error: the types are unrelated.
    a as? Base2; // Error: the types are unrelated.
    // If the example above does occur in the runtime, the runtime should throw an exception.
}
```

## For Nullable Types

Conversion from a non-nullable type to a nullable type is always viable:

$$
\frac{
    \Gamma\vdash e: \mathrm{T}
}{
    e\ \text{as}\ \mathrm{T?}: \mathrm{T?}
}
(\text{T-NonNullableAsNullable})
$$

Converting a value into nullable type does not change itself anything:

$$
\frac{
    \text{\_\_typeof}(v) = \mathrm{T}
}{
    v\ \text{as}\ \mathrm{T?} \rightarrow v
}
(\text{E-NonNullAsNullable})
$$

And conversion from nullable type to non-nullable type is also viable in compile-time:

$$
\frac{
    \Gamma\vdash e: \mathrm{T?}
}{
    e\ \text{as}\ \mathrm{T}: \mathrm{T}
}
(\text{T-NullableAsNonNullable})
$$

But if the value is `null`, the runtime will throw a `TypeCastException` instance:

$$
\frac{
    \text{\_\_typeof}(v) = \mathrm{T}
}{
    v\ \text{as}\ \mathrm{T} \rightarrow v
}
(\text{E-NonNullAsNonNullable})
$$

$$
\frac{
    \text{\_\_typeof}(v) = \mathrm{null}
}{
    v\ \text{as}\ \mathrm{T} \rightarrow \text{throw}\ \text{new}\ \mathrm{TypeCastException}()
}
(\text{E-NullAsNonNullable})
$$

Note that both conversions must be with operands with the same base type:

$$
\Gamma\vdash e: \mathrm{S}
\quad
\mathrm{S} <: \mathrm{T}
\nRightarrow
e\ \text{as}\ \mathrm{T?}: \mathrm{T?}
$$

$$
\Gamma\vdash e: \mathrm{S?}
\quad
\mathrm{S} <: \mathrm{T}
\nRightarrow
e\ \text{as}\ \mathrm{T}: \mathrm{T}
$$

## Fallback Operation

If any condition listed above does not meet (in most cases will be prohibited by the compiler), the runtime should throw a `TypeCastException` by default instead of aborting:

$$
\frac{
    \text{otherwise}
}{
    v\ \text{as}\ \mathrm{T} \rightarrow \text{throw}\ \text{new}\ \mathrm{TypeCastException}()
}
(\text{E-CastUnrelated})
$$

$$
\frac{
    \text{otherwise}
}{
    v\ \text{as?}\ \mathrm{T} \rightarrow \text{throw}\ \text{new}\ \mathrm{TypeCastException}()
}
(\text{E-NullableCastUnrelated})
$$