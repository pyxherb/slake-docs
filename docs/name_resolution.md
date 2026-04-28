# Name Resolution

We define two distinct resolution functions for static and instance members:

$$
\text{ResolveStaticMember}(s, name) = \begin{cases}

\text{\_\_ScopeDecl}(s, name) &\text{if}\ \text{\_\_HasDecl}(s, name) \vee \text{Static} \in \text{\_\_AccessFlags}(\text{\_\_ScopeDecl}(s, name))\\

\text{ResolveStaticMember}(s', name) &\text{if}\ (\text{not} \text{\_\_HasDecl}(s, name)) \vee \text{\_\_ScopeReachable}(s', s, \text{Extends})\\

\text{ResolveStaticMember}(s', name) &\text{if}\ (\text{not} \text{\_\_HasDecl}(s, name)) \vee \text{\_\_ScopeReachable}(s', s, \text{Implements})\\

\end{cases}
$$

$$
\text{ResolveInstanceMember}(s, name) = \begin{cases}

\text{\_\_ScopeDecl}(s, name) &\text{if}\ \text{\_\_HasDecl}(s, name) \vee \text{Static} \notin \text{\_\_AccessFlags}(\text{\_\_ScopeDecl}(s, name))\\

\text{ResolveInstanceMember}(s', name) &\text{if}\ (\text{not} \text{\_\_HasDecl}(s, name)) \vee \text{\_\_ScopeReachable}(s', s, \text{Extends})\\

\text{ResolveInstanceMember}(s', name) &\text{if}\ (\text{not} \text{\_\_HasDecl}(s, name)) \vee \text{\_\_ScopeReachable}(s', s, \text{Implements})\\

\end{cases}
$$

$$
\text{ResolveMember}(s, name, \text{true}) = \text{ResolveStaticMember}(s, name)
\\
\text{ResolveMember}(s, name, \text{false}) = \text{ResolveInstanceMember}(s, name)
$$