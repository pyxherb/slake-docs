# References

Performing reference operation creates a reference to the original variable, with corresponding reference type:

$$
\frac{
    \Gamma\vdash x:\mathrm{T}
}{
    \Gamma\vdash \And x:\mathrm{T\And}
}
$$

Performing dereference operation extracts the original value referred by the reference：

$$
\frac{
    \Gamma\vdash x:\mathrm{T\And}
}{
    \Gamma\vdash \text{*}x:\mathrm{T}
}
$$

Assignment to a reference is allowed by a non-reference value:

$$
\frac{
    \Gamma\vdash x:\mathrm{T\And}
    \quad
    \Gamma\vdash y:\mathrm{T}
}{
    \Gamma\vdash x\text{=}y:\mathrm{T\And}
}
$$