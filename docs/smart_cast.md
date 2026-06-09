# Smart Casting

$\Gamma$ is the variable binding context, $\Delta$ is the flow-sensitive variable type context, $\Theta$ is the nullity context.

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