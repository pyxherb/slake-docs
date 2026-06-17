# About The Type System

## Type Relationships

### Built-in Types

#### any

`any` is the top type which can contain an instance of any type.

$$
\frac{
    \Gamma\vdash \mathrm{T}\ \mathrm{type}
}{
    \Gamma\vdash \mathrm{T} <: \mathrm{any}
}
(\text{T-AnyTop})
$$

#### never_t

`never_t` is the bottom type which has no instance.

$$
\frac{
    \Gamma\vdash \mathrm{T}\ \mathrm{type}
}{
    \Gamma\vdash \mathrm{never\_t} <: \mathrm{T}
}
(\text{T-NeverBottom})
$$

#### Built-in Arithmetic Types

We have following trait set for the arithmetic types:

$$
\begin{aligned}
\mathrm{ArithmTraits::<T>} ::=&\ \mathrm{IAdd::<T, T>}\\
&\mid\mathrm{ISub::<T,T>}\\
&\mid\mathrm{IMul::<T,T>}\\
&\mid\mathrm{IDiv::<T,T>}\\
&\mid\mathrm{IMod::<T,T>}\\
&\mid\mathrm{IAddAssign::<T,T>}\\
&\mid\mathrm{ISubAssign::<T,T>}\\
&\mid\mathrm{IMulAssign::<T,T>}\\
&\mid\mathrm{IDivAssign::<T,T>}\\
&\mid\mathrm{IModAssign::<T,T>}\\
&\mid\mathrm{IEq::<T,T>}\\
&\mid\mathrm{INeq::<T,T>}\\
&\mid\mathrm{ILt::<T,T>}\\
&\mid\mathrm{IGt::<T,T>}\\
&\mid\mathrm{ILtEq::<T,T>}\\
&\mid\mathrm{IGtEq::<T,T>}\\
&\mid\mathrm{ICmp::<T,T>}\\
&\mid\mathrm{INeg::<T,T>}\\
\end{aligned}
$$

$$
\mathrm{FloatTraits::<T>} ::= \mathrm{ArithmTraits::<T>}\\
$$

$$
\begin{aligned}
\mathrm{IntegerTraits::<T>} ::=&\ \mathrm{ArithmTraits::<T>}\\
&\mid\mathrm{IAnd::<T,T>}\\
&\mid\mathrm{IOr::<T,T>}\\
&\mid\mathrm{IXor::<T,T>}\\
&\mid\mathrm{ILsh::<T,u32>}\\
&\mid\mathrm{IRsh::<T,u32>}\\
&\mid\mathrm{IAndAssign::<T,T>}\\
&\mid\mathrm{IOrAssign::<T,T>}\\
&\mid\mathrm{IXorAssign::<T,T>}\\
&\mid\mathrm{ILshAssign::<T,u32>}\\
&\mid\mathrm{IRshAssign::<T,u32>}\\
&\mid\mathrm{INot::<T,T>}\\
\end{aligned}
$$

Signed integer types included `i8`, `i16`, `i32` and `i64`:

$$
\mathrm{SignedInteger} ::=
\mathrm{i8}
\mid\mathrm{i16}
\mid\mathrm{i32}
\mid\mathrm{i64}
\mid\mathrm{isize}
$$

Unsigned integer types included `u8`, `u16`, `u32` and `u64`:

$$
\mathrm{UnsignedInteger} ::=
\mathrm{u8}
\mid\mathrm{u16}
\mid\mathrm{u32}
\mid\mathrm{u64}
\mid\mathrm{usize}
$$

For unsigned integer types, their relationship is defined as following:

Integer types included the signed integer types and the unsigned integer types:

$$
\mathrm{Integer} ::= \mathrm{SignedInteger}\mid\mathrm{UnsignedInteger}
$$

Every integer type is subtype of all types of the `IntegerTraits`.

$$
\frac{
    \Gamma\vdash \mathrm{T}\ \mathrm{type}
    \quad
    \mathrm{T} \in \mathrm{Integer}
}{
    \forall i \in \mathrm{IntegerTraits::<T>},
    \Gamma\vdash \mathrm{T} <: i
}
(\text{T-IntegerTraits})
$$

Floating-point types includes `f32` and `f64`:

$$
\mathrm{Float} ::=
\mathrm{f32}
\mid\mathrm{f64}
$$

Every floating-point type is subtype of all types of the `FloatTraits`.

$$
\frac{
    \Gamma\vdash \mathrm{T}\ \mathrm{type}
    \quad
    \mathrm{T} \in \mathrm{Float}
}{
    \forall i \in \mathrm{FloatTraits::<T>},
    \Gamma\vdash \mathrm{T} <: i
}
(\text{T-FloatTraits})
$$

Arithmetic types include $\mathrm{Float}$ and $\mathrm{Integer}$ types.

$$
\mathrm{Arithm} ::=
\mathrm{Float}
\mid\mathrm{Integer}
$$

### Fundamental Types

Fundamental types included arithmetic types and `bool`.

$$
\mathrm{Fundamental} ::=
\mathrm{Arithm}
\mid\mathrm{bool}
$$

All of fundamental type are unconditionally a type:

$$
\frac{
}{
    \forall i \in \mathrm{Fundamental},
    \Gamma\vdash i\ \mathrm{type}
}
(\text{T-FundamentalTypesAreType})
$$

### Nullable Types

Each nullable type's non-nullable version is its nullable version's parent type:

$$
\frac{
    \Gamma\vdash \mathrm{T}\ \mathrm{type}
}{
    \Gamma\vdash\mathrm{T} <: \mathrm{T?}
}
(\text{T-NullableSub})
$$

If a type is a subtype of another type, the type's nullable version is subtype of another type's nullable version:

$$
\frac{
    \Gamma\vdash \mathrm{T_1} <: \mathrm{T_2}
}{
    \Gamma\vdash \mathrm{T_1?} <: \mathrm{T_2?}
}
(\text{T-SubNullableSub})
$$

and `T?` is not `P`'s subtype where the `T` is subtype of `P`.

$$
\Gamma\vdash\mathrm{T} <: \mathrm{P} \nRightarrow \Gamma\vdash\mathrm{T?} <: \mathrm{P}
$$

`null` is subtype of all nullable types:

$$
\frac{
    \Gamma\vdash \mathrm{T}\ \mathrm{type}
}{
    \Gamma\vdash\mathrm{null} <: \mathrm{T?}
}
(\text{T-Null})
$$

### Array Types

Array types are invariant, which means there's no subtyping relationship between different array types:

$$
\Gamma\vdash\mathrm{Derived} <: \mathrm{Base} \nRightarrow
\Gamma\vdash\mathrm{Derived[]} <: \mathrm{Base[]}
$$

### Variable Reference Types

Variable reference types are also invariant:

$$
\Gamma\vdash\mathrm{Derived} <: \mathrm{Base} \nRightarrow
\Gamma\vdash\mathrm{Derived\And} <: \mathrm{Base\And}
$$

### Generic and Instantiated Generic Types

Instantiated generic types, or monomorphized types are generated (or monomorphized) from generic types.

Type operations between generic types are prohibited, which means if there is a generic type $\mathrm{T}::<x>$ and if $x$ has not been filled yet, then the type operations for the type $\mathrm{T}$ are not available.

Unlike some programming languages, the instantiated generic types are **invariant** even the type parameters are covariant or contravariant:

$$
\Gamma\vdash\mathrm{Derived} <: \mathrm{Base} \nRightarrow
\Gamma\vdash\mathrm{T::<Derived>} <: \mathrm{T::<Base>}
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

The Common Result Type Function $\text{CommonResult}(A, B)$ infers the common result value type between two result types.

$$
\frac{
    \Gamma\vdash \mathrm{T}\ \mathrm{type}
    \quad
    \Gamma\vdash \mathrm{B}\ \mathrm{type}
    \quad
    A <: B \vee B <: A
}{
    \text{CommonType}(A, B) = LUB(A,B)
}
$$

$$
\frac{
    \mathrm{A} \in \mathrm{UnsignedInteger}
    \quad
    \mathrm{B} \in \mathrm{UnsignedInteger}
    \quad
    \mathrm{A} \not<: \mathrm{usize}
    \quad
    \mathrm{A} \not<: \mathrm{usize}
    \quad
    \text{\$int\_width}(A) \le \text{\$int\_width}(B)
}{
    \text{CommonType}(A, B) = B
}
$$

$$
\frac{
    \mathrm{A} \in \mathrm{UnsignedInteger}
    \quad
    \mathrm{B} \in \mathrm{UnsignedInteger}
    \quad
    \mathrm{A} \not<: \mathrm{usize}
    \quad
    \mathrm{B} \not<: \mathrm{usize}
    \quad
    \text{\$int\_width}(A) > \text{\$int\_width}(B)
}{
    \text{CommonType}(A, B) = A
}
$$

$$
\frac{
    \mathrm{A} \in \mathrm{SignedInteger}
    \quad
    \mathrm{B} \in \mathrm{SignedInteger}
    \quad
    \mathrm{A} \not<: \mathrm{isize}
    \quad
    \mathrm{B} \not<: \mathrm{isize}
    \quad
    \text{\$int\_width}(A) \le \text{\$int\_width}(B)
}{
    \text{CommonType}(A, B) = B
}
$$

$$
\frac{
    \mathrm{A} \in \mathrm{SignedInteger}
    \quad
    \mathrm{B} \in \mathrm{SignedInteger}
    \quad
    \mathrm{A} \not<: \mathrm{isize}
    \quad
    \mathrm{B} \not<: \mathrm{isize}
    \quad
    \text{\$int\_width}(A) > \text{\$int\_width}(B)
}{
    \text{CommonType}(A, B) = A
}
$$

$$
\frac{
    \mathrm{A} \in \mathrm{Float}
    \quad
    \mathrm{B} \in \mathrm{Float}
    \quad
    \text{\$float\_width}(A) \le \text{\$float\_width}(B)
}{
    \text{CommonType}(A, B) = B
}
$$

$$
\frac{
    \mathrm{A} \in \mathrm{Float}
    \quad
    \mathrm{B} \in \mathrm{Float}
    \quad
    \text{\$float\_width}(A) > \text{\$float\_width}(B)
}{
    \text{CommonType}(A, B) = A
}
$$
