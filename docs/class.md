# Class

## Constructor

In below all single 'members' refer to both variable members and constant
members.

In the beginning of the constructor, all uninitialized members' read type is
internally overriden as `void`, and the compiler will check if all of the
members are initialized (is not still overriden as `void` typed) at the end of
the constructor, all of the members should be initialized at every exit of the
constructor (unless the exit of the constructor throws an exception).

Note that only the compiler can override variable's type as `void`, the user
is not able to define a variable with `void` type.

Because `void` type is not assignable to variable of all types (including
`void` itself), they are not readable before initialized.

Because the language is GC-based, when the exception is thrown, the
fields that are not initialized will remain uninitialized and the garbage
collector will recognize and process them correctly since the object is
unreachable. Note that the exception cannot hold the `this` reference here
if the object's members are not all initialized since the `this` reference
cannot be passed if the members are not all initialized.

Once a member is assigned and with no compilation error, the member is
initialized.

The initialization order is controlled by the user, and the compiler does not
specify.

Pre-initialized constant members are always not writable during the
construction, constant members that are not pre-initialized are always
writable during the whole construction, even they are initialized by previous
expressions in the constructor.

Because of compile-time constant restrictions, the constant members cannot
use any other members as a initial value outside the constructors. The user
also may not use any non-nullable object type or non-nullable structure type
for global/static variables.

Determination of if a member is initialized is checking if the member is
initialized in this path. If the member is not must to be initialized, it will
be regarded as uninitialized (e.g. in if statement, you must initialize it in
all of the branches, and because certainty of initializations in loop
statements cannot be determined, the variable initialized inside the loop will
be always assumed to be not initialized or not re-initialized).

Before all of the members are initialized, the constructor is not able to call
any instance method with `this` during the initialization, nor able passing
`this` reference. Note that accessing fields within `this` reference is allowed
because it does not pass `this` reference directly.

Once all of the members are initialized, the constructor may
pass `this` reference freely, and will be able to call instance methods with
`this`.

Members from the base class should be initialized by the base class'
constructor. Base class' constructor will be called by the compiler before
the subclass' constructor is called, thus the base class' members will always
be initialized before the subclass' constructor is called.

```slake
fn externalClassMethod(obj: Class) {
    obj.method();
}

class Class {
    let p: Class;

    public fn method() {}

    public operator new(q: Class?) {
        // this.method();              // Error: The class is not completely initialized.
        // this.p = this;              // Error: The class is not completely initialized.
        // externalClassMethod(this);  // Error: The class is not completely initialized.
        // p = q;                      // Error: q may be null
        //
        // let f = fn [this]() {};     // Error: Cannot capture `this` when the class is not completely initialized.
        //
        if (q === null)
            throw core.except.RuntimeError("q is null");
        this.p = q;                 // OK: q is not null
        this.method();              // Works here.
        externalClassMethod(this);  // OK, all members are initialized.
    }
}
```

Before the subclass' constructor is called, the object will be regarded as the
base class' instance instead of the subclass' instance. Also, the object cannot
be converted to the subclass if the type of the object has not been switched to
a type that is same to or more derived than the subclass.

The virtual method invocation during the base class' construction will be
dispatched to the base class' virtual method table, which means the method
from the subclass will not be called if the method is overriden by the
subclass.

The type of the object will be switched to the subclass when the subclass'
initialization is done (Usually via a special series of instructions).

```slake
let t: Test;

fn setGlobalTest(test: Test) {
    t = test;
}

fn globalTest() {
    t.test();
}

class Test {
    let x: object;

    fn test() virtual {
    }

    operator new() {
        this.x = new object();
        setGlobalTest(this);
        globalTest(); // Calls Test's test().
    }
}

class Derived(Test) {
    let y: object;
    
    fn test() virtual override {
    }

    operator new(): base() {
        globalTest(); // Calls Test's test().

        // Initialize Derived.
        this.y = new object();
        // Derived is now initialized.
        globalTest(); // Calls Derived's test().
    }
}
```