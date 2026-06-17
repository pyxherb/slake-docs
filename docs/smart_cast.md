# Smart Casting

$\Gamma$ is the variable binding context, $\Delta$ is the flow-sensitive variable type context, $\Theta$ is the nullity context.

For this, we will define an analysis function $\llbracket \cdot \rrbracket$ for flow-based analysis.

<!--Note: The analysis function should be applied in the precondition-->

## Auxiliary Definitions

$$
\text{UpdateNullityRecordParallel}(\tau, \overline{\Theta}) = \begin{cases}

\text{Nullified}
&
\forall i, \tau \in \text{dom}(\overline{\Theta}_i) \wedge \overline{\Theta}_i(\tau) = \text{Nullified}
\\
\text{Denullified}
&
\forall i, \tau \in \text{dom}(\overline{\Theta}_i) \wedge \overline{\Theta}_i(\tau) = \text{Denullified}
\\
\text{Uncertain}
&
\text{otherwise}

\end{cases}
$$

$$
\begin{cases}
\text{UpdateNullityRecordsParallel}(\Gamma_{outer}, \Theta_{outer}, \overline{\Theta}) =
\forall i \in (
    \text{dom}(\overline{\Theta}_1) \cup
    \text{dom}(\overline{\Theta}_2) \cup
    \dots),
\Theta_{outer}[i \mapsto \text{UpdateNullityRecordParallel}(i, \overline{\Theta})]
&\text{if}\ i \in \text{dom}(\Gamma_{outer})
\end{cases}
$$

## Assignments

<!--Incompleted yet.-->

$$
\llbracket x = e \rrbracket(\Gamma, \Delta, \Theta) = \begin{cases}

(\Gamma, \Delta[x \mapsto \mathrm{T?}], \Theta[x \mapsto \mathrm{Nullified}])
&
\Gamma(x)=\mathrm{T?}
\wedge
\Gamma,\Delta,\Theta\vdash e: \mathrm{null}\\

(\Gamma, \Delta[x \mapsto \mathrm{T?}], \Theta[x \mapsto \mathrm{Uncertain}])
&
\Gamma(x)=\mathrm{T?}
\wedge
\Gamma,\Delta,\Theta\vdash e: \mathrm{T?}\\

(\Gamma, \Delta[x \mapsto \mathrm{T}], \Theta[x \mapsto \mathrm{Denullified}])
&
\Gamma(x)=\mathrm{T?}
\wedge
\Gamma,\Delta,\Theta\vdash e: \mathrm{T}\\

\end{cases}
$$

$$
\llbracket x === y \rrbracket(\Gamma, \Delta, \Theta) = \begin{cases}

(\Gamma, \Delta[x \mapsto \mathrm{T?}], \Theta[x \mapsto \mathrm{Nullified}])
&
\Gamma(x)=\mathrm{T?}
\wedge
\Gamma,\Delta,\Theta\vdash y: \mathrm{null}\\

(\Gamma, \Delta[x \mapsto \mathrm{T?}], \Theta[x \mapsto \mathrm{Uncertain}])
&
\Gamma(x)=\mathrm{T?}
\wedge
\Gamma,\Delta,\Theta\vdash y: \mathrm{T?}\\

(\Gamma, \Delta[x \mapsto \mathrm{T}], \Theta[x \mapsto \mathrm{Denullified}])
&
\Gamma(x)=\mathrm{T?}
\wedge
\Gamma,\Delta,\Theta\vdash y: \mathrm{T}\\

(\Gamma, \Delta[y \mapsto \mathrm{T?}], \Theta[y \mapsto \mathrm{Nullified}])
&
\Gamma(y)=\mathrm{T?}
\wedge
\Gamma,\Delta,\Theta\vdash x: \mathrm{null}\\

(\Gamma, \Delta[y \mapsto \mathrm{T?}], \Theta[y \mapsto \mathrm{Uncertain}])
&
\Gamma(y)=\mathrm{T?}
\wedge
\Gamma,\Delta,\Theta\vdash x: \mathrm{T?}\\

(\Gamma, \Delta[y \mapsto \mathrm{T}], \Theta[y \mapsto \mathrm{Denullified}])
&
\Gamma(y)=\mathrm{T?}
\wedge
\Gamma,\Delta,\Theta\vdash x: \mathrm{T}\\

(\Gamma, \Delta, \Theta)
&
\text{otherwise}

\end{cases}
$$

$$
\llbracket x !== y \rrbracket(\Gamma, \Delta, \Theta) = \begin{cases}

(\Gamma, \Delta[x \mapsto \mathrm{T}], \Theta[x \mapsto \mathrm{Denullified}])
&
\Gamma(x)=\mathrm{T?}
\wedge
(\Gamma,\Delta,\Theta\vdash y: \mathrm{null})\\

(\Gamma, \Delta[y \mapsto \mathrm{T}], \Theta[y \mapsto \mathrm{Denullified}])
&
\Gamma(y)=\mathrm{T?}
\wedge
(\Gamma,\Delta,\Theta\vdash x: \mathrm{null})\\

(\Gamma, \Delta, \Theta)
&
\text{otherwise}

\end{cases}
$$

## Other Expressions and Statements

$$
\llbracket e \rrbracket(\Gamma, \Delta, \Theta) =
(\Gamma, \Delta, \Theta)
$$

For other expressions and statements, the analysis function does not update anything.
