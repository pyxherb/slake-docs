# Binary Expression

## Operations

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
}{
    \text{BinaryExprMainOpType}(\mathrm{A}, \mathrm{B}) = \mathrm{A}
}
$$

$$
\frac{
    \mathrm{B} <: \mathrm{class}
    \vee
    \mathrm{B} <: \mathrm{struct}
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

## Non-overloaded Binary Expressions

The non-overloaded binary expression requires a main operand type to determine which kind of operations can be performed by the type.

First, the compiler must look up the operation type:

* For `<<`, `>>` and all assignment and compound assignment operations, the main operand type is the variable-reference-removed type of LHS:
  * For `=`, `+=`, `-=`, `*=`, `/=`, `%=`, `&=`, `|=`, `^=`, they require the LHS to be an l-value, RHS to have the same type as the main operand type or any convertible type of the main operand type, the result type is l-valued type of LHS.
  * For `<<`, `>>`, they require the LHS to be integral type and the RHS to be `u32` typed or any r-value with type that can be implicitly converted to `u32`, the result type is variable-reference-removed type of LHS.
  * For `<<=`, `>>=`, they require the LHS to be l-valued integral type and the RHS to be `u32` typed or any r-value with type that can be implicitly converted to `u32`, the result type is l-valued type of LHS.
* For `[]`, the main operand type is the variable-reference-removed type of LHS, it requires the LHS to be array typed or `string` typed and the RHS must be `usize` typed or any r-value with type that can be implicitly converted to `usize`, the result type is l-valued element type of the LHS for array typed LHS or `u8` for `string` typed LHS.
* For `&&`, the LHS and the RHS must be `bool` typed or implicitly convertible to `bool` type, the RHS should not be evaluated at the runtime if the LHS is evaluated to be `false`, the result type is `bool`.
* For `||`, the LHS and the RHS must be `bool` typed or implicitly convertible to `bool` type, the RHS should not be evaluated at the runtime if the LHS is evaluated to be `true`, the result type is `bool`.
* For any other kind of non-overloaded binary expression, if both sides of the operands can be converted to other side's variable-reference-removed type, the main operand type is the wider variable-reference-removed type of the same kind if viable, or if only one side can be converted to other side's variable-reference-removed type, the main operand type is variable-reference-removed type of the side which has type the other operand can be converted to. If there is no side's conversion is viable, use LHS's variable-reference-removed type to generate the error message (the error message should be emitted during the operand conversion).
  * Once the main operand type is determined, the compiler should lookup if the operation is viable by the main operand type, and generate corresponding codes if viable or generate an error message otherwise. Both sides of the expression should be converted to the main operand type. The result type is the main operand type.
  * For `<`, `>`, `<=`, `>=`, `==`, `!=`, `===`, `!==`, the result type is `bool`.
  * For `<=>`, the result type is `i32`.

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
$$

$$
\frac{
    \Gamma\vdash e_1: \mathrm{T_1}
    \quad
    \Gamma\vdash e_2: \mathrm{T_2}
    \quad
    \text{BinaryExprMainOpType}(\mathrm{T_1}, \mathrm{T_2}) = \mathrm{T}
}{
    \Gamma\vdash e_1 + e_2
    \triangleq
    (e_1\ \text{as}\ \mathrm{T}) + (e_2\ \text{as}\ \mathrm{T})
}
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
$$

$$
\frac{
    \Gamma\vdash e_1: \mathrm{T_1}
    \quad
    \Gamma\vdash e_2: \mathrm{T_2}
    \quad
    \text{BinaryExprMainOpType}(\mathrm{T_1}, \mathrm{T_2}) = \mathrm{T}
}{
    \Gamma\vdash e_1 + e_2
    \triangleq
    (e_1\ \text{as}\ \mathrm{T}) - (e_2\ \text{as}\ \mathrm{T})
}
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
$$

$$
\frac{
    \Gamma\vdash e_1: \mathrm{T_1}
    \quad
    \Gamma\vdash e_2: \mathrm{T_2}
    \quad
    \text{BinaryExprMainOpType}(\mathrm{T_1}, \mathrm{T_2}) = \mathrm{T}
}{
    \Gamma\vdash e_1 * e_2
    \triangleq
    (e_1\ \text{as}\ \mathrm{T}) * (e_2\ \text{as}\ \mathrm{T})
}
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
$$

$$
\frac{
    \Gamma\vdash e_1: \mathrm{T_1}
    \quad
    \Gamma\vdash e_2: \mathrm{T_2}
    \quad
    \text{BinaryExprMainOpType}(\mathrm{T_1}, \mathrm{T_2}) = \mathrm{T}
}{
    \Gamma\vdash e_1 / e_2
    \triangleq
    (e_1\ \text{as}\ \mathrm{T}) / (e_2\ \text{as}\ \mathrm{T})
}
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
$$

$$
\frac{
    \Gamma\vdash e_1: \mathrm{T_1}
    \quad
    \Gamma\vdash e_2: \mathrm{T_2}
    \quad
    \text{BinaryExprMainOpType}(\mathrm{T_1}, \mathrm{T_2}) = \mathrm{T}
}{
    \Gamma\vdash e_1 \% e_2
    \triangleq
    (e_1\ \text{as}\ \mathrm{T}) \% (e_2\ \text{as}\ \mathrm{T})
}
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
$$

$$
\frac{
    \Gamma\vdash e_1: \mathrm{T_1}
    \quad
    \Gamma\vdash e_2: \mathrm{T_2}
    \quad
    \text{BinaryExprMainOpType}(\mathrm{T_1}, \mathrm{T_2}) = \mathrm{T}
}{
    \Gamma\vdash e_1 \And e_2
    \triangleq
    (e_1\ \text{as}\ \mathrm{T}) \And (e_2\ \text{as}\ \mathrm{T})
}
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
$$

$$
\frac{
    \Gamma\vdash e_1: \mathrm{T_1}
    \quad
    \Gamma\vdash e_2: \mathrm{T_2}
    \quad
    \text{BinaryExprMainOpType}(\mathrm{T_1}, \mathrm{T_2}) = \mathrm{T}
}{
    \Gamma\vdash e_1 | e_2
    \triangleq
    (e_1\ \text{as}\ \mathrm{T}) | (e_2\ \text{as}\ \mathrm{T})
}
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
$$

$$
\frac{
    \Gamma\vdash e_1: \mathrm{T_1}
    \quad
    \Gamma\vdash e_2: \mathrm{T_2}
    \quad
    \text{BinaryExprMainOpType}(\mathrm{T_1}, \mathrm{T_2}) = \mathrm{T}
}{
    \Gamma\vdash e_1 \wedge e_2
    \triangleq
    (e_1\ \text{as}\ \mathrm{T}) \wedge (e_2\ \text{as}\ \mathrm{T})
}
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
$$

$$
\frac{
    \Gamma\vdash e_1: \mathrm{T_1}
    \quad
    \Gamma\vdash e_2: \mathrm{T_2}
}{
    \Gamma\vdash e_1 << e_2
    \triangleq
    e_1 << (e_2\ \text{as}\ \mathrm{u32})
}
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
$$

$$
\frac{
    \Gamma\vdash e_1: \mathrm{T_1}
    \quad
    \Gamma\vdash e_2: \mathrm{T_2}
}{
    \Gamma\vdash e_1 >> e_2
    \triangleq
    e_1 >> (e_2\ \text{as}\ \mathrm{u32})
}
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
$$

## `is` Expression

The `is` expression checks if an object has type which is subtype of a specific type.

$$
\frac{
    \Gamma\vdash\mathrm{T_1}\ \mathrm{type}
    \quad
    \Gamma\vdash e: \mathrm{T_2}
    \quad
    \mathrm{T_2} <: \mathrm{T_1}
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