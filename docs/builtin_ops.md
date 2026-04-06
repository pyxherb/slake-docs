# Built-in Operations

$\text{\_\_typeof}(v)$ - Evaluates type of the value $v$ in the runtime.

$\text{\_\_int\_width}(t)$ - Evaluates width of the type $t$ where $t$ must be a type in $\mathrm{Integer}$.

$\text{\_\_float\_width}(t)$ - Evaluates width of the type $t$ where $t$ must be a type in $\mathrm{Float}$.

$\text{\_\_narrow\_int}(v, n)$ - Narrow $v$ into an $n$ bits integer, where `v` must be $\mathrm{Integer}$ typed.

$\text{\_\_widen\_uint}(v, n)$ - Widen $v$ into an $n$ bits unsigned integer, where `v` must be $\mathrm{UnsignedInteger}$ typed.

$\text{\_\_widen\_int}(v, n)$ - Widen $v$ into an $n$ bits signed integer, where `v` must be $\mathrm{SignedInteger}$ typed.

$\text{\_\_to\_signed}(v)$ - Convert $v$ into a same-width signed integer, where `v` must be $\mathrm{UnsignedInteger}$ typed.

$\text{\_\_to\_unsigned}(v)$ - Convert $v$ into a same-width signed integer, where `v` must be $\mathrm{SignedInteger}$ typed.

$\text{\_\_to\_f32}(v)$ - Convert $v$ into a 32-bit IEEE754 floating-point number, where `v` must be $\mathrm{Arithm}$ typed.

$\text{\_\_to\_f64}(v)$ - Convert $v$ into a 64-bit IEEE754 floating-point number, where `v` must be $\mathrm{Arithm}$ typed.

$\text{\_\_float\_to\_i8}(v)$ - Converts $v$ into an `i8` value, where `v` must be a $\mathrm{Float}$ typed value.

$\text{\_\_float\_to\_i16}(v)$ - Converts $v$ into an `i16` value, where `v` must be a $\mathrm{Float}$ typed value.

$\text{\_\_float\_to\_i32}(v)$ - Converts $v$ into an `i32` value, where `v` must be a $\mathrm{Float}$ typed value.

$\text{\_\_float\_to\_i64}(v)$ - Converts $v$ into an `i64` value, where `v` must be a $\mathrm{Float}$ typed value.

$\text{\_\_float\_to\_u8}(v)$ - Converts $v$ into an `u8` value, where `v` must be a $\mathrm{Float}$ typed value.

$\text{\_\_float\_to\_u16}(v)$ - Converts $v$ into an `u16` value, where `v` must be a $\mathrm{Float}$ typed value.

$\text{\_\_float\_to\_u32}(v)$ - Converts $v$ into an `u32` value, where `v` must be a $\mathrm{Float}$ typed value.

$\text{\_\_float\_to\_u64}(v)$ - Converts $v$ into an `u64` value, where `v` must be a $\mathrm{Float}$ typed value.

$\text{\_\_int\_to\_width}(v, n)$ - Converts $v$ into an signed integer value with specific width, where `v` must be an $\mathrm{Integer}$ typed value:

$\text{\_\_stack\_depth}(\mu)$ - Evaluate current stack depth in the runtime.

$\text{\_\_stack\_max}(\mu)$ - Evaluate maximum stack depth in the runtime.

$\text{\_\_stack\_align\_diff}(t, \mu)$ - Evaluate difference between current stack top address of $\mu$ and next type $t$ aligned address.

$\text{\_\_sizeof}(t)$ - Evaluate size of an instance of a type.

$\text{\_\_defaultof}(t)$ - Get default value of a type.

$\text{\_\_alloca}(t, \mu)$ - Allocate an instance of a type on the stack.

$\text{\_\_add}(t, x, y)$ - Evaluate $(x + y)$ with operands, valid types are $\mathrm{i8}$, $\mathrm{i16}$, $\mathrm{i32}$, $\mathrm{i64}$, $\mathrm{isize}$, $\mathrm{u8}$, $\mathrm{u16}$, $\mathrm{u32}$, $\mathrm{u64}$, $\mathrm{usize}$, $\mathrm{f32}$, $\mathrm{f64}$, where:

* Both operands must have the same type.
* For $\mathrm{SignedInteger}$ types, overflow's behavior is undefined.
* For $\mathrm{UnsignedInteger}$ types, the result is $(x + y)\ \text{mod}\ 2^{\text{\_\_int\_width}(t)}$.
* For $\mathrm{Float}$ types, see the IEEE-754 standard.

$\text{\_\_sub}(t, x, y)$ - Evaluate $(x - y)$ with operands, valid types are $\mathrm{i8}$, $\mathrm{i16}$, $\mathrm{i32}$, $\mathrm{i64}$, $\mathrm{isize}$, $\mathrm{u8}$, $\mathrm{u16}$, $\mathrm{u32}$, $\mathrm{u64}$, $\mathrm{usize}$, $\mathrm{f32}$, $\mathrm{f64}$, where:

* Both operands must have the same type.
* For $\mathrm{SignedInteger}$ types, underflow's behavior is undefined.
* For $\mathrm{UnsignedInteger}$ types, the result is $(y - x)\ \text{mod}\ 2^{\text{\_\_int\_width}(t)}$.
* For $\mathrm{Float}$ types, see the IEEE-754 standard.

$\text{\_\_mul}(t, x, y)$ - Evaluate $(x \times y)$ with operands, valid types are $\mathrm{i8}$, $\mathrm{i16}$, $\mathrm{i32}$, $\mathrm{i64}$, $\mathrm{isize}$, $\mathrm{u8}$, $\mathrm{u16}$, $\mathrm{u32}$, $\mathrm{u64}$, $\mathrm{usize}$, $\mathrm{f32}$, $\mathrm{f64}$, where:

* Both operands must have the same type.
* For $\mathrm{SignedInteger}$ types, overflow's behavior is undefined.
* For $\mathrm{UnsignedInteger}$ types, the result is $(x \times y)\ \text{mod}\ 2^{\text{\_\_int\_width}(t)}$.
* For $\mathrm{Float}$ types, see the IEEE-754 standard.

$\text{\_\_mul}(t, x, y)$ - Evaluate $(x \div y)$ with operands, valid types are $\mathrm{i8}$, $\mathrm{i16}$, $\mathrm{i32}$, $\mathrm{i64}$, $\mathrm{isize}$, $\mathrm{u8}$, $\mathrm{u16}$, $\mathrm{u32}$, $\mathrm{u64}$, $\mathrm{usize}$, $\mathrm{f32}$, $\mathrm{f64}$, where:

* Both operands must have the same type.
* For $\mathrm{Integer}$ types, the remainder will be discarded and the result will be rounded down to the nearest integer.
  * If the value is $(0\ \text{as}\ t)$, the program should abort.
* For $\mathrm{Float}$ types, see the IEEE-754 standard.

$\text{\_\_mod}(t, x, y)$ - Evaluate $(x\ \text{mod}\ y)$ with operands, valid types are $\mathrm{i8}$, $\mathrm{i16}$, $\mathrm{i32}$, $\mathrm{i64}$, $\mathrm{isize}$, $\mathrm{u8}$, $\mathrm{u16}$, $\mathrm{u32}$, $\mathrm{u64}$, $\mathrm{usize}$, $\mathrm{f32}$, $\mathrm{f64}$, where:

* Both operands must have the same type.
* For $\mathrm{Integer}$ types:
  * The program should abort if $y$ is $(0\ \text{as}\ t)$.
  * Otherwise, the result is remainder of $(x \div y)$.

* For $\mathrm{Float}$ types:
  * The result is $\text{NaN}$ if $y$ is $+0$ or $-0$.
  * If one side is $\text{NaN}$, the result is $\text{NaN}$
  * If one side is $\text{Infinity}$ or $-\text{Infinity}$, the result is $\text{NaN}$
  * Otherwise, the result is remainder of $(x \div y)$.

$\text{\_\_and}(t, x, y)$ - Evaluate $(x\ \text{AND}\ y)$ with operands, valid types are $\mathrm{i8}$, $\mathrm{i16}$, $\mathrm{i32}$, $\mathrm{i64}$, $\mathrm{isize}$, $\mathrm{u8}$, $\mathrm{u16}$, $\mathrm{u32}$, $\mathrm{u64}$, $\mathrm{usize}$, where:

* Both operands must have the same type.

$\text{\_\_or}(t, x, y)$ - Evaluate $(x\ \text{OR}\ y)$ with operands, valid types are $\mathrm{i8}$, $\mathrm{i16}$, $\mathrm{i32}$, $\mathrm{i64}$, $\mathrm{isize}$, $\mathrm{u8}$, $\mathrm{u16}$, $\mathrm{u32}$, $\mathrm{u64}$, $\mathrm{usize}$, where:

* Both operands must have the same type.

$\text{\_\_xor}(t, x, y)$ - Evaluate $(x\ \text{XOR}\ y)$ with operands, valid types are $\mathrm{i8}$, $\mathrm{i16}$, $\mathrm{i32}$, $\mathrm{i64}$, $\mathrm{isize}$, $\mathrm{u8}$, $\mathrm{u16}$, $\mathrm{u32}$, $\mathrm{u64}$, $\mathrm{usize}$, where:

* Both operands must have the same type.