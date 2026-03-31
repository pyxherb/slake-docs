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
$$

#### never_t

`never_t` is the bottom type which has no instance.

$$
\frac{
    \Gamma\vdash \mathrm{T}\ \mathrm{type}
}{
    \Gamma\vdash \mathrm{never\_t} <: \mathrm{T}
}
$$

#### Built-in Arithmetic Types

We have following trait set for the arithmetic types:

$$
\begin{aligned}
\mathrm{ArithmTraits \lang T \rang} ::=&\ \mathrm{IAdd \lang T, T \rang}\\
&\mid\mathrm{ISub \lang T,T \rang}\\
&\mid\mathrm{IMul \lang T,T \rang}\\
&\mid\mathrm{IDiv \lang T,T \rang}\\
&\mid\mathrm{IMod \lang T,T \rang}\\
&\mid\mathrm{IAddAssign \lang T,T \rang}\\
&\mid\mathrm{ISubAssign \lang T,T \rang}\\
&\mid\mathrm{IMulAssign \lang T,T \rang}\\
&\mid\mathrm{IDivAssign \lang T,T \rang}\\
&\mid\mathrm{IModAssign \lang T,T \rang}\\
&\mid\mathrm{IEq \lang T,T \rang}\\
&\mid\mathrm{INeq \lang T,T \rang}\\
&\mid\mathrm{ILt \lang T,T \rang}\\
&\mid\mathrm{IGt \lang T,T \rang}\\
&\mid\mathrm{ILtEq \lang T,T \rang}\\
&\mid\mathrm{IGtEq \lang T,T \rang}\\
&\mid\mathrm{ICmp \lang T,T \rang}\\
&\mid\mathrm{INeg \lang T,T \rang}\\
\end{aligned}
$$

$$
\mathrm{FloatTraits \lang T \rang} ::= \mathrm{ArithmTraits \lang T \rang}\\
$$

$$
\begin{aligned}
\mathrm{IntegerTraits \lang T \rang} ::=&\ \mathrm{ArithmTraits \lang T \rang}\\
&\mid\mathrm{IAnd \lang T,T \rang}\\
&\mid\mathrm{IOr \lang T,T \rang}\\
&\mid\mathrm{IXor \lang T,T \rang}\\
&\mid\mathrm{ILsh \lang T,u32 \rang}\\
&\mid\mathrm{IRsh \lang T,u32 \rang}\\
&\mid\mathrm{IAndAssign \lang T,T \rang}\\
&\mid\mathrm{IOrAssign \lang T,T \rang}\\
&\mid\mathrm{IXorAssign \lang T,T \rang}\\
&\mid\mathrm{ILshAssign \lang T,u32 \rang}\\
&\mid\mathrm{IRshAssign \lang T,u32 \rang}\\
&\mid\mathrm{INot \lang T,T \rang}\\
\end{aligned}
$$

Signed integer types included `i8`, `i16`, `i32` and `i64`:

$$\mathrm{SignedInteger} ::=
\mathrm{i8}
\mid\mathrm{i16}
\mid\mathrm{i32}
\mid\mathrm{i64}
$$

Unsigned integer types included `u8`, `u16`, `u32` and `u64`:

$$\mathrm{UnsignedInteger} ::=
\mathrm{u8}
\mid\mathrm{u16}
\mid\mathrm{u32}
\mid\mathrm{u64}$$

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
    \forall i \in \mathrm{IntegerTraits \lang T \rang},
    \Gamma\vdash \mathrm{T} <: i
}
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
    \forall i \in \mathrm{FloatTraits \lang T \rang},
    \Gamma\vdash \mathrm{T} <: i
}
$$

Every arithmetic type is unconditionally a type:

$$
\frac{
}{
    \forall i \in \mathrm{Arithm},
    \Gamma\vdash i\ \mathrm{type}
}
$$

### Nullable Types

Each nullable type's non-nullable version is its nullable version's parent type:

$$
\frac{
    \Gamma\vdash \mathrm{T}\ \mathrm{type}
}{
    \Gamma\vdash\mathrm{T} <: \mathrm{T?}
}
$$

If a type is a subtype of another type, the type's nullable version is subtype of another type's nullable version:

$$
\frac{
    \Gamma\vdash \mathrm{T_1} <: \mathrm{T_2}
}{
    \Gamma\vdash \mathrm{T_1?} <: \mathrm{T_2?}
}
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

Type operations between generic types are prohibited, which means if there is a generic type $\mathrm{T} \lang x \rang$ and if $x$ has not been filled yet, then the type operations for the type $\mathrm{T}$ are not available.

Unlike some programming languages, the instantiated generic types are **invariant** even the type parameters are covariant or contravariant:

$$
\Gamma\vdash\mathrm{Derived} <: \mathrm{Base} \nRightarrow
\Gamma\vdash\mathrm{T \lang Derived \rang} <: \mathrm{T \lang Base \rang}
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
    A <: B \mid B <: A
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
    \text{\_\_int\_width}(A) \le \text{\_\_int\_width}(B)
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
    \text{\_\_int\_width}(A) > \text{\_\_int\_width}(B)
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
    \text{\_\_int\_width}(A) \le \text{\_\_int\_width}(B)
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
    \text{\_\_int\_width}(A) > \text{\_\_int\_width}(B)
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
    \text{\_\_float\_width}(A) \le \text{\_\_float\_width}(B)
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
    \text{\_\_float\_width}(A) > \text{\_\_float\_width}(B)
}{
    \text{CommonType}(A, B) = A
}
$$