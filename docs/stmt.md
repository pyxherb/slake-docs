# Statement

A single statement performs a (maybe nested) instruction included inside.

## Expression Statement

Expression statement evaluates the inner expression and then discards the result immediately after the expression is evaluated.

$$
\frac{
    e_1 \mid \gamma \mid \sigma_0 \mid \mu_0 \mid U_0 \rightarrow v_1 \mid \gamma \mid \sigma_1 \mid \mu_1 \mid U_1
}{
    e_1;\ \overline{s} \mid \gamma \mid \sigma_0 \mid \mu_0 \rightarrow \overline{s} \mid \gamma \mid \sigma_1 \mid \mu_1
}
(\text{E-ExprStmt})
$$

$$
\frac{
    \Gamma\vdash t : \mathrm{T}
}{
    \Gamma\vdash t; : \mathrm{void}
}
(\text{T-ExprStmt})
$$

## Block Statement

## If Statement

`if` statement executes a branch which is determined by the condition expression.

$$
\frac{
    \Gamma, \Delta \vdash e: \mathrm{bool}
    \quad
    (\Gamma', \Delta', \Theta') = \llbracket e \rrbracket(\Gamma, \Delta, \Theta)
    \quad
    (\Gamma_t, \Delta_t, \Theta_t) = \llbracket b_1 \rrbracket(\Gamma', \Delta', \Theta')
    \quad
    (\Gamma_f, \Delta_f, \Theta_f) = \llbracket b_2 \rrbracket(\Gamma', \Delta', \Theta')
    \quad
    (\Gamma'', \Delta'', \Theta'') = \text{\_\_MergeFlowContexts}((\Gamma_t, \Delta_t, \Theta_t), (\Gamma_f, \Delta_f, \Theta_f))
}{
    \Gamma, \Delta \vdash \text{if}(e_1)\ b_1\ \text{else}\ b_2: \text{void}
}
(\text{T-IfStmt})
$$

$$
\frac{
    e_1 \mid \gamma \mid \sigma_0 \mid \mu_0 \mid U_0
    \rightarrow
    \text{true} \mid \gamma \mid \sigma_1 \mid \mu_1 \mid U_1
}{
    \text{if}(e_1)\ b_1\ \text{else}\ b_2\ \overline{s} \mid \gamma \mid \sigma_0 \mid \mu_0 \mid U_0
    \rightarrow
    \{b_1\} \overline{s} \mid \gamma \mid \sigma_1 \mid \mu_1 \mid U_1
}
(\text{E-IfStmtTrue})
$$

$$
\frac{
    e_1 \mid \gamma \mid \sigma_0 \mid \mu_0 \mid U_0
    \rightarrow
    \text{false} \mid \gamma \mid \sigma_1 \mid \mu_1 \mid U_1
}{
    \text{if}(e_1)\ b_1\ \text{else}\ b_2\ \overline{s} \mid \gamma \mid \sigma_0 \mid \mu_0 \mid U_0
    \rightarrow
    \{b_2\} \overline{s} \mid \gamma \mid \sigma_1 \mid \mu_1 \mid U_1
}
(\text{E-IfStmtFalse})
$$

$$
\frac{
    e_1 \mid \gamma \mid \sigma_0 \mid \mu_0 \mid U_0
    \rightarrow
    \text{true} \mid \gamma \mid \sigma_1 \mid \mu_1 \mid U_1
}{
    \text{if}(e_1)\ b_1\ \overline{s} \mid \gamma \mid \sigma_0 \mid \mu_0 \mid U_0
    \rightarrow
    \{b_1\} \overline{s} \mid \gamma \mid \sigma_1 \mid \mu_1 \mid U_1
}
(\text{E-IfStmtWithoutElseTrue})
$$

$$
\frac{
    e_1 \mid \gamma \mid \sigma_0 \mid \mu_0 \mid U_0
    \rightarrow
    \text{false} \mid \gamma \mid \sigma_1 \mid \mu_1 \mid U_1
}{
    \text{if}(e_1)\ b_1\ \overline{s} \mid \gamma \mid \sigma_0 \mid \mu_0 \mid U_0
    \rightarrow
    \overline{s} \mid \gamma \mid \sigma_1 \mid \mu_1 \mid U_1
}
(\text{E-IfStmtWithoutElseFalse})
$$

## While Statement

`while` statement is the simplest form of loop.

$$
\frac{
    e_1 \mid \gamma \mid \sigma_0 \mid \mu_0 \mid U_0
    \rightarrow
    \text{true} \mid \gamma \mid \sigma_1 \mid \mu_1 \mid U_1
}{
    \text{while}(e_1)\ b\ \overline{s} \mid \gamma \mid \sigma_0 \mid \mu_0 \mid U_0
    \rightarrow
    \{b\} \text{while}(e_1)\ b\ \overline{s} \mid \gamma \mid \sigma_1 \mid \mu_1 \mid U_1
}
(\text{E-WhileStmtTrue})
$$

$$
\frac{
    e_1 \mid \gamma \mid \sigma_0 \mid \mu_0 \mid U_0
    \rightarrow
    \text{false} \mid \gamma \mid \sigma_1 \mid \mu_1 \mid U_1
}{
    \text{while}(e_1)\ b\ \overline{s} \mid \gamma \mid \sigma_0 \mid \mu_0 \mid U_0
    \rightarrow \overline{s} \mid \gamma \mid \sigma_1 \mid \mu_1 \mid U_1
}
(\text{E-WhileStmtFalse})
$$

## Do-while Statement

Just like `while`, but `do`-`while` executes the body first.

$$
\frac{
}{
    \text{do}\ b\ \text{while}(e_1);\ \overline{s} \mid \gamma \mid \sigma\mid \mu \mid U
    \rightarrow
    \{b\} \text{while}(e_1)\ b\ \overline{s} \mid \gamma \mid \sigma \mid \mu \mid U
}
(\text{E-DoWhileStmt})
$$

## Let Statement

Let statement defines one or more local variables.

$$
\frac{
    \Gamma\vdash\mathrm{T}\ \mathrm{type}
    \quad
    x \notin \text{dom}(\Gamma)
    \quad
    \Gamma, x: \mathrm{T}\vdash \overline{s}: \mathrm{void}
}{
    \Gamma\vdash \text{let}\ x: \mathrm{T}; \overline{s}: \mathrm{void}
}
(\text{T-LetTyped})
$$

$$
\frac{
    \Gamma\vdash t: \mathrm{T_1}
    \quad
    x \notin \text{dom}(\Gamma)
    \quad
    \Gamma, x: \mathrm{T_2}\vdash \overline{s}: \mathrm{void}
    \quad
    \mathrm{T_1} <: \mathrm{T_2}
}{
    \Gamma\vdash \text{let}\ x: \mathrm{T_2} = t; \overline{s}: \mathrm{void}
}
(\text{T-LetTypedInit})
$$

$$
\frac{
    \Gamma\vdash e: \mathrm{T}
}{
    \Gamma\vdash\text{let}\ x = e
    \triangleq
    \text{let}\ x: \mathrm{T} = e
}
$$

$$
\frac{
    m = \text{Normal}
    \quad
    \text{\_\_alloca}(\mathrm{T}, \sigma) \mid \gamma \mid \sigma \mid \mu \mid U
    =
    l \mid \gamma \mid \sigma' \mid \mu \mid U
    \quad
    v\ \text{is a value}
}{
    \text{let}\ x: \mathrm{T} = v; \overline{s} \mid \gamma \mid \sigma \mid \mu \mid s \mid m \mid U
    \rightarrow
    \overline{s} \mid \gamma[x \mapsto l] \mid \sigma'[l \mapsto v] \mid \mu \mid s \mid m \mid U
}
(\text{E-LetTypedInit-Normal})
$$

$$
\frac{
    m = \text{Normal}
    \quad
    e \mid \gamma \mid \sigma \mid \mu \mid s \mid m \mid U
    \rightarrow
    e' \mid \gamma \mid \sigma' \mid \mu' \mid s \mid m \mid U'
    \quad
    e\ \text{is not a value}
}{
    \text{let}\ x: \mathrm{T} = e; \overline{s} \mid \gamma \mid \sigma \mid \mu \mid s \mid m \mid U
    \rightarrow
    \text{let}\ x: \mathrm{T} = e'; \overline{s} \mid \gamma \mid \sigma' \mid \mu' \mid s \mid m \mid U'
}
(\text{E-LetTypedInitReduce-Normal})
$$

$$
\frac{
    m = \text{Normal}
}{
    \text{let}\ x: \mathrm{T}; \overline{s}
    \rightarrow
    \text{let}\ x: \mathrm{T} = \text{\_\_defaultof}(\mathrm{T}); \overline{s}
}
(\text{E-LetTyped-Normal})
$$
