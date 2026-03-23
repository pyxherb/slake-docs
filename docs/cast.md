# Type Casting

Type casting means convert a value to another value with another type.

$$
\frac{
v_1 \rightarrow v_2
}{
v_1\ \text{as}\ \mathrm{T} \rightarrow v_2\ \text{as}\ \mathrm{T}
}
$$

## Functions and Operations

We define following functions and operations for the specifications, note that the implementation may be different but the final result and side effects (if specified) must be the same to the specification:

$\text{typeof}(v)$ - Evaluates type of the value $v$.

$\text{int\_width}(t)$ - Evaluates width of the type $t$ where $t$ must be a type in $Integer$.

$\text{narrow\_int}(v, n)$ - Narrow $v$ into an $n$ bits integer, where `v` must be $Integer$ typed.

$\text{widen\_uint}(v, n)$ - Widen $v$ into an $n$ bits unsigned integer, where `v` must be $UnsignedInteger$ typed.

$\text{widen\_int}(v, n)$ - Widen $v$ into an $n$ bits signed integer, where `v` must be $SignedInteger$ typed.

$\text{to\_signed}(v)$ - Convert $v$ into a same-width signed integer, where `v` must be $UnsignedInteger$ typed.

$\text{to\_unsigned}(v)$ - Convert $v$ into a same-width signed integer, where `v` must be $SignedInteger$ typed.

$\text{to\_f32}(v)$ - Convert $v$ into a 32-bit IEEE754 floating-point number, where `v` must be $Arithm$ typed.

$\text{to\_f64}(v)$ - Convert $v$ into a 64-bit IEEE754 floating-point number, where `v` must be $Arithm$ typed.

$\text{int\_to\_width}(v, n)$ - Converts $v$ into an signed integer value with specific width, where `v` must be an $Integer$ typed value:

$$
\frac{
\text{typeof}(v): \mathrm{T}
\quad
\mathrm{T} \in SignedInteger
\quad
\text{int\_width}(\mathrm{T}) < n
}{
\text{int\_to\_width}(v, n) = \text{widen\_int}(v, n)
}
$$

$$
\frac{
\text{typeof}(v): \mathrm{T}
\quad
\mathrm{T} \in SignedInteger
\quad
\text{int\_width}(\mathrm{T}) = n
}{
\text{int\_to\_width}(v, n) = v
}
$$

$$
\frac{
\text{typeof}(v): \mathrm{T}
\quad
\mathrm{T} \in SignedInteger
\quad
\text{int\_width}(\mathrm{T}) > n
}{
\text{int\_to\_width}(v, n) = \text{narrow\_int}(v, n)
}
$$

$$
\frac{
\text{typeof}(v): \mathrm{T}
\quad
\mathrm{T} \in UnsignedInteger
}{
\text{int\_to\_width}(v, n) = \text{int\_to\_width}(\text{to\_signed}(v), n)
}
$$

$\text{int\_to\_width}(v, n)$ - Converts $v$ into an unsigned uinteger value with specific width, where `v` must be an $Integer$ typed value:

$$
\frac{
\text{typeof}(v): \mathrm{T}
\quad
\mathrm{T} \in UnsignedInteger
\quad
\text{int\_width}(\mathrm{T}) < n
}{
\text{int\_to\_width}(v, n) = \text{widen\_uint}(v, n)
}
$$

$$
\frac{
\text{typeof}(v): \mathrm{T}
\quad
\mathrm{T} \in UnsignedInteger
\quad
\text{int\_width}(\mathrm{T}) = n
}{
\text{int\_to\_width}(v, n) = v
}
$$

$$
\frac{
\text{typeof}(v): \mathrm{T}
\quad
\mathrm{T} \in UnsignedInteger
\quad
\text{int\_width}(\mathrm{T}) > n
}{
\text{int\_to\_width}(v, n) = \text{narrow\_uint}(v, n)
}
$$

$$
\frac{
\text{typeof}(v): \mathrm{T}
\quad
\mathrm{T} \in SignedInteger
}{
\text{int\_to\_width}(v, n) = \text{int\_to\_width}(\text{to\_unsigned}(v), n)
}
$$

$\text{int\_to\_i8}(v)$ - Converts $v$ into an `i8` value, where `v` must be an $Integer$ typed value, widening, narrowing or signed conversion may be applied.

$$
\frac{
\text{typeof}(v): \mathrm{T}
\quad
\mathrm{T} \in Integer
}{
\text{int\_to\_i8}(v, n) = \text{int\_to\_width}(v, 8)
}
$$

$\text{int\_to\_i16}(v)$ - Converts $v$ into an `i16` value, where `v` must be an $Integer$ typed value, widening, narrowing or signed conversion may be applied.

$$
\frac{
\text{typeof}(v): \mathrm{T}
\quad
\mathrm{T} \in Integer
}{
\text{int\_to\_i16}(v, n) = \text{int\_to\_width}(v, 16)
}
$$

$\text{int\_to\_i32}(v)$ - Converts $v$ into an `i32` value, where `v` must be an $Integer$ typed value, widening, narrowing or signed conversion may be applied.

$$
\frac{
\text{typeof}(v): \mathrm{T}
\quad
\mathrm{T} \in Integer
}{
\text{int\_to\_i32}(v, n) = \text{int\_to\_width}(v, 32)
}
$$

$\text{int\_to\_i64}(v)$ - Converts $v$ into an `i64` value, where `v` must be an $Integer$ typed value, widening or signed conversion may be applied.

$$
\frac{
\text{typeof}(v): \mathrm{T}
\quad
\mathrm{T} \in Integer
}{
\text{int\_to\_i64}(v, n) = \text{int\_to\_width}(v, 64)
}
$$

$\text{int\_to\_u8}(v)$ - Converts $v$ into an `u8` value, where `v` must be an $Integer$ typed value, widening, narrowing or signed conversion may be applied.

$$
\frac{
\text{typeof}(v): \mathrm{T}
\quad
\mathrm{T} \in Integer
}{
\text{int\_to\_u8}(v, n) = \text{int\_to\_width}(v, 8)
}
$$

$\text{int\_to\_u16}(v)$ - Converts $v$ into an `u16` value, where `v` must be an $Integer$ typed value, widening, narrowing or signed conversion may be applied.

$$
\frac{
\text{typeof}(v): \mathrm{T}
\quad
\mathrm{T} \in Integer
}{
\text{int\_to\_u16}(v, n) = \text{int\_to\_width}(v, 16)
}
$$

$\text{int\_to\_u32}(v)$ - Converts $v$ into an `u32` value, where `v` must be an $Integer$ typed value, widening, narrowing or signed conversion may be applied.

$$
\frac{
\text{typeof}(v): \mathrm{T}
\quad
\mathrm{T} \in Integer
}{
\text{int\_to\_u32}(v, n) = \text{int\_to\_width}(v, 32)
}
$$

$\text{int\_to\_u64}(v)$ - Converts $v$ into an `u64` value, where `v` must be an $Integer$ typed value, widening or signed conversion may be applied.

$$
\frac{
\text{typeof}(v): \mathrm{T}
\quad
\mathrm{T} \in Integer
}{
\text{int\_to\_u64}(v, n) = \text{int\_to\_width}(v, 64)
}
$$

$\text{float\_to\_i8}(v)$ - Converts $v$ into an `i8` value, where `v` must be a $Float$ typed value.

$\text{float\_to\_i16}(v)$ - Converts $v$ into an `i16` value, where `v` must be a $Float$ typed value.

$\text{float\_to\_i32}(v)$ - Converts $v$ into an `i32` value, where `v` must be a $Float$ typed value.

$\text{float\_to\_i64}(v)$ - Converts $v$ into an `i64` value, where `v` must be a $Float$ typed value.

$\text{float\_to\_u8}(v)$ - Converts $v$ into an `u8` value, where `v` must be a $Float$ typed value.

$\text{float\_to\_u16}(v)$ - Converts $v$ into an `u16` value, where `v` must be a $Float$ typed value.

$\text{float\_to\_u32}(v)$ - Converts $v$ into an `u32` value, where `v` must be a $Float$ typed value.

$\text{float\_to\_u64}(v)$ - Converts $v$ into an `u64` value, where `v` must be a $Float$ typed value.

$\text{to\_i8}(v)$ - Converts $v$ into an `i8` value, where `v` must be an $Arithm$ typed value.

$$
\frac{
\text{typeof}(v): \mathrm{T}
\quad
\mathrm{T} \in Integer
}{
\text{to\_i8}(v, n) = \text{int\_to\_i8}(v, n)
}
$$

$$
\frac{
\text{typeof}(v): \mathrm{T}
\quad
\mathrm{T} \in Float
}{
\text{to\_i8}(v, n) = \text{float\_to\_i8}(v, n)
}
$$

$\text{to\_i16}(v)$ - Converts $v$ into an `i16` value, where `v` must be an $Arithm$ typed value.

$$
\frac{
\text{typeof}(v): \mathrm{T}
\quad
\mathrm{T} \in Integer
}{
\text{to\_i16}(v, n) = \text{int\_to\_i16}(v, n)
}
$$

$$
\frac{
\text{typeof}(v): \mathrm{T}
\quad
\mathrm{T} \in Float
}{
\text{to\_i16}(v, n) = \text{float\_to\_i16}(v, n)
}
$$

$\text{to\_i32}(v)$ - Converts $v$ into an `i32` value, where `v` must be an $Arithm$ typed value.

$$
\frac{
\text{typeof}(v): \mathrm{T}
\quad
\mathrm{T} \in Integer
}{
\text{to\_i32}(v, n) = \text{int\_to\_i32}(v, n)
}
$$

$$
\frac{
\text{typeof}(v): \mathrm{T}
\quad
\mathrm{T} \in Float
}{
\text{to\_i32}(v, n) = \text{float\_to\_i32}(v, n)
}
$$

$\text{to\_i64}(v)$ - Converts $v$ into an `i64` value, where `v` must be an $Arithm$ typed value.
$$
\frac{
\text{typeof}(v): \mathrm{T}
\quad
\mathrm{T} \in Integer
}{
\text{to\_i64}(v, n) = \text{int\_to\_i64}(v, n)
}
$$

$$
\frac{
\text{typeof}(v): \mathrm{T}
\quad
\mathrm{T} \in Float
}{
\text{to\_i64}(v, n) = \text{float\_to\_i64}(v, n)
}
$$

$\text{to\_u8}(v)$ - Converts $v$ into an `u8` value, where `v` must be an $Arithm$ typed value.

$$
\frac{
\text{typeof}(v): \mathrm{T}
\quad
\mathrm{T} \in Integer
}{
\text{to\_u8}(v, n) = \text{int\_to\_u8}(v, n)
}
$$

$$
\frac{
\text{typeof}(v): \mathrm{T}
\quad
\mathrm{T} \in Float
}{
\text{to\_u8}(v, n) = \text{float\_to\_u8}(v, n)
}
$$

$\text{to\_u16}(v)$ - Converts $v$ into an `u16` value, where `v` must be an $Arithm$ typed value.

$$
\frac{
\text{typeof}(v): \mathrm{T}
\quad
\mathrm{T} \in Integer
}{
\text{to\_u16}(v, n) = \text{int\_to\_u16}(v, n)
}
$$

$$
\frac{
\text{typeof}(v): \mathrm{T}
\quad
\mathrm{T} \in Float
}{
\text{to\_u16}(v, n) = \text{float\_to\_u16}(v, n)
}
$$

$\text{to\_u32}(v)$ - Converts $v$ into an `u32` value, where `v` must be an $Arithm$ typed value.

$$
\frac{
\text{typeof}(v): \mathrm{T}
\quad
\mathrm{T} \in Integer
}{
\text{to\_u32}(v, n) = \text{int\_to\_u32}(v, n)
}
$$

$$
\frac{
\text{typeof}(v): \mathrm{T}
\quad
\mathrm{T} \in Float
}{
\text{to\_u32}(v, n) = \text{float\_to\_u32}(v, n)
}
$$

$\text{to\_u64}(v)$ - Converts $v$ into an `u64` value, where `v` must be an $Arithm$ typed value.

$$
\frac{
\text{typeof}(v): \mathrm{T}
\quad
\mathrm{T} \in Integer
}{
\text{to\_u64}(v, n) = \text{int\_to\_u64}(v, n)
}
$$

$$
\frac{
\text{typeof}(v): \mathrm{T}
\quad
\mathrm{T} \in Float
}{
\text{to\_u64}(v, n) = \text{float\_to\_u64}(v, n)
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
$$

$$
\frac{
\text{typeof}(v) = \mathrm{S}
\quad
\mathrm{S} \in \mathrm{Arithm}
}{
v\ \text{as}\ \mathrm{i8} \rightarrow \text{to\_i8}(v)
}
$$

$$
\frac{
\text{typeof}(v) = \mathrm{S}
\quad
\mathrm{S} \in \mathrm{Arithm}
}{
v\ \text{as}\ \mathrm{i16} \rightarrow \text{to\_i16}(v)
}
$$

$$
\frac{
\text{typeof}(v) = \mathrm{S}
\quad
\mathrm{S} \in \mathrm{Arithm}
}{
v\ \text{as}\ \mathrm{i32} \rightarrow \text{to\_i32}(v)
}
$$

$$
\frac{
\text{typeof}(v) = \mathrm{S}
\quad
\mathrm{S} \in \mathrm{Arithm}
}{
v\ \text{as}\ \mathrm{i64} \rightarrow \text{to\_i64}(v)
}
$$

$$
\frac{
\text{typeof}(v) = \mathrm{S}
\quad
\mathrm{S} \in \mathrm{Arithm}
}{
v\ \text{as}\ \mathrm{u8} \rightarrow \text{to\_u8}(v)
}
$$

$$
\frac{
\text{typeof}(v) = \mathrm{S}
\quad
\mathrm{S} \in \mathrm{Arithm}
}{
v\ \text{as}\ \mathrm{u16} \rightarrow \text{to\_u16}(v)
}
$$

$$
\frac{
\text{typeof}(v) = \mathrm{S}
\quad
\mathrm{S} \in \mathrm{Arithm}
}{
v\ \text{as}\ \mathrm{u32} \rightarrow \text{to\_u32}(v)
}
$$

$$
\frac{
\text{typeof}(v) = \mathrm{S}
\quad
\mathrm{S} \in \mathrm{Arithm}
}{
v\ \text{as}\ \mathrm{u64} \rightarrow \text{to\_u64}(v)
}
$$

$$
\frac{
\text{typeof}(v) = \mathrm{S}
\quad
\mathrm{S} \in \mathrm{Arithm}
}{
v\ \text{as}\ \mathrm{f32} \rightarrow \text{to\_f32}(v)
}
$$

$$
\frac{
\text{typeof}(v) = \mathrm{S}
\quad
\mathrm{S} \in \mathrm{Arithm}
}{
v\ \text{as}\ \mathrm{f64} \rightarrow \text{to\_f64}(v)
}
$$

`bool` type can be converted to integer types, but cannot be converted into floating-point types, or a `TypeCastException` instance should be thrown by the runtime:

$$
\frac{
\Gamma\vdash e: \mathrm{bool}
\quad
\mathrm{S} \in \mathrm{Integer}
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
\quad
\frac{
v = \text{true}
}{
v\ \text{as}\ \mathrm{i8} \rightarrow \text{1}\ \text{as}\ \mathrm{i8}
}
$$

$$
\frac{
v = \text{false}
}{
v\ \text{as}\ \mathrm{i16} \rightarrow \text{0}\ \text{as}\ \mathrm{i16}
}
\quad
\frac{
v = \text{true}
}{
v\ \text{as}\ \mathrm{i16} \rightarrow \text{1}\ \text{as}\ \mathrm{i16}
}
$$

$$
\frac{
v = \text{false}
}{
v\ \text{as}\ \mathrm{i32} \rightarrow \text{0}\ \text{as}\ \mathrm{i32}
}
\quad
\frac{
v = \text{true}
}{
v\ \text{as}\ \mathrm{i32} \rightarrow \text{1}\ \text{as}\ \mathrm{i32}
}
$$

$$
\frac{
v = \text{false}
}{
v\ \text{as}\ \mathrm{i64} \rightarrow \text{0}\ \text{as}\ \mathrm{i64}
}
\quad
\frac{
v = \text{true}
}{
v\ \text{as}\ \mathrm{i64} \rightarrow \text{1}\ \text{as}\ \mathrm{i64}
}
$$

$$
\frac{
v = \text{false}
}{
v\ \text{as}\ \mathrm{u8} \rightarrow \text{0}\ \text{as}\ \mathrm{u8}
}
\quad
\frac{
v = \text{true}
}{
v\ \text{as}\ \mathrm{u8} \rightarrow \text{1}\ \text{as}\ \mathrm{u8}
}
$$

$$
\frac{
v = \text{false}
}{
v\ \text{as}\ \mathrm{u16} \rightarrow \text{0}\ \text{as}\ \mathrm{u16}
}
\quad
\frac{
v = \text{true}
}{
v\ \text{as}\ \mathrm{u16} \rightarrow \text{1}\ \text{as}\ \mathrm{u16}
}
$$

$$
\frac{
v = \text{false}
}{
v\ \text{as}\ \mathrm{u32} \rightarrow \text{0}\ \text{as}\ \mathrm{u32}
}
\quad
\frac{
v = \text{true}
}{
v\ \text{as}\ \mathrm{u32} \rightarrow \text{1}\ \text{as}\ \mathrm{u32}
}
$$

$$
\frac{
v = \text{false}
}{
v\ \text{as}\ \mathrm{u64} \rightarrow \text{0}\ \text{as}\ \mathrm{u64}
}
\quad
\frac{
v = \text{true}
}{
v\ \text{as}\ \mathrm{u64} \rightarrow \text{1}\ \text{as}\ \mathrm{u64}
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

The runtime should throw a `TypeCastException` instance when converting an object reference value to an unrelated type:

$$
\frac{
\text{typeof}(v): \mathrm{S}
\quad
\mathrm{S} <: \mathrm{object}
\quad
\mathrm{T} <: \mathrm{object}
\quad
\mathrm{S} \not<: \mathrm{T}
\quad
\mathrm{T} \not<: \mathrm{S}
}{
v\ \text{as}\ \mathrm{T} \rightarrow \text{throw}\ \text{new}\ \mathrm{TypeCastException}()
}
$$
