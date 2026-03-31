# Built-in Operations

$\text{\_\_typeof}(v)$ - Evaluates type of the value $v$ in the runtime.

$\text{\_\_int\_width}(t)$ - Evaluates width of the type $t$ where $t$ must be a type in $Integer$.

$\text{\_\_narrow\_int}(v, n)$ - Narrow $v$ into an $n$ bits integer, where `v` must be $Integer$ typed.

$\text{\_\_widen\_uint}(v, n)$ - Widen $v$ into an $n$ bits unsigned integer, where `v` must be $UnsignedInteger$ typed.

$\text{\_\_widen\_int}(v, n)$ - Widen $v$ into an $n$ bits signed integer, where `v` must be $SignedInteger$ typed.

$\text{\_\_to\_signed}(v)$ - Convert $v$ into a same-width signed integer, where `v` must be $UnsignedInteger$ typed.

$\text{\_\_to\_unsigned}(v)$ - Convert $v$ into a same-width signed integer, where `v` must be $SignedInteger$ typed.

$\text{\_\_to\_f32}(v)$ - Convert $v$ into a 32-bit IEEE754 floating-point number, where `v` must be $Arithm$ typed.

$\text{\_\_to\_f64}(v)$ - Convert $v$ into a 64-bit IEEE754 floating-point number, where `v` must be $Arithm$ typed.

$\text{\_\_float\_to\_i8}(v)$ - Converts $v$ into an `i8` value, where `v` must be a $Float$ typed value.

$\text{\_\_float\_to\_i16}(v)$ - Converts $v$ into an `i16` value, where `v` must be a $Float$ typed value.

$\text{\_\_float\_to\_i32}(v)$ - Converts $v$ into an `i32` value, where `v` must be a $Float$ typed value.

$\text{\_\_float\_to\_i64}(v)$ - Converts $v$ into an `i64` value, where `v` must be a $Float$ typed value.

$\text{\_\_float\_to\_u8}(v)$ - Converts $v$ into an `u8` value, where `v` must be a $Float$ typed value.

$\text{\_\_float\_to\_u16}(v)$ - Converts $v$ into an `u16` value, where `v` must be a $Float$ typed value.

$\text{\_\_float\_to\_u32}(v)$ - Converts $v$ into an `u32` value, where `v` must be a $Float$ typed value.

$\text{\_\_float\_to\_u64}(v)$ - Converts $v$ into an `u64` value, where `v` must be a $Float$ typed value.

$\text{\_\_int\_to\_width}(v, n)$ - Converts $v$ into an signed integer value with specific width, where `v` must be an $Integer$ typed value:

$\text{\_\_stack\_depth}(\mu)$ - Evaluate current stack depth in the runtime.

$\text{\_\_stack\_max}(\mu)$ - Evaluate maximum stack depth in the runtime.

$\text{\_\_sizeof}(t)$ - Evaluate size of an instance of a type.

$\text{\_\_alloca}(t, \mu)$ - Allocate an instance of a type on the stack.