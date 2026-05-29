# Garbage Collection

We define following configuration:

$$
e \mid \mu \mid G
$$

Where $e$ is current expression, $\mu$ is global environment (included the heap), $G$ is current GC status.

The GC status can be:

* $\text{Idle}$, the GC is not running.
* $\text{Marking}(W)$, the garbage collector is scanning the garbages, with working root set $W$.
* $\text{Sweeping}(L)$, the garbage collector is cleaning the garbages, with current list $L$.

The GC process can be started at any time.

$$
\frac{
    W = \text{\_\_roots}(\mu)
}{
    e \mid \mu \mid \text{Idle}
    \rightarrow
    e \mid \mu \mid \text{Marking}(W)
}
$$

First, the garbage collector marks living objects:

$$
\frac{
    l \in W
    \quad
    \text{\_\_mark}(l) = \text{false}
}{
    e \mid \mu \mid \text{Marking}(W)
    \rightarrow
    e \mid \mu[l.\text{marked} := \text{true}] \mid \text{Marking}((W \backslash \{l\}) \cup \text{\_\_children}(l))
}
$$

$$
\frac{
    l \in W
    \quad
    \text{\_\_mark}(l) = \text{true}
}{
    e \mid \mu \mid \text{Marking}(W)
    \rightarrow
    e \mid \mu \mid \text{Marking}(W \backslash \{l\})
}
$$

When a write operation occurs during the marking stage, the garbage collector should walk the value if the value is an object:

$$
\frac{
    \text{\_\_write}(x, v); \overline{s} \mid \gamma \mid \sigma \mid \mu \mid s \mid m \mid U \mid \text{Marking}(W)
    \rightarrow
    \overline{s} \mid \gamma' \mid \sigma' \mid \mu' \mid s' \mid m' \mid U'  \mid \text{Marking}(W)
    \quad
    \text{\_\_typeof}(v) <: \mathrm{object}
}{
    \text{\_\_write}(x, v); \overline{s} \mid \gamma \mid \sigma \mid \mu \mid s \mid m \mid U  \mid \text{Marking}(W)
    \rightarrow
    \overline{s} \mid \gamma' \mid \sigma' \mid \mu' \mid s' \mid m' \mid U' \mid \text{Marking}(W \cup \{v\})
}
$$

When there is no more objects can be walked, the garbage collector should turn into the $\text{Sweeping}$ stage:

$$
\frac{
    W = \empty
}{
    \overline{s} \mid \gamma \mid \sigma \mid \mu \mid s \mid m \mid \text{Marking}(W)
    \rightarrow
    \overline{s} \mid \gamma \mid \sigma \mid \mu \mid s \mid m \mid \text{Sweeping}(\text{\_\_sort\_by\_dtor\_prio}(\text{\_\_unreachable}(\mu)))
}
$$

The garbage collector should take out the first element $L_0$ of the sweeping set $L$ and finalize it and repeat it:

$$
\frac{
    L \ne \empty
}{
    \overline{s} \mid \gamma \mid \sigma \mid \mu \mid s \mid m \mid \text{Sweeping}(L)
    \rightarrow
    \overline{s} \mid \gamma \mid \sigma \mid \mu' \mid s \mid m \mid \text{Sweeping}(L \backslash L_0)
}
$$

Where $\mu'$ is $\text{\_\_finalize}(L_0, \mu')$.

When the sweeping list $L$ is empty, reset the GC state to $\text{Idle}$ and clear the $\text{marked}$ flags of all objects:

$$
\frac{
    L = \empty
}{
    \overline{s} \mid \gamma \mid \sigma \mid \mu \mid s \mid m \mid \text{Sweeping}(L)
    \rightarrow
    \overline{s} \mid \gamma \mid \sigma \mid \mu' \mid s \mid m \mid \text{Idle}
}
$$

Where $\mu'$ is $\text{\_\_clear\_marks}(\mu)$.
