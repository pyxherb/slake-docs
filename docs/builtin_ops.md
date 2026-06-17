# Built-in Operations

## Static Operations

$\text{ScopeOf}(t)$ - Get scope of a type or module.

$\text{HasDecl}(s, name)$ - Check if there is a declaration named $name$ in scope $s$.

$\text{ScopeDecl}(s, name)$ - Get a declaration named $name$ in scope $s$.

$\text{AccessMode}(t)$ - Get access mode of a type or a module, where the value can be $\text{Public}$ or $\text{Private}$ or $\text{Protected}$.

$\text{AccessFlags}(t)$ - Get access flags set of a type or a module, where the value can have $\{\text{Static}, \text{Native}\}$.

$\text{ScopeReachable}(s, t, R)$ - Check if $s$ is reachable from $t$ via scope relationship $R$, where $R \in \{\text{Import}, \text{Extends}, \text{Implements}, \text{Lexical}\}$.

## Dynamic Operations

$\text{typeof}(v)$ - Evaluates type of the value $v$ in the runtime.

$\text{int\_width}(t)$ - Evaluates width of the type $t$ where $t$ must be a type in $\mathrm{Integer}$.

$\text{float\_width}(t)$ - Evaluates width of the type $t$ where $t$ must be a type in $\mathrm{Float}$.

$\text{narrow\_int}(v, n)$ - Narrow $v$ into an $n$ bits integer, where `v` must be $\mathrm{Integer}$ typed.

$\text{widen\_uint}(v, n)$ - Widen $v$ into an $n$ bits unsigned integer, where `v` must be $\mathrm{UnsignedInteger}$ typed.

$\text{widen\_int}(v, n)$ - Widen $v$ into an $n$ bits signed integer, where `v` must be $\mathrm{SignedInteger}$ typed.

$\text{to\_signed}(v)$ - Convert $v$ into a same-width signed integer, where `v` must be $\mathrm{UnsignedInteger}$ typed.

$\text{to\_unsigned}(v)$ - Convert $v$ into a same-width signed integer, where `v` must be $\mathrm{SignedInteger}$ typed.

$\text{to\_f32}(v)$ - Convert $v$ into a 32-bit IEEE754 floating-point number, where `v` must be $\mathrm{Arithm}$ typed.

$\text{to\_f64}(v)$ - Convert $v$ into a 64-bit IEEE754 floating-point number, where `v` must be $\mathrm{Arithm}$ typed.

$\text{float\_to\_i8}(v)$ - Converts $v$ into an `i8` value, where `v` must be a $\mathrm{Float}$ typed value.

$\text{float\_to\_i16}(v)$ - Converts $v$ into an `i16` value, where `v` must be a $\mathrm{Float}$ typed value.

$\text{float\_to\_i32}(v)$ - Converts $v$ into an `i32` value, where `v` must be a $\mathrm{Float}$ typed value.

$\text{float\_to\_i64}(v)$ - Converts $v$ into an `i64` value, where `v` must be a $\mathrm{Float}$ typed value.

$\text{float\_to\_u8}(v)$ - Converts $v$ into an `u8` value, where `v` must be a $\mathrm{Float}$ typed value.

$\text{float\_to\_u16}(v)$ - Converts $v$ into an `u16` value, where `v` must be a $\mathrm{Float}$ typed value.

$\text{float\_to\_u32}(v)$ - Converts $v$ into an `u32` value, where `v` must be a $\mathrm{Float}$ typed value.

$\text{float\_to\_u64}(v)$ - Converts $v$ into an `u64` value, where `v` must be a $\mathrm{Float}$ typed value.

$\text{int\_to\_width}(v, n)$ - Converts $v$ into an signed integer value with specific width, where `v` must be an $\mathrm{Integer}$ typed value:

$\text{stack\_depth}(\mu)$ - Evaluate current stack depth in the runtime.

$\text{stack\_max}(\mu)$ - Evaluate maximum stack depth in the runtime.

$\text{stack\_align\_diff}(t, \mu)$ - Evaluate difference between current stack top address of $\mu$ and next type $t$ aligned address.

$\text{sizeof}(t)$ - Evaluate size of an instance of a type.

$\text{defaultof}(t)$ - Get default value of a type.

$\text{alloca}(t, \sigma)$ - Allocate an instance of a type on the stack (uninitialized).

$\text{add}(t, x, y)$ - Evaluate $(x + y)$ with operands, valid types are $\mathrm{i8}$, $\mathrm{i16}$, $\mathrm{i32}$, $\mathrm{i64}$, $\mathrm{isize}$, $\mathrm{u8}$, $\mathrm{u16}$, $\mathrm{u32}$, $\mathrm{u64}$, $\mathrm{usize}$, $\mathrm{f32}$, $\mathrm{f64}$, where:

* Both operands must have the same type.
* For $\mathrm{SignedInteger}$ types, overflow's behavior is undefined.
* For $\mathrm{UnsignedInteger}$ types, the result is $(x + y)\ \text{mod}\ 2^{\text{int\_width}(t)}$.
* For $\mathrm{Float}$ types, see the IEEE-754 standard.

$\text{sub}(t, x, y)$ - Evaluate $(x - y)$ with operands, valid types are $\mathrm{i8}$, $\mathrm{i16}$, $\mathrm{i32}$, $\mathrm{i64}$, $\mathrm{isize}$, $\mathrm{u8}$, $\mathrm{u16}$, $\mathrm{u32}$, $\mathrm{u64}$, $\mathrm{usize}$, $\mathrm{f32}$, $\mathrm{f64}$, where:

* Both operands must have the same type.
* For $\mathrm{SignedInteger}$ types, underflow's behavior is undefined.
* For $\mathrm{UnsignedInteger}$ types, the result is $(y - x)\ \text{mod}\ 2^{\text{int\_width}(t)}$.
* For $\mathrm{Float}$ types, see the IEEE-754 standard.

$\text{mul}(t, x, y)$ - Evaluate $(x \times y)$ with operands, valid types are $\mathrm{i8}$, $\mathrm{i16}$, $\mathrm{i32}$, $\mathrm{i64}$, $\mathrm{isize}$, $\mathrm{u8}$, $\mathrm{u16}$, $\mathrm{u32}$, $\mathrm{u64}$, $\mathrm{usize}$, $\mathrm{f32}$, $\mathrm{f64}$, where:

* Both operands must have the same type.
* For $\mathrm{SignedInteger}$ types, overflow's behavior is undefined.
* For $\mathrm{UnsignedInteger}$ types, the result is $(x \times y)\ \text{mod}\ 2^{\text{int\_width}(t)}$.
* For $\mathrm{Float}$ types, see the IEEE-754 standard.

$\text{mul}(t, x, y)$ - Evaluate $(x \div y)$ with operands, valid types are $\mathrm{i8}$, $\mathrm{i16}$, $\mathrm{i32}$, $\mathrm{i64}$, $\mathrm{isize}$, $\mathrm{u8}$, $\mathrm{u16}$, $\mathrm{u32}$, $\mathrm{u64}$, $\mathrm{usize}$, $\mathrm{f32}$, $\mathrm{f64}$, where:

* Both operands must have the same type.
* For $\mathrm{Integer}$ types, the remainder will be discarded and the result will be rounded down to the nearest integer.
  * If the value is $(0\ \text{as}\ t)$, the program should abort.
* For $\mathrm{Float}$ types, see the IEEE-754 standard.

$\text{mod}(t, x, y)$ - Evaluate $(x\ \text{mod}\ y)$ with operands, valid types are $\mathrm{i8}$, $\mathrm{i16}$, $\mathrm{i32}$, $\mathrm{i64}$, $\mathrm{isize}$, $\mathrm{u8}$, $\mathrm{u16}$, $\mathrm{u32}$, $\mathrm{u64}$, $\mathrm{usize}$, $\mathrm{f32}$, $\mathrm{f64}$, where:

* Both operands must have the same type.
* For $\mathrm{Integer}$ types:
  * The program should abort if $y$ is $(0\ \text{as}\ t)$.
  * Otherwise, the result is remainder of $(x \div y)$.

* For $\mathrm{Float}$ types:
  * The result is $\text{NaN}$ if $y$ is $+0$ or $-0$.
  * If one side is $\text{NaN}$, the result is $\text{NaN}$
  * If one side is $\text{Infinity}$ or $-\text{Infinity}$, the result is $\text{NaN}$
  * Otherwise, the result is remainder of $(x \div y)$.

$\text{and}(t, x, y)$ - Evaluate $(x\ \text{AND}\ y)$ with operands, valid types are $\mathrm{i8}$, $\mathrm{i16}$, $\mathrm{i32}$, $\mathrm{i64}$, $\mathrm{isize}$, $\mathrm{u8}$, $\mathrm{u16}$, $\mathrm{u32}$, $\mathrm{u64}$, $\mathrm{usize}$, where:

* Both operands must have the same type.

$\text{or}(t, x, y)$ - Evaluate $(x\ \text{OR}\ y)$ with operands, valid types are $\mathrm{i8}$, $\mathrm{i16}$, $\mathrm{i32}$, $\mathrm{i64}$, $\mathrm{isize}$, $\mathrm{u8}$, $\mathrm{u16}$, $\mathrm{u32}$, $\mathrm{u64}$, $\mathrm{usize}$, where:

* Both operands must have the same type.

$\text{xor}(t, x, y)$ - Evaluate $(x\ \text{XOR}\ y)$ with operands, valid types are $\mathrm{i8}$, $\mathrm{i16}$, $\mathrm{i32}$, $\mathrm{i64}$, $\mathrm{isize}$, $\mathrm{u8}$, $\mathrm{u16}$, $\mathrm{u32}$, $\mathrm{u64}$, $\mathrm{usize}$, where:

* Both operands must have the same type.

$\text{eq}(t, x, y)$ - Evaluate if $x$ equals to $y$, valid types are $\mathrm{i8}$, $\mathrm{i16}$, $\mathrm{i32}$, $\mathrm{i64}$, $\mathrm{isize}$, $\mathrm{u8}$, $\mathrm{u16}$, $\mathrm{u32}$, $\mathrm{u64}$, $\mathrm{usize}$, $\mathrm{bool}$, where:

* Both operands must have the same type.
* For $\mathrm{Float}$ types, see the IEEE-754 standard.

$\text{neq}(t, x, y)$ - Evaluate if $x$ is not equal to $y$, valid types are $\mathrm{i8}$, $\mathrm{i16}$, $\mathrm{i32}$, $\mathrm{i64}$, $\mathrm{isize}$, $\mathrm{u8}$, $\mathrm{u16}$, $\mathrm{u32}$, $\mathrm{u64}$, $\mathrm{usize}$, $\mathrm{bool}$, where:

* Both operands must have the same type.
* For $\mathrm{Float}$ types, see the IEEE-754 standard.

$\text{phy\_eq}(t, x, y)$ - Evaluate if $x$ physically equals to $y$, where $t$ must subtype of $\mathrm{object}$ and type of $x$ and $y$ must be $t$.

$\text{phy\_neq}(t, x, y)$ - Evaluate if $x$ is not physically equal to $y$, where $t$ must subtype of $\mathrm{object}$ and type of $x$ and $y$ must be $t$.

$\text{lt}(t, x, y)$ - Evaluate if $x$ is less than $y$, valid types are $\mathrm{i8}$, $\mathrm{i16}$, $\mathrm{i32}$, $\mathrm{i64}$, $\mathrm{isize}$, $\mathrm{u8}$, $\mathrm{u16}$, $\mathrm{u32}$, $\mathrm{u64}$, $\mathrm{usize}$, where:

* Both operands must have the same type.
* For $\mathrm{Float}$ types, see the IEEE-754 standard.

$\text{gt}(t, x, y)$ - Evaluate if $x$ is greater than $y$, valid types are $\mathrm{i8}$, $\mathrm{i16}$, $\mathrm{i32}$, $\mathrm{i64}$, $\mathrm{isize}$, $\mathrm{u8}$, $\mathrm{u16}$, $\mathrm{u32}$, $\mathrm{u64}$, $\mathrm{usize}$, where:

* Both operands must have the same type.
* For $\mathrm{Float}$ types, see the IEEE-754 standard.

$\text{lteq}(t, x, y)$ - Evaluate if $x$ is less than or equal to $y$, valid types are $\mathrm{i8}$, $\mathrm{i16}$, $\mathrm{i32}$, $\mathrm{i64}$, $\mathrm{isize}$, $\mathrm{u8}$, $\mathrm{u16}$, $\mathrm{u32}$, $\mathrm{u64}$, $\mathrm{usize}$, where:

* Both operands must have the same type.
* For $\mathrm{Float}$ types, see the IEEE-754 standard.
* $\text{bool}$ typed values must be converted to any supported types to compare. 

$\text{gteq}(t, x, y)$ - Evaluate if $x$ is greater than or equal to $y$, valid types are $\mathrm{i8}$, $\mathrm{i16}$, $\mathrm{i32}$, $\mathrm{i64}$, $\mathrm{isize}$, $\mathrm{u8}$, $\mathrm{u16}$, $\mathrm{u32}$, $\mathrm{u64}$, $\mathrm{usize}$, where:

* Both operands must have the same type.
* For $\mathrm{Float}$ types, see the IEEE-754 standard.
* $\text{bool}$ typed values must be converted to any supported types to compare.

$\text{roots}(\mu)$ - Extract root object set from a global state.

$\text{unreachable}(\mu)$ - Get objects that are not marked by the garbage collector.

$\text{mark}(x, \mu)$ - Mark $x$ in $\mu$ as accessible.

$\text{clear\_marks}(\mu)$ - Clear objects marked accessible in $\mu$.

$\text{children}(x)$ - Get objects referenced by $x$.

$\text{finalize}(x, \mu)$ - Delete object $x$ and remove it from global state $\mu$ and execute corresponding native destructors, returns the new global state $\mu$.

$\text{sort\_by\_dtor\_prio}(L)$ - Sort elements in object list $L$ by destructor priority

$\text{restrict\_alive}(\mu, S)$ - Restrict alive object set to $S$ for global state $\mu$.

$\text{allocated\_size}(\mu)$ - Returns memory allocated by the runtime.

$\text{read}(x)$ - Read value from variable $v$.

$\text{write}(x, v)$ - Write $v$ to variable $x$.

$\text{unwind}(t)$ - Unwind current stack with statement $t$ to be executed after unwinding. In specification, if this operation is performed without any stack frame, the behavior is undefined.

$$
\frac{
}
{
    \text{unwind}(t) \mid \gamma \mid (K, \gamma_{prev}, U_{prev}) \cdot \sigma \mid \mu \mid s \mid m
    \rightarrow
    t \mid \gamma_{prev} \mid \sigma \mid \mu \mid s \mid m \mid U_{prev}
}
$$

$\text{uncaught}(e)$ - Report the uncaught exception $e$ to the user or perform corresponding handler which should be triggered when an exception is uncaught through the whole program.
