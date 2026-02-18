# Variables

## Accessing Nullable Variables

Nullable variables can only be used after a null check or a non-null assignment:

```slake
fn externalNullable() -> i32?;

fn test(cond: bool) -> i32 {
    let a: i32?;
    let b: i32 = 0;

    // b = a;   // Error: a is null

    if (bool)
        a = 123;
    
    // b = a;   // Error: a may be null

    a = 123;
    b = a;      // OK: a must not be null

    let result: i32? = externalNullable();

    if (result != null)
        b += result;

    return b;
}
```

For nullable global variables, static member variables and instance member
variables, the direct null check is invalid, or the variable must be locked
by a `synchronized` statement to perform a valid null check with itself.

```slake
let global: i32?;

fn test() -> i32 {
    let x: i32 = 0;

    // Meaningless and invalid
    // if (global != null) {
        // x += global; // Error: `global` may be changed to null by other concurrent units.
    // }

    synchronized global; // Lock the global variable.

    if (global != null) {
        x += global;
    }

    x += 123;

    return x;
}
```

The user can always perform valid null check with local variables, which means
the user can copy or move value of any variables where direct null checks are
invalid into a local variable to perform a valid null check.

```slake
let global: i32?;

fn test() -> i32 {
    let x: i32 = 0;

    // Meaningless and invalid,
    // `global` may be changed to null by other concurrent units.
    // if (global != null) {
        // x += global;
    // }

    let copiedGlobal: i32? = global;

    if (copiedGlobal != null) {
        x += copiedGlobal;
    }

    x += 123;

    return x;
}
```