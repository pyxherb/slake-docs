# Type Relationships

## Built-in Types

### any

`any` is the top type which can contain an instance of any type.

$$
\frac{\mathrm{T}\ \text{is\ a\ type}}{T <: \mathrm{any}}
$$

### never_t

`never_t` is the bottom type which has no instance.

$$
\frac{\mathrm{T}\ \text{is\ a\ type}}{\mathrm{never\_t} <: \mathrm{T}}
$$

### Built-in Arithmetic Types

#### Traits

We have following trait set for the arithmetic types:

$$
\begin{aligned}
\mathrm{ArithmTraits[T]} ::= \  &\mathrm{IAdd[T, T]}\\
&\mid\mathrm{ISub[T,T]}\\
&\mid\mathrm{IMul[T,T]}\\
&\mid\mathrm{IDiv[T,T]}\\
&\mid\mathrm{IMod[T,T]}\\
&\mid\mathrm{IAddAssign[T,T]}\\
&\mid\mathrm{ISubAssign[T,T]}\\
&\mid\mathrm{IMulAssign[T,T]}\\
&\mid\mathrm{IDivAssign[T,T]}\\
&\mid\mathrm{IModAssign[T,T]}\\
&\mid\mathrm{IEq[T,T]}\\
&\mid\mathrm{INeq[T,T]}\\
&\mid\mathrm{ILt[T,T]}\\
&\mid\mathrm{IGt[T,T]}\\
&\mid\mathrm{ILtEq[T,T]}\\
&\mid\mathrm{IGtEq[T,T]}\\
&\mid\mathrm{ICmp[T,T]}\\
&\mid\mathrm{INeg[T,T]}\\
\end{aligned}
$$

$$
\mathrm{FloatTraits[T]} ::= \mathrm{ArithmTraits[T]}\\
$$

$$
\begin{aligned}
\mathrm{IntegerTraits[T]} ::= &\mathrm{ArithmTraits[T]}\\
&\mid\mathrm{IAnd[T,T]}\\
&\mid\mathrm{IOr[T,T]}\\
&\mid\mathrm{IXor[T,T]}\\
&\mid\mathrm{ILsh[T,u32]}\\
&\mid\mathrm{IRsh[T,u32]}\\
&\mid\mathrm{IAndAssign[T,T]}\\
&\mid\mathrm{IOrAssign[T,T]}\\
&\mid\mathrm{IXorAssign[T,T]}\\
&\mid\mathrm{ILshAssign[T,u32]}\\
&\mid\mathrm{IRshAssign[T,u32]}\\
&\mid\mathrm{INot[T,T]}\\
\end{aligned}
$$

#### Built-in Integer Types

Signed integer types included `i8`, `i16`, `i32` and `i64`:

$$\mathrm{SignedInteger} ::=
\mathrm{i8}
\mid\mathrm{i16}
\mid\mathrm{i32}
\mid\mathrm{i64}
$$

For signed integer types, their relationship is defined as following:

$$
\frac{}{\mathrm{i8} <: \mathrm{i16}}
$$

$$
\frac{}{\mathrm{i16} <: \mathrm{i32}}
$$

$$
\frac{}{\mathrm{i32} <: \mathrm{i64}}
$$

Unsigned integer types included `u8`, `u16`, `u32` and `u64`:

$$\mathrm{UnsignedInteger} ::=
\mathrm{u8}
\mid\mathrm{u16}
\mid\mathrm{u32}
\mid\mathrm{u64}$$

For unsigned integer types, their relationship is defined as following:

$$
\frac{}{\mathrm{u8} <: \mathrm{u16}}
$$

$$
\frac{}{\mathrm{u16} <: \mathrm{u32}}
$$

$$
\frac{}{\mathrm{u32} <: \mathrm{u64}}
$$

Integer types included the signed integer types and the unsigned integer types:

$$
\mathrm{Integer} ::= \mathrm{SignedInteger}\mid\mathrm{UnsignedInteger}
$$

Every integer type is subtype of all types of the `IntegerTraits`.

$$
\frac{\mathrm{T} \in \mathrm{Integer}}{\mathrm{T} <: \mathrm{IntegerTraits[T]}}
$$

#### Built-in Floating-point Types

Floating-point types includes `f32` and `f64`:

$$
\mathrm{Float} ::=
\mathrm{f32}
\mid\mathrm{f64}
$$

Their relationships are like following:

$$
\frac{}{\mathrm{f32} <: \mathrm{f64}}
$$

## Nullable Types

Each nullable type's non-nullable version is its nullable version's parent type:

$$
\frac{}{\mathrm{T} <: \mathrm{T?}}
$$

If a type is a subtype of another type, the type's nullable version is subtype of another type's nullable version:

$$
\frac{\mathrm{T_1} <: \mathrm{T_2}}{\mathrm{T_1?} <: \mathrm{T_2?}}
$$

and `T?` is not `P`'s subtype where the `T` is subtype of `P`.

$$
\mathrm{T} <: \mathrm{P} \nRightarrow \mathrm{T?} <: \mathrm{P}
$$

`null` is subtype of all nullable types:

$$
\frac{}{\mathrm{null} <: \mathrm{T?}}
$$

## Type Operations

### Least Upper Bound

The Least Upper Bound $LUB(A, B)$ infers the upper bound between two types.

If one of the input type is another input type's subtype, return the another input type.

$$
LUB(A, B)=\begin{cases}
B &\mathrm{A} <: \mathrm{B}\\
A &\text{otherwise}
\end{cases}
$$

### Greatest Lower Bound

The Least Upper Bound $GLB(A, B)$ infers the lower bound between two types.

Selects and returns the most derived type from the parameters.

$$
GLB(A, B)=\begin{cases}
B &\mathrm{B} <: \mathrm{A}\\
A &\text{otherwise}
\end{cases}
$$

### Common Result Type

The Common Result Type Function $CommonResult(A, B)$ infers the common result value type between two result types.

$$
\frac{
    A <: B \mid B <: A
}{
    CommonType(A, B) = LUB(A,B)
}
$$

<!--
## Overview

```txt
i8 <: i8?
i16 <: i16?
i32 <: i32?
i64 <: i64?

u8 <: u8?
u16 <: u16?
u32 <: u32?
u64 <: u64?

f32 <: f32?
f64 <: f64?

bool <: bool?

i8 <: i16
i16 <: i32
i32 <: i64

u8 <: u16
u16 <: u32
u32 <: u64

f32 <: f64

i8? <: i16?
i16? <: i32?
i32? <: i64?

u8? <: u16?
u16? <: u32?
u32? <: u64?

object <: object?

class? <: object?
class <: class?

string? <: object?
string <: object

null <: any

null <: i8?
null <: i16?
null <: i32?
null <: i64?

null <: u8?
null <: u16?
null <: u32?
null <: u64?

null <: f32?
null <: f64?

null <: bool?

null <: object?

Base <: Base?
Derived <: Derived?

If Derived <: Base then Dervied? <: Base?
```
-->