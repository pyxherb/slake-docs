# Binary Expression

## Operations

$\text{BinaryExprMainOpType}(A, B)$ infers the main operation type of a common binary expression between types of two undecorated operands.

If both operand types are $\mathrm{UnsignedInteger}$, $\mathrm{SignedInteger}$ or $\mathrm{Float}$ type, choose the wider one:

$$
\frac{
    \mathrm{A} \in \mathrm{UnsignedInteger}
    \quad
    \mathrm{B} \in \mathrm{UnsignedInteger}
}{
    \text{BinaryExprMainOpType}(A, B) = \text{CommonType}(A, B)
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

Or if type of one side is a custom type, use that side:

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

## Assignment Expression

$$
\frac{
    \Gamma(x) = \mathrm{T}
    \quad
    \Gamma\vdash e: \mathrm{T}
}{
    \Gamma\vdash (x = e)\ \triangleright\ \Gamma
}
$$

## Comma Expression

Comma expression evaluates both side of the comma operator but discards the result of left operand and the result of the expression is the right operand:

$$
\frac{
    e_1 \mid \mu_0 \rightarrow v_1 \mid \mu_1
    \quad
    e_2 \mid \mu_1 \rightarrow v_2 \mid \mu_2
}{
(e_1, e_2) \mid \mu_0 \rightarrow v_2 \mid \mu_2
}
(\text{E-CommaExpr})
$$

The result type is type of the right operand.

$$
\frac{
    \Gamma\vdash x: \mathrm{T_1} \quad \Gamma\vdash y: \mathrm{T_2}
}{
    \Gamma\vdash (x,y): \mathrm{T_2}
}
(\text{T-CommaExpr})
$$