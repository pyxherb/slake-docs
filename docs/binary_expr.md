# Binary Expression

## Auxiliary Definitions

$\text{BinaryExprMainOpType}(A, B)$ infers the main operation type of a common binary expression between types of two undecorated operands.

If one side of the operand is nullable, choose that side, if both operands are nullable, choose the left one.

$$
\frac{
    \Gamma\vdash\mathrm{A}\ \mathrm{type}
    \quad
    \Gamma\vdash\mathrm{B}\ \mathrm{type}
}{
    \text{BinaryExprMainOpType}(\mathrm{A?}, \mathrm{B}) = \mathrm{A}
}
$$

$$
\frac{
    \Gamma\vdash\mathrm{A}\ \mathrm{type}
    \quad
    \Gamma\vdash\mathrm{B}\ \mathrm{type}
}{
    \text{BinaryExprMainOpType}(\mathrm{A}, \mathrm{B?}) = \mathrm{B}
}
$$

If type of one side is a custom type, use that side:

$$
\frac{
    \mathrm{A} <: \mathrm{class}
    \vee
    \mathrm{A} <: \mathrm{struct}
    \vee
    \mathrm{A} <: \mathrm{interface}
}{
    \text{BinaryExprMainOpType}(\mathrm{A}, \mathrm{B}) = \mathrm{A}
}
$$

$$
\frac{
    \mathrm{B} <: \mathrm{class}
    \vee
    \mathrm{B} <: \mathrm{struct}
    \vee
    \mathrm{A} <: \mathrm{interface}
}{
    \text{BinaryExprMainOpType}(\mathrm{A}, \mathrm{B}) = \mathrm{B}
}
$$

Or, if type of one side is $\text{bool}$, use $\text{bool}$:

$$
\frac{
    \mathrm{A} <: \mathrm{bool}
    \quad
    \mathrm{B} \not<: \mathrm{class}
    \quad
    \mathrm{B} \not<: \mathrm{struct}
}{
    \text{BinaryExprMainOpType}(\mathrm{A}, \mathrm{B}) = \mathrm{A}
}
$$

$$
\frac{
    \mathrm{B} <: \mathrm{bool}
    \quad
    \mathrm{A} \not<: \mathrm{class}
    \quad
    \mathrm{A} \not<: \mathrm{struct}
}{
    \text{BinaryExprMainOpType}(\mathrm{A}, \mathrm{B}) = \mathrm{B}
}
$$

Or, if both operand types are $\mathrm{UnsignedInteger}$, $\mathrm{SignedInteger}$ or $\mathrm{Float}$ type, choose the wider one.

$$
\frac{
    \mathrm{A} \in \mathrm{UnsignedInteger}
    \quad
    \mathrm{B} \in \mathrm{UnsignedInteger}
}{
    \text{BinaryExprMainOpType}(\mathrm{A}, \mathrm{B}) = \text{CommonType}(\mathrm{A}, \mathrm{B})
}
$$

$$
\frac{
    \mathrm{A} \in \mathrm{SignedInteger}
    \quad
    \mathrm{B} \in \mathrm{SignedInteger}
}{
    \text{BinaryExprMainOpType}(\mathrm{A}, \mathrm{B}) = \text{CommonType}(\mathrm{A}, \mathrm{B})
}
$$

$$
\frac{
    \mathrm{A} \in \mathrm{Float}
    \quad
    \mathrm{B} \in \mathrm{Float}
}{
    \text{BinaryExprMainOpType}(\mathrm{A}, \mathrm{B}) = \text{CommonType}(\mathrm{A}, \mathrm{B})
}
$$

Or, if the type of RHS can be converted to type of LHS, use type of LHS.

$$
\frac{
    \Gamma\vdash\mathrm{A}\ \mathrm{type}
    \quad
    \Gamma\vdash\mathrm{B}\ \mathrm{type}
    \quad
    \text{IsConvertible}(\mathrm{B}, \mathrm{A}) = \text{true}
}{
    \text{BinaryExprMainOpType}(\mathrm{A}, \mathrm{B}) = \mathrm{A}
}
$$

Otherwise, use type of RHS.

$$
\frac{
    \text{otherwise}
}{
    \text{BinaryExprMainOpType}(\mathrm{A}, \mathrm{B}) = \mathrm{B}
}
$$

### Examples

$$
\text{BinaryExprMainOpType}(\mathrm{i16}, \mathrm{i32}) = \mathrm{i32}
$$

$$
\text{BinaryExprMainOpType}(\mathrm{u8}, \mathrm{u16}) = \mathrm{u16}
$$

$$
\text{BinaryExprMainOpType}(\mathrm{f32}, \mathrm{f64}) = \mathrm{f64}
$$

$$
\text{BinaryExprMainOpType}(\mathrm{i32}, \mathrm{isize}) = \mathrm{i32}
$$

$$
\text{BinaryExprMainOpType}(\mathrm{isize}, \mathrm{i32}) = \mathrm{isize}
$$

$$
\text{BinaryExprMainOpType}(\mathrm{i8}, \mathrm{bool}) = \mathrm{bool}
$$

$$
\text{BinaryExprMainOpType}(\mathrm{std.interfaces.IAdd::<i32, i32>}, \mathrm{i32}) = \mathrm{std.interfaces.IAdd::<i32, i32>}
$$

$$
\text{BinaryExprMainOpType}(\mathrm{object}, \mathrm{i32}) = \mathrm{object}
$$

## Adding Expression

$$
\frac{
    \Gamma\vdash e_1: \mathrm{T}
    \quad
    \Gamma\vdash e_2: \mathrm{T}
    \quad
    \mathrm{T} \in \mathrm{Arithm}
}{
    \Gamma\vdash e_1 + e_2: \mathrm{T}
}
(\text{T-Add})
$$

$$
\frac{
    \Gamma\vdash e_1: \mathrm{T_1}
    \quad
    \Gamma\vdash e_2: \mathrm{T_2}
    \quad
    \text{BinaryExprMainOpType}(\mathrm{T_1}, \mathrm{T_2}) = \mathrm{T}
    \quad
    \mathrm{T} \in \mathrm{Arithm}
}{
    \Gamma\vdash e_1 + e_2
    \triangleq
    (e_1\ \text{as}\ \mathrm{T}) + (e_2\ \text{as}\ \mathrm{T})
}
$$

$$
\frac{
    e_1 \rightarrow v_1
}{
    e_1 + e_2 \rightarrow v_1 + e_2
}
(\text{E-AddReduce1})
$$

$$
\frac{
    v_1 \text{ is a value}
    \quad
    e_2 \rightarrow v_2
}{
    v_1 + e_2 \rightarrow v_1 + v_2
}
(\text{E-AddReduce2})
$$

$$
\frac{
    \text{\_\_typeof}(v_1) = \mathrm{T}
    \quad
    \text{\_\_typeof}(v_2) = \mathrm{T}
    \quad
    \mathrm{T} \in \mathrm{Arithm}
    \quad
    v_1 \text{ is a value}
    \quad
    v_2 \text{ is a value}
}{
    v_1 + v_2 \rightarrow \text{\_\_add}(\mathrm{T}, v_1, v_2)
}
(\text{E-Add})
$$

## Subtraction Expression

$$
\frac{
    \Gamma\vdash e_1: \mathrm{T}
    \quad
    \Gamma\vdash e_2: \mathrm{T}
    \quad
    \mathrm{T} \in \mathrm{Arithm}
}{
    \Gamma\vdash e_1 - e_2: \mathrm{T}
}
(\text{T-Sub})
$$

$$
\frac{
    \Gamma\vdash e_1: \mathrm{T_1}
    \quad
    \Gamma\vdash e_2: \mathrm{T_2}
    \quad
    \text{BinaryExprMainOpType}(\mathrm{T_1}, \mathrm{T_2}) = \mathrm{T}
    \quad
    \mathrm{T} \in \mathrm{Arithm}
}{
    \Gamma\vdash e_1 - e_2
    \triangleq
    (e_1\ \text{as}\ \mathrm{T}) - (e_2\ \text{as}\ \mathrm{T})
}
$$

$$
\frac{
    e_1 \rightarrow v_1
}{
    e_1 - e_2 \rightarrow v_1 - e_2
}
(\text{E-SubReduce1})
$$

$$
\frac{
    v_1 \text{ is a value}
    \quad
    e_2 \rightarrow v_2
}{
    v_1 - e_2 \rightarrow v_1 - v_2
}
(\text{E-SubReduce2})
$$

$$
\frac{
    \text{\_\_typeof}(v_1) = \mathrm{T}
    \quad
    \text{\_\_typeof}(v_2) = \mathrm{T}
    \quad
    \mathrm{T} \in \mathrm{Arithm}
    \quad
    v_1 \text{ is a value}
    \quad
    v_2 \text{ is a value}
}{
    v_1 - v_2 \rightarrow \text{\_\_sub}(\mathrm{T}, v_1, v_2)
}
(\text{E-Sub})
$$

## Multiplication Expression

$$
\frac{
    \Gamma\vdash e_1: \mathrm{T}
    \quad
    \Gamma\vdash e_2: \mathrm{T}
    \quad
    \mathrm{T} \in \mathrm{Arithm}
}{
    \Gamma\vdash e_1 * e_2: \mathrm{T}
}
(\text{T-Mul})
$$

$$
\frac{
    \Gamma\vdash e_1: \mathrm{T_1}
    \quad
    \Gamma\vdash e_2: \mathrm{T_2}
    \quad
    \text{BinaryExprMainOpType}(\mathrm{T_1}, \mathrm{T_2}) = \mathrm{T}
    \quad
    \mathrm{T} \in \mathrm{Arithm}
}{
    \Gamma\vdash e_1 * e_2
    \triangleq
    (e_1\ \text{as}\ \mathrm{T}) * (e_2\ \text{as}\ \mathrm{T})
}
$$

$$
\frac{
    e_1 \rightarrow v_1
}{
    e_1 * e_2 \rightarrow v_1 * e_2
}
(\text{E-MulReduce1})
$$

$$
\frac{
    v_1 \text{ is a value}
    \quad
    e_2 \rightarrow v_2
}{
    v_1 * e_2 \rightarrow v_1 * v_2
}
(\text{E-MulReduce2})
$$

$$
\frac{
    \text{\_\_typeof}(v_1) = \mathrm{T}
    \quad
    \text{\_\_typeof}(v_2) = \mathrm{T}
    \quad
    \mathrm{T} \in \mathrm{Arithm}
    \quad
    v_1 \text{ is a value}
    \quad
    v_2 \text{ is a value}
}{
    v_1 * v_2 \rightarrow \text{\_\_mul}(\mathrm{T}, v_1, v_2)
}
(\text{E-Mul})
$$

## Division Expression

$$
\frac{
    \Gamma\vdash e_1: \mathrm{T}
    \quad
    \Gamma\vdash e_2: \mathrm{T}
    \quad
    \mathrm{T} \in \mathrm{Arithm}
}{
    \Gamma\vdash e_1 / e_2: \mathrm{T}
}
(\text{T-Div})
$$

$$
\frac{
    \Gamma\vdash e_1: \mathrm{T_1}
    \quad
    \Gamma\vdash e_2: \mathrm{T_2}
    \quad
    \text{BinaryExprMainOpType}(\mathrm{T_1}, \mathrm{T_2}) = \mathrm{T}
    \quad
    \mathrm{T} \in \mathrm{Arithm}
}{
    \Gamma\vdash e_1 / e_2
    \triangleq
    (e_1\ \text{as}\ \mathrm{T}) / (e_2\ \text{as}\ \mathrm{T})
}
$$

$$
\frac{
    e_1 \rightarrow v_1
}{
    e_1 / e_2 \rightarrow v_1 / e_2
}
(\text{E-DivReduce1})
$$

$$
\frac{
    v_1 \text{ is a value}
    \quad
    e_2 \rightarrow v_2
}{
    v_1 / e_2 \rightarrow v_1 / v_2
}
(\text{E-DivReduce2})
$$

$$
\frac{
    \text{\_\_typeof}(v_1) = \mathrm{T}
    \quad
    \text{\_\_typeof}(v_2) = \mathrm{T}
    \quad
    \mathrm{T} \in \mathrm{Arithm}
    \quad
    v_1 \text{ is a value}
    \quad
    v_2 \text{ is a value}
}{
    v_1 / v_2 \rightarrow \text{\_\_div}(\mathrm{T}, v_1, v_2)
}
(\text{E-Div})
$$

## Modulo Expression

$$
\frac{
    \Gamma\vdash e_1: \mathrm{T}
    \quad
    \Gamma\vdash e_2: \mathrm{T}
    \quad
    \mathrm{T} \in \mathrm{Arithm}
}{
    \Gamma\vdash e_1 \% e_2: \mathrm{T}
}
(\text{T-Mod})
$$

$$
\frac{
    \Gamma\vdash e_1: \mathrm{T_1}
    \quad
    \Gamma\vdash e_2: \mathrm{T_2}
    \quad
    \text{BinaryExprMainOpType}(\mathrm{T_1}, \mathrm{T_2}) = \mathrm{T}
    \quad
    \mathrm{T} \in \mathrm{Arithm}
}{
    \Gamma\vdash e_1 \% e_2
    \triangleq
    (e_1\ \text{as}\ \mathrm{T}) \% (e_2\ \text{as}\ \mathrm{T})
}
$$

$$
\frac{
    e_1 \rightarrow v_1
}{
    e_1 \% e_2 \rightarrow v_1 \% e_2
}
(\text{E-ModReduce1})
$$

$$
\frac{
    v_1 \text{ is a value}
    \quad
    e_2 \rightarrow v_2
}{
    v_1 \% e_2 \rightarrow v_1 \% v_2
}
(\text{E-ModReduce2})
$$

$$
\frac{
    \text{\_\_typeof}(v_1) = \mathrm{T}
    \quad
    \text{\_\_typeof}(v_2) = \mathrm{T}
    \quad
    \mathrm{T} \in \mathrm{Arithm}
    \quad
    v_1 \text{ is a value}
    \quad
    v_2 \text{ is a value}
}{
    v_1 \% v_2 \rightarrow \text{\_\_mod}(\mathrm{T}, v_1, v_2)
}
(\text{E-Mod})
$$

## Bitwise AND Expression

$$
\frac{
    \Gamma\vdash e_1: \mathrm{T}
    \quad
    \Gamma\vdash e_2: \mathrm{T}
    \quad
    \mathrm{T} \in \mathrm{Integer}
}{
    \Gamma\vdash e_1 \And e_2: \mathrm{T}
}
(\text{T-And})
$$

$$
\frac{
    \Gamma\vdash e_1: \mathrm{T_1}
    \quad
    \Gamma\vdash e_2: \mathrm{T_2}
    \quad
    \text{BinaryExprMainOpType}(\mathrm{T_1}, \mathrm{T_2}) = \mathrm{T}
    \quad
    \mathrm{T} \in \mathrm{Integer}
}{
    \Gamma\vdash e_1 \And e_2
    \triangleq
    (e_1\ \text{as}\ \mathrm{T}) \And (e_2\ \text{as}\ \mathrm{T})
}
$$

$$
\frac{
    e_1 \rightarrow v_1
}{
    e_1 \And e_2 \rightarrow v_1 \And e_2
}
(\text{E-AndReduce1})
$$

$$
\frac{
    v_1 \text{ is a value}
    \quad
    e_2 \rightarrow v_2
}{
    v_1 \And e_2 \rightarrow v_1 \And v_2
}
(\text{E-AndReduce2})
$$

$$
\frac{
    \text{\_\_typeof}(v_1) = \mathrm{T}
    \quad
    \text{\_\_typeof}(v_2) = \mathrm{T}
    \quad
    \mathrm{T} \in \mathrm{Integer}
    \quad
    v_1 \text{ is a value}
    \quad
    v_2 \text{ is a value}
}{
    v_1 \And v_2 \rightarrow \text{\_\_and}(\mathrm{T}, v_1, v_2)
}
(\text{E-And})
$$

## Bitwise OR Expression

$$
\frac{
    \Gamma\vdash e_1: \mathrm{T}
    \quad
    \Gamma\vdash e_2: \mathrm{T}
    \quad
    \mathrm{T} \in \mathrm{Integer}
}{
    \Gamma\vdash e_1 | e_2: \mathrm{T}
}
(\text{T-Or})
$$

$$
\frac{
    \Gamma\vdash e_1: \mathrm{T_1}
    \quad
    \Gamma\vdash e_2: \mathrm{T_2}
    \quad
    \text{BinaryExprMainOpType}(\mathrm{T_1}, \mathrm{T_2}) = \mathrm{T}
    \quad
    \mathrm{T} \in \mathrm{Integer}
}{
    \Gamma\vdash e_1 | e_2
    \triangleq
    (e_1\ \text{as}\ \mathrm{T}) | (e_2\ \text{as}\ \mathrm{T})
}
$$

$$
\frac{
    e_1 \rightarrow v_1
}{
    e_1 | e_2 \rightarrow v_1 | e_2
}
(\text{E-OrReduce1})
$$

$$
\frac{
    v_1 \text{ is a value}
    \quad
    e_2 \rightarrow v_2
}{
    v_1 | e_2 \rightarrow v_1 | v_2
}
(\text{E-OrReduce2})
$$

$$
\frac{
    \text{\_\_typeof}(v_1) = \mathrm{T}
    \quad
    \text{\_\_typeof}(v_2) = \mathrm{T}
    \quad
    \mathrm{T} \in \mathrm{Integer}
    \quad
    v_1 \text{ is a value}
    \quad
    v_2 \text{ is a value}
}{
    v_1 | v_2 \rightarrow \text{\_\_or}(\mathrm{T}, v_1, v_2)
}
(\text{E-Or})
$$

## Bitwise XOR Expression

$$
\frac{
    \Gamma\vdash e_1: \mathrm{T}
    \quad
    \Gamma\vdash e_2: \mathrm{T}
    \quad
    \mathrm{T} \in \mathrm{Integer}
}{
    \Gamma\vdash e_1 \wedge e_2: \mathrm{T}
}
(\text{T-Xor})
$$

$$
\frac{
    \Gamma\vdash e_1: \mathrm{T_1}
    \quad
    \Gamma\vdash e_2: \mathrm{T_2}
    \quad
    \text{BinaryExprMainOpType}(\mathrm{T_1}, \mathrm{T_2}) = \mathrm{T}
    \quad
    \mathrm{T} \in \mathrm{Integer}
}{
    \Gamma\vdash e_1 \wedge e_2
    \triangleq
    (e_1\ \text{as}\ \mathrm{T}) \wedge (e_2\ \text{as}\ \mathrm{T})
}
$$

$$
\frac{
    e_1 \rightarrow v_1
}{
    e_1 \wedge e_2 \rightarrow v_1 \wedge e_2
}
(\text{E-XorReduce1})
$$

$$
\frac{
    v_1 \text{ is a value}
    \quad
    e_2 \rightarrow v_2
}{
    v_1 \wedge e_2 \rightarrow v_1 \wedge v_2
}
(\text{E-XorReduce2})
$$

$$
\frac{
    \text{\_\_typeof}(v_1) = \mathrm{T}
    \quad
    \text{\_\_typeof}(v_2) = \mathrm{T}
    \quad
    \mathrm{T} \in \mathrm{Integer}
    \quad
    v_1 \text{ is a value}
    \quad
    v_2 \text{ is a value}
}{
    v_1 \wedge v_2 \rightarrow \text{\_\_xor}(\mathrm{T}, v_1, v_2)
}
(\text{E-Xor})
$$

## Left-Shift Expression

$$
\frac{
    \Gamma\vdash e_1: \mathrm{T}
    \quad
    \Gamma\vdash e_2: \mathrm{u32}
    \quad
    \mathrm{T} \in \mathrm{Integer}
}{
    \Gamma\vdash e_1 << e_2: \mathrm{T}
}
(\text{T-Shl})
$$

$$
\frac{
    \Gamma\vdash e_1: \mathrm{T_1}
    \quad
    \Gamma\vdash e_2: \mathrm{T_2}\quad
    \mathrm{T_1} \in \mathrm{Integer}
}{
    \Gamma\vdash e_1 << e_2
    \triangleq
    e_1 << (e_2\ \text{as}\ \mathrm{u32})
}
$$

$$
\frac{
    e_1 \rightarrow v_1
}{
    e_1 << e_2 \rightarrow v_1 << e_2
}
(\text{E-ShlReduce1})
$$

$$
\frac{
    v_1 \text{ is a value}
    \quad
    e_2 \rightarrow v_2
}{
    v_1 << e_2 \rightarrow v_1 << v_2
}
(\text{E-ShlReduce2})
$$

$$
\frac{
    \text{\_\_typeof}(v_1) = \mathrm{T}
    \quad
    \text{\_\_typeof}(v_2) = \mathrm{T}
    \quad
    \mathrm{T} \in \mathrm{Integer}
    \quad
    v_1 \text{ is a value}
    \quad
    v_2 \text{ is a value}
    
}{
    v_1 << v_2 \rightarrow \text{\_\_shl}(\mathrm{T}, v_1, v_2)
}
(\text{E-Shl})
$$

## Right-Shift Expression

$$
\frac{
    \Gamma\vdash e_1: \mathrm{T}
    \quad
    \Gamma\vdash e_2: \mathrm{u32}
    \quad
    \mathrm{T} \in \mathrm{Integer}
}{
    \Gamma\vdash e_1 >> e_2: \mathrm{T}
}
(\text{T-Shr})
$$

$$
\frac{
    \Gamma\vdash e_1: \mathrm{T_1}
    \quad
    \Gamma\vdash e_2: \mathrm{T_2}
    \quad
    \mathrm{T_1} \in \mathrm{Integer}
}{
    \Gamma\vdash e_1 >> e_2
    \triangleq
    e_1 >> (e_2\ \text{as}\ \mathrm{u32})
}
$$

$$
\frac{
    e_1 \rightarrow v_1
}{
    e_1 >> e_2 \rightarrow v_1 >> e_2
}
(\text{E-ShrReduce1})
$$

$$
\frac{
    v_1 \text{ is a value}
    \quad
    e_2 \rightarrow v_2
}{
    v_1 >> e_2 \rightarrow v_1 >> v_2
}
(\text{E-ShrReduce2})
$$

$$
\frac{
    \text{\_\_typeof}(v_1) = \mathrm{T}
    \quad
    \text{\_\_typeof}(v_2) = \mathrm{T}
    \quad
    \mathrm{T} \in \mathrm{Integer}
    \quad
    v_1 \text{ is a value}
    \quad
    v_2 \text{ is a value}
}{
    v_1 >> v_2 \rightarrow \text{\_\_shr}(\mathrm{T}, v_1, v_2)
}
(\text{E-Shr})
$$

## Logical AND Expression

$$
\frac{
    \Gamma\vdash e_1: \mathrm{bool}
    \quad
    \Gamma\vdash e_2: \mathrm{bool}
}{
    \Gamma\vdash e_1 \&\& e_2: \mathrm{bool}
}
(\text{T-LAnd})
$$

$$
\frac{
    \Gamma\vdash e_1: \mathrm{T_1}
    \quad
    \Gamma\vdash e_2: \mathrm{T_2}
    \quad
    \text{BinaryExprMainOpType}(\mathrm{T_1}, \mathrm{T_2}) = \mathrm{bool}
}{
    \Gamma\vdash e_1 \&\& e_2
    \triangleq
    (e_1\ \text{as}\ \mathrm{bool}) \&\& (e_2\ \text{as}\ \mathrm{bool})
}
$$

$$
\frac{
    e_1 \rightarrow v_1
}{
    e_1 \&\& e_2 \rightarrow v_1 \&\& e_2
}
(\text{E-LAndReduce1})
$$

$$
\frac{
    v_1 = \text{true}
    \quad
    e_2 \rightarrow v_2
}{
    v_1 \&\& e_2 \rightarrow v_2
}
(\text{E-LAndReduce2})
$$

$$
\frac{
    v_1 = \text{false}
}{
    v_1 \&\& e_2 \rightarrow v_1
}
(\text{E-LAndReduce3})
$$

## Logical OR Expression

$$
\frac{
    \Gamma\vdash e_1: \mathrm{bool}
    \quad
    \Gamma\vdash e_2: \mathrm{bool}
}{
    \Gamma\vdash e_1 || e_2: \mathrm{bool}
}
(\text{T-LOr})
$$

$$
\frac{
    \Gamma\vdash e_1: \mathrm{T_1}
    \quad
    \Gamma\vdash e_2: \mathrm{T_2}
    \quad
    \text{BinaryExprMainOpType}(\mathrm{T_1}, \mathrm{T_2}) = \mathrm{bool}
}{
    \Gamma\vdash e_1 || e_2
    \triangleq
    (e_1\ \text{as}\ \mathrm{bool}) || (e_2\ \text{as}\ \mathrm{bool})
}
$$

$$
\frac{
    e_1 \rightarrow v_1
}{
    e_1 || e_2 \rightarrow v_1 || e_2
}
(\text{E-LOrReduce1})
$$

$$
\frac{
    v_1 = \text{false}
    \quad
    e_2 \rightarrow v_2
}{
    v_1 || e_2 \rightarrow v_2
}
(\text{E-LOrReduce2})
$$

$$
\frac{
    v_1 = \text{true}
}{
    v_1 || e_2 \rightarrow v_1
}
(\text{E-LOrReduce3})
$$

## Equality Expression

$$
\frac{
    \Gamma\vdash e_1: \mathrm{T}
    \quad
    \Gamma\vdash e_2: \mathrm{T}
    \quad
    \mathrm{T} \in \mathrm{Fundamental}
}{
    \Gamma\vdash e_1 == e_2: \mathrm{bool}
}
(\text{T-Eq})
$$

$$
\frac{
    \Gamma\vdash e_1: \mathrm{T_1}
    \quad
    \Gamma\vdash e_2: \mathrm{T_2}
    \quad
    \text{BinaryExprMainOpType}(\mathrm{T_1}, \mathrm{T_2}) = \mathrm{T}
    \quad
    \mathrm{T} \in \mathrm{Fundamental}
}{
    \Gamma\vdash e_1 == e_2
    \triangleq
    (e_1\ \text{as}\ \mathrm{T}) == (e_2\ \text{as}\ \mathrm{T})
}
$$

$$
\frac{
    e_1 \rightarrow v_1
}{
    e_1 == e_2 \rightarrow v_1 == e_2
}
(\text{E-EqReduce1})
$$

$$
\frac{
    v_1 \text{ is a value}
    \quad
    e_2 \rightarrow v_2
}{
    v_1 == e_2 \rightarrow v_1 == v_2
}
(\text{E-EqReduce2})
$$

$$
\frac{
    \text{\_\_typeof}(v_1) = \mathrm{T}
    \quad
    \text{\_\_typeof}(v_2) = \mathrm{T}
    \quad
    \mathrm{T} \in \mathrm{Arithm}
    \quad
    v_1 \text{ is a value}
    \quad
    v_2 \text{ is a value}
}{
    v_1 == v_2 \rightarrow \text{\_\_eq}(\mathrm{T}, v_1, v_2)
}
(\text{E-Eq})
$$

## Inequality Expression

$$
\frac{
    \Gamma\vdash e_1: \mathrm{T}
    \quad
    \Gamma\vdash e_2: \mathrm{T}
    \quad
    \mathrm{T} \in \mathrm{Fundamental}
}{
    \Gamma\vdash e_1 != e_2: \mathrm{bool}
}
(\text{T-Neq})
$$

$$
\frac{
    \Gamma\vdash e_1: \mathrm{T_1}
    \quad
    \Gamma\vdash e_2: \mathrm{T_2}
    \quad
    \text{BinaryExprMainOpType}(\mathrm{T_1}, \mathrm{T_2}) = \mathrm{T}
    \quad
    \mathrm{T} \in \mathrm{Fundamental}
}{
    \Gamma\vdash e_1 != e_2
    \triangleq
    (e_1\ \text{as}\ \mathrm{T}) != (e_2\ \text{as}\ \mathrm{T})
}
$$

$$
\frac{
    e_1 \rightarrow v_1
}{
    e_1 != e_2 \rightarrow v_1 != e_2
}
(\text{E-NeqReduce1})
$$

$$
\frac{
    v_1 \text{ is a value}
    \quad
    e_2 \rightarrow v_2
}{
    v_1 != e_2 \rightarrow v_1 != v_2
}
(\text{E-NeqReduce2})
$$

$$
\frac{
    \text{\_\_typeof}(v_1) = \mathrm{T}
    \quad
    \text{\_\_typeof}(v_2) = \mathrm{T}
    \quad
    \mathrm{T} \in \mathrm{Arithm}
    \quad
    v_1 \text{ is a value}
    \quad
    v_2 \text{ is a value}
}{
    v_1 != v_2 \rightarrow \text{\_\_neq}(\mathrm{T}, v_1, v_2)
}
(\text{E-Neq})
$$

## Physical/Strict Equality Expression

$$
\frac{
    \Gamma\vdash e_1: \mathrm{T}
    \quad
    \Gamma\vdash e_2: \mathrm{T}
    \quad
    \mathrm{T} <: \mathrm{object}
}{
    \Gamma\vdash e_1 === e_2: \mathrm{bool}
}
(\text{T-StrictEq})
$$

$$
\frac{
    \Gamma\vdash e_1: \mathrm{T_1}
    \quad
    \Gamma\vdash e_2: \mathrm{T_2}
    \quad
    \mathrm{T_1} <: \mathrm{object}
    \quad
    \mathrm{T_2} <: \mathrm{object}
    \quad
    \mathrm{T_2} <: \mathrm{T_1}
}{
    \Gamma\vdash e_1 === e_2
    \triangleq
    (e_1\ \text{as}\ \mathrm{T_1}) === (e_2\ \text{as}\ \mathrm{T_1})
}
$$

$$
\frac{
    \Gamma\vdash e_1: \mathrm{T_1}
    \quad
    \Gamma\vdash e_2: \mathrm{T_2}
    \quad
    \mathrm{T_1} <: \mathrm{object}
    \quad
    \mathrm{T_2} <: \mathrm{object}
    \quad
    \mathrm{T_1} <: \mathrm{T_2}
}{
    \Gamma\vdash e_1 === e_2
    \triangleq
    (e_1\ \text{as}\ \mathrm{T_2}) === (e_2\ \text{as}\ \mathrm{T_2})
}
$$

$$
\frac{
    \Gamma\vdash e_1: \mathrm{T_1}
    \quad
    \Gamma\vdash e_2: \mathrm{T_2}
    \quad
    \mathrm{T_1} \not<: \mathrm{object}
    \vee
    \mathrm{T_2} \not<: \mathrm{object}
}{
    \Gamma\vdash e_1 === e_2
    \triangleq
    e_1 == e_2
}
$$

$$
\frac{
    e_1 \rightarrow v_1
}{
    e_1 === e_2 \rightarrow v_1 === e_2
}
(\text{E-StrictEqReduce1})
$$

$$
\frac{
    v_1 \text{ is a value}
    \quad
    e_2 \rightarrow v_2
}{
    v_1 === e_2 \rightarrow v_1 === v_2
}
(\text{E-StrictEqReduce2})
$$

$$
\frac{
    \text{\_\_typeof}(v_1) = \mathrm{T}
    \quad
    \text{\_\_typeof}(v_2) = \mathrm{T}
    \quad
    \mathrm{T} <: \mathrm{object}
    \quad
    v_1 \text{ is a value}
    \quad
    v_2 \text{ is a value}
}{
    v_1 === v_2 \rightarrow \text{\_\_phy\_eq}(v_1, v_2)
}
(\text{E-StrictEq})
$$

## Physical/Strict Inequality Expression

$$
\frac{
    \Gamma\vdash e_1: \mathrm{T}
    \quad
    \Gamma\vdash e_2: \mathrm{T}
    \quad
    \mathrm{T} <: \mathrm{object}
}{
    \Gamma\vdash e_1 !== e_2: \mathrm{bool}
}
(\text{T-StrictNeq})
$$

$$
\frac{
    \Gamma\vdash e_1: \mathrm{T_1}
    \quad
    \Gamma\vdash e_2: \mathrm{T_2}
    \quad
    \mathrm{T_1} <: \mathrm{object}
    \quad
    \mathrm{T_2} <: \mathrm{object}
    \quad
    \mathrm{T_2} <: \mathrm{T_1}
}{
    \Gamma\vdash e_1 !== e_2
    \triangleq
    (e_1\ \text{as}\ \mathrm{T_1}) !== (e_2\ \text{as}\ \mathrm{T_1})
}
$$

$$
\frac{
    \Gamma\vdash e_1: \mathrm{T_1}
    \quad
    \Gamma\vdash e_2: \mathrm{T_2}
    \quad
    \mathrm{T_1} <: \mathrm{object}
    \quad
    \mathrm{T_2} <: \mathrm{object}
    \quad
    \mathrm{T_1} <: \mathrm{T_2}
}{
    \Gamma\vdash e_1 !== e_2
    \triangleq
    (e_1\ \text{as}\ \mathrm{T_2}) !== (e_2\ \text{as}\ \mathrm{T_2})
}
$$

$$
\frac{
    \Gamma\vdash e_1: \mathrm{T_1}
    \quad
    \Gamma\vdash e_2: \mathrm{T_2}
    \quad
    \mathrm{T_1} \not<: \mathrm{object}
    \vee
    \mathrm{T_2} \not<: \mathrm{object}
}{
    \Gamma\vdash e_1 !== e_2
    \triangleq
    e_1 != e_2
}
$$

$$
\frac{
    e_1 \rightarrow v_1
}{
    e_1 !== e_2 \rightarrow v_1 !== e_2
}
(\text{E-StrictNeqReduce1})
$$

$$
\frac{
    v_1 \text{ is a value}
    \quad
    e_2 \rightarrow v_2
}{
    v_1 !== e_2 \rightarrow v_1 !== v_2
}
(\text{E-StrictNeqReduce2})
$$

$$
\frac{
    \text{\_\_typeof}(v_1) = \mathrm{T}
    \quad
    \text{\_\_typeof}(v_2) = \mathrm{T}
    \quad
    \mathrm{T} <: \mathrm{object}
    \quad
    v_1 \text{ is a value}
    \quad
    v_2 \text{ is a value}
}{
    v_1 !== v_2 \rightarrow \text{\_\_phy\_neq}(v_1, v_2)
}
(\text{E-StrictNeq})
$$

## Less Expression

$$
\frac{
    \Gamma\vdash e_1: \mathrm{T}
    \quad
    \Gamma\vdash e_2: \mathrm{T}
    \quad
    \mathrm{T} \in \mathrm{Arithm}
}{
    \Gamma\vdash e_1 < e_2: \mathrm{bool}
}
(\text{T-Lt})
$$

$$
\frac{
    \Gamma\vdash e_1: \mathrm{T_1}
    \quad
    \Gamma\vdash e_2: \mathrm{T_2}
    \quad
    \text{BinaryExprMainOpType}(\mathrm{T_1}, \mathrm{T_2}) = \mathrm{T}
    \quad
    \mathrm{T} \in \mathrm{Arithm}
}{
    \Gamma\vdash e_1 < e_2
    \triangleq
    (e_1\ \text{as}\ \mathrm{T}) < (e_2\ \text{as}\ \mathrm{T})
}
$$

$$
\frac{
    e_1 \rightarrow v_1
}{
    e_1 < e_2 \rightarrow v_1 < e_2
}
(\text{E-LtReduce1})
$$

$$
\frac{
    v_1 \text{ is a value}
    \quad
    e_2 \rightarrow v_2
}{
    v_1 < e_2 \rightarrow v_1 < v_2
}
(\text{E-LtReduce2})
$$

$$
\frac{
    \text{\_\_typeof}(v_1) = \mathrm{T}
    \quad
    \text{\_\_typeof}(v_2) = \mathrm{T}
    \quad
    \mathrm{T} \in \mathrm{Arithm}
    \quad
    v_1 \text{ is a value}
    \quad
    v_2 \text{ is a value}
}{
    v_1 < v_2 \rightarrow \text{\_\_lt}(\mathrm{T}, v_1, v_2)
}
(\text{E-Lt})
$$

## Greater Expression

$$
\frac{
    \Gamma\vdash e_1: \mathrm{T}
    \quad
    \Gamma\vdash e_2: \mathrm{T}
    \quad
    \mathrm{T} \in \mathrm{Arithm}
}{
    \Gamma\vdash e_1 > e_2: \mathrm{bool}
}
(\text{T-Gt})
$$

$$
\frac{
    \Gamma\vdash e_1: \mathrm{T_1}
    \quad
    \Gamma\vdash e_2: \mathrm{T_2}
    \quad
    \text{BinaryExprMainOpType}(\mathrm{T_1}, \mathrm{T_2}) = \mathrm{T}
    \quad
    \mathrm{T} \in \mathrm{Arithm}
}{
    \Gamma\vdash e_1 > e_2
    \triangleq
    (e_1\ \text{as}\ \mathrm{T}) > (e_2\ \text{as}\ \mathrm{T})
}
$$

$$
\frac{
    e_1 \rightarrow v_1
}{
    e_1 > e_2 \rightarrow v_1 > e_2
}
(\text{E-GtReduce1})
$$

$$
\frac{
    v_1 \text{ is a value}
    \quad
    e_2 \rightarrow v_2
}{
    v_1 > e_2 \rightarrow v_1 > v_2
}
(\text{E-GtReduce2})
$$

$$
\frac{
    \text{\_\_typeof}(v_1) = \mathrm{T}
    \quad
    \text{\_\_typeof}(v_2) = \mathrm{T}
    \quad
    \mathrm{T} \in \mathrm{Arithm}
    \quad
    v_1 \text{ is a value}
    \quad
    v_2 \text{ is a value}
}{
    v_1 > v_2 \rightarrow \text{\_\_gt}(\mathrm{T}, v_1, v_2)
}
(\text{E-Gt})
$$

## Less-Equal Expression

$$
\frac{
    \Gamma\vdash e_1: \mathrm{T}
    \quad
    \Gamma\vdash e_2: \mathrm{T}
    \quad
    \mathrm{T} \in \mathrm{Arithm}
}{
    \Gamma\vdash e_1 <= e_2: \mathrm{bool}
}
(\text{T-LtEq})
$$

$$
\frac{
    \Gamma\vdash e_1: \mathrm{T_1}
    \quad
    \Gamma\vdash e_2: \mathrm{T_2}
    \quad
    \text{BinaryExprMainOpType}(\mathrm{T_1}, \mathrm{T_2}) = \mathrm{T}
    \quad
    \mathrm{T} \in \mathrm{Arithm}
}{
    \Gamma\vdash e_1 <= e_2
    \triangleq
    (e_1\ \text{as}\ \mathrm{T}) <= (e_2\ \text{as}\ \mathrm{T})
}
$$

$$
\frac{
    e_1 \rightarrow v_1
}{
    e_1 <= e_2 \rightarrow v_1 <= e_2
}
(\text{E-LtEqReduce1})
$$

$$
\frac{
    v_1 \text{ is a value}
    \quad
    e_2 \rightarrow v_2
}{
    v_1 <= e_2 \rightarrow v_1 <= v_2
}
(\text{E-LtEqReduce2})
$$

$$
\frac{
    \text{\_\_typeof}(v_1) = \mathrm{T}
    \quad
    \text{\_\_typeof}(v_2) = \mathrm{T}
    \quad
    \mathrm{T} \in \mathrm{Arithm}
    \quad
    v_1 \text{ is a value}
    \quad
    v_2 \text{ is a value}
}{
    v_1 <= v_2 \rightarrow \text{\_\_lteq}(\mathrm{T}, v_1, v_2)
}
(\text{E-LtEq})
$$

## Greater-Equal Expression

$$
\frac{
    \Gamma\vdash e_1: \mathrm{T}
    \quad
    \Gamma\vdash e_2: \mathrm{T}
    \quad
    \mathrm{T} \in \mathrm{Arithm}
}{
    \Gamma\vdash e_1 >= e_2: \mathrm{bool}
}
(\text{T-GtEq})
$$

$$
\frac{
    \Gamma\vdash e_1: \mathrm{T_1}
    \quad
    \Gamma\vdash e_2: \mathrm{T_2}
    \quad
    \text{BinaryExprMainOpType}(\mathrm{T_1}, \mathrm{T_2}) = \mathrm{T}
    \quad
    \mathrm{T} \in \mathrm{Arithm}
}{
    \Gamma\vdash e_1 >= e_2
    \triangleq
    (e_1\ \text{as}\ \mathrm{T}) >= (e_2\ \text{as}\ \mathrm{T})
}
$$

$$
\frac{
    e_1 \rightarrow v_1
}{
    e_1 >= e_2 \rightarrow v_1 >= e_2
}
(\text{E-GtEqReduce1})
$$

$$
\frac{
    v_1 \text{ is a value}
    \quad
    e_2 \rightarrow v_2
}{
    v_1 >= e_2 \rightarrow v_1 >= v_2
}
(\text{E-GtEqReduce2})
$$

$$
\frac{
    \text{\_\_typeof}(v_1) = \mathrm{T}
    \quad
    \text{\_\_typeof}(v_2) = \mathrm{T}
    \quad
    \mathrm{T} \in \mathrm{Arithm}
    \quad
    v_1 \text{ is a value}
    \quad
    v_2 \text{ is a value}
}{
    v_1 >= v_2 \rightarrow \text{\_\_gteq}(\mathrm{T}, v_1, v_2)
}
(\text{E-GtEq})
$$

## Three-way Comparison Expression

$$
\frac{
    \Gamma\vdash e_1: \mathrm{T}
    \quad
    \Gamma\vdash e_2: \mathrm{T}
    \quad
    \mathrm{T} \in \mathrm{Arithm}
}{
    \Gamma\vdash e_1 <=> e_2: \mathrm{bool}
}
(\text{T-Cmp})
$$

$$
\frac{
    \Gamma\vdash e_1: \mathrm{T_1}
    \quad
    \Gamma\vdash e_2: \mathrm{T_2}
    \quad
    \text{BinaryExprMainOpType}(\mathrm{T_1}, \mathrm{T_2}) = \mathrm{T}
    \quad
    \mathrm{T} \in \mathrm{Arithm}
}{
    \Gamma\vdash e_1 <=> e_2
    \triangleq
    (e_1\ \text{as}\ \mathrm{T}) <=> (e_2\ \text{as}\ \mathrm{T})
}
$$

$$
\frac{
    e_1 \rightarrow v_1
}{
    e_1 <=> e_2 \rightarrow v_1 <=> e_2
}
(\text{E-CmpReduce1})
$$

$$
\frac{
    v_1 \text{ is a value}
    \quad
    e_2 \rightarrow v_2
}{
    v_1 <=> e_2 \rightarrow v_1 <=> v_2
}
(\text{E-CmpReduce2})
$$

$$
\frac{
    \text{\_\_typeof}(v_1) = \mathrm{T}
    \quad
    \text{\_\_typeof}(v_2) = \mathrm{T}
    \quad
    \mathrm{T} \in \mathrm{Arithm}
    \quad
    v_1 \text{ is a value}
    \quad
    v_2 \text{ is a value}
    \quad
    v_1 < v_2
}{
    v_1 <=> v_2 \rightarrow \text{1i32}
}
(\text{E-CmpLt})
$$

$$
\frac{
    \text{\_\_typeof}(v_1) = \mathrm{T}
    \quad
    \text{\_\_typeof}(v_2) = \mathrm{T}
    \quad
    \mathrm{T} \in \mathrm{Arithm}
    \quad
    v_1 \text{ is a value}
    \quad
    v_2 \text{ is a value}
    \quad
    v_2 < v_1
}{
    v_1 <=> v_2 \rightarrow \text{-1i32}
}
(\text{E-CmpGt})
$$

$$
\frac{
    \text{\_\_typeof}(v_1) = \mathrm{T}
    \quad
    \text{\_\_typeof}(v_2) = \mathrm{T}
    \quad
    \mathrm{T} \in \mathrm{Arithm}
    \quad
    v_1 \text{ is a value}
    \quad
    v_2 \text{ is a value}
    \quad
    v_1 = v_2
}{
    v_1 <=> v_2 \rightarrow \text{0i32}
}
(\text{E-CmpGt})
$$

## Assignment Expression

$$
\frac{
    \Gamma(x) = \mathrm{T}
    \quad
    \Gamma\vdash e: \mathrm{T}
}{
    \Gamma\vdash x = e: \mathrm{void}
}
(\text{T-Assign})
$$

$$
\frac{
    \Gamma(x) = \mathrm{T_1}
    \quad
    \Gamma\vdash e: \mathrm{T_2}
}{
    \Gamma\vdash x = e
    \triangleq
    x = (e\ \text{as}\ \mathrm{T_1})
}
$$

$$
\frac{
    m = \text{Normal}
    \quad
    \gamma(x) = l
    \quad
    v\ \text{is a value}
}{
    x = v; \overline{s} \mid \gamma \mid \sigma \mid \mu \mid s \mid m \mid U
    \rightarrow
    \overline{s} \mid \gamma \mid \sigma[l \mapsto v] \mid \mu \mid s \mid m \mid U
}
(\text{E-Assign})
$$

## `is` Expression

The `is` expression checks if an object has type which is subtype of a specific type.

$$
\frac{
    \Gamma\vdash\mathrm{T_1}\ \mathrm{type}
    \quad
    \Gamma\vdash e: \mathrm{T_2}
    \quad
    \mathrm{T_1} <: \mathrm{T_2}
}{
    \Gamma\vdash e\ \text{is}\ \mathrm{T_1}: \mathrm{bool}
}
(\text{T-IsExpr})
$$

$$
\frac{
    \text{\_\_typeof}(v) = \mathrm{S}
    \quad
    \mathrm{S} <: \mathrm{T}
}{
    v\ \text{is}\ \mathrm{T} \rightarrow \text{true}
}
(\text{E-IsExprTrue})
$$

$$
\frac{
    \text{\_\_typeof}(v) = \mathrm{S}
    \quad
    \mathrm{S} \not<: \mathrm{T}
}{
    v\ \text{is}\ \mathrm{T} \rightarrow \text{false}
}
(\text{E-IsExprFalse})
$$

## Comma Expression

Comma expression evaluates both side of the comma operator but discards the result of left operand and the result of the expression is the right operand:

$$
\frac{
    e_1 \mid \gamma \mid \sigma_0 \mid \mu_0 \mid U_0
    \rightarrow
    v_1 \mid \gamma \mid \sigma_1 \mid \mu_1 \mid U_1
    \quad
    e_2 \mid \gamma \mid \sigma_1 \mid \mu_1 \mid U_1
    \rightarrow
    v_2 \mid \gamma \mid \sigma_2 \mid \mu_2 \mid U_2
}{
    (e_1, e_2) \mid \gamma \mid \sigma_0 \mid \mu_0 \mid U_0
    \rightarrow
    v_2 \mid \gamma \mid \sigma_2 \mid \mu_2 \mid U_2
}
(\text{E-CommaExpr})
$$

The result type is type of the right operand.

$$
\frac{
    \Gamma\vdash x: \mathrm{T_1} \quad \Gamma\vdash y: \mathrm{T_2}
}{
    \Gamma\vdash x,y: \mathrm{T_2}
}
(\text{T-CommaExpr})
$$