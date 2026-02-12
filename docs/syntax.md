# Slake Syntax Reference

```txt
UTF8_BYTE ::= <Any single byte that is greater than or equal to 0x80>

IDENTIFIER_START ::= ( [`a`-`z``A`-`Z``_`] | UTF8_BYTE )
IDENTIFIER_CONT ::= ( IDENTIFIER_START | [`0`-`9`] )

IDENTIFIER ::= IDENTIFIER_START IDENTIFIER_CONT*

DEC_LITERAL ::= [`0`-`9`]+
HEX_LITERAL ::= `0` ( `x` | `X` ) [`0`-`9``a`-`f``A`-`F`]+
OCT_LITERAL ::= `0` ( `o` | `O` ) [`0`-`7`]+
BIN_LITERAL ::= `0` ( `b` | `B` ) [`0``1`]+

FLOATING_POINT_LITERAL ::= [`0`-`9`]+ `.` [`0`-`9`]+ ( `f` | `F` )

F32_LITERAL ::= FLOATING_POINT_LITERAL ( `f` | `F` )
F64_LITERAL ::= FLOATING_POINT_LITERAL

SINT_LITERAL ::=
    `-`? (
        DEC_LITERAL
        | HEX_LITERAL
        | OCT_LITERAL
        | BIN_LITERAL
    )
UINT_LITERAL ::=
    ( DEC_LITERAL
        | HEX_LITERAL
        | OCT_LITERAL
        | BIN_LITERAL
    ) ( `u` | `U` )

SLONG_LITERAL ::=
    SINT_LITERAL ( `l` | `L` )
    
ULONG_LITERAL ::=
    UINT_LITERAL ( `l` | `L` )

Program ::=
    ModuleDecl?
    ProgramStmt*
    
ProgramStmt ::=
    AccessModifiers? (
        InterfaceDef
        | ClassDef
        | VarDef `;`
        | ConstDef `;`
        | ConstEnumDef
        | ScopedEnumDef
        | UnionEnumDef
        | ExtensionDef
        | FnDecl `;`
        | FnDef
        | AccessorDef
        | CoroutineDecl `;`
        | CoroutineDef
        | AccessorDef
    )
    
AccessModifier ::=
    `public`
    | `static`
    | `native`
    
AccessModifiers ::=
    AccessModifier+

IdRefEntry ::= ( IDENTIFIER | `operator` OperatorName ) ( `<` GenericArg ( `,` GenericArg )*  `>` )?

IdRef ::= 
    `this`
    | IdRefEntry (`.` IdRefEntry)*
    | ( `this` `.` ) IdRefEntry (`.` IdRefEntry)*
    | `::` IdRefEntry (`.` IdRefEntry)*

ModuleDecl ::= `module` IdRef `;`

GenericArgExpr ::= `(` Expr `)`

GenericArg ::=
    TypeName
    | GenericArgExpr

SignedIntegerTypeName ::=
    `i8`
    | `i16`
    | `i32`
    | `i64`
    
UnsignedIntegerTypeName ::=
    `u8`
    | `u16`
    | `u32`
    | `u64`
    
IntegerTypeName ::=
    SignedIntegerTypeName
    | UnsignedIntegerTypeName
    
BoolTypeName ::=
    `bool`
    
FloatingPointTypeName ::=
    `f32`
    | `f64`
    
SIMDTypeName ::=
    `simd_t` `<` TypeName `,` GenericArgExpr `>`
    
NeverTypeName ::=
    `never_t`
    
CustomTypeName ::=
    IdRef
    
ArrayTypeName ::=
    TypeName `[` `]`
    
ConstTypeName ::=
    TypeName `const`
    
LocalTypeName ::=
    TypeName `local`
    
NullableTypeName ::=
    TypeName `?`
    
ExtensionModifier ::=
    `&&` TypeName
    
TypeNameWithExtensions ::=
    TypeName ExtensionModifier*

TypeName ::=
    IntegerTypeName
    | BoolTypeName
    | FloatingPointTypeName
    | SIMDTypeName
    | NeverTypeName
    | CustomTypeName
    | ArrayTypeName
    | ConstTypeName
    | LocalTypeName
    | NullableTypeName
    | TypeNameWithExtensions

InheritedTypeConstraint ::=
    `(` TypeName `)`
    
ImplTypeConstraint ::=
    `:` TypeName ( `+` TypeName )*
    
GenericParam ::=
    IDENTIFIER InheritedTypeConstraint? ImplTypeConstraint?
    | `const` IDENTIFIER `:` TypeName
    
GenericParamDefs ::=
    `<` GenericParam ( `,` GenericParam )* `>`
    
EnumItem ::=
    IDENTIFIER ( `=` Expr )?
    
EnumItemList ::=
    EnumItem ( `,` EnumItem )*
    
ConstEnumDef ::=
    `enum` `const` `(` TypeName `)` `{` EnumItemList? `}`
    
ScopedEnumDef ::=
    `enum` IDENTIFIER `(` TypeName `)` `{` EnumItemList? `}`
    
UnionEnumItemField ::=
    IDENTIFIER `:` TypeName
    
UnionEnumItemFieldList ::=
    UnionEnumItemField ( `,` UnionEnumItemField )*
    
UnionEnumItem ::=
    IDENTIFIER `(` UnionEnumItemFieldList? `)`
    
UnionEnumItemList ::=
    UnionEnumItem ( `,` UnionEnumItem )*
    
UnionEnumDef ::=
    `enum` `union` IDENTIFIER `{` UnionEnumItemList? `}`
    
AccessorSetterDef ::=
    `in` ( `{` FnStmt `}` | `native` )
    
AccessorGetterDef ::=
    `out` ( `{` FnStmt `}` | `native` )
    
AccessorDef ::=
    `var` IDENTIFIER `:` TypeName `{` (AccessorGetterDef | AccessorSetterDef)* `}`

ClassDef ::=
    `class` IDENTIFIER GenericParamDefs? InheritedTypeConstraint? ImplTypeConstraint? `{`
        ClassStmt*
    `}`
    
ClassStmt ::=
    AccessModifiers? (
        InterfaceDef
        | ClassDef
        | VarDef `;`
        | ConstDef `;`
        | ConstEnumDef
        | ScopedEnumDef
        | UnionEnumDef
        | ExtensionDef
        | FnDecl `;`
        | FnDef
        | CoroutineDecl `;`
        | CoroutineDef
        | AccessorDef
    )

InterfaceDef ::=
    `interface` IDENTIFIER GenericParamDefs? ImplTypeConstraint? `{`
        ClassStmt*
    `}`
    
StructDef ::=
    `struct` IDENTIFIER GenericParamDefs? ImplTypeConstraint? `{`
        ClassStmt*
    `}`
    
ExtensionDef ::=
    `extension` IDENTIFIER InheritedTypeConstraint ImplTypeConstraint? `{` ExtensionStmt* `}`
    
ExtensionStmt ::=
    AccessModifier? (
        FnDecl `;`
        | FnDef
    )
    
VarDefEntry ::= IDENTIFIER ( `:` TypeName )? ( `=` Expr )?

VarDef ::= `let` VarDefEntry+

ConstDef ::= `const` VarDefEntry+

Param ::= `restrict`? IDENTIFIER `:` TypeName

ParamList ::= Param ( `,` Param )*

ClosureParamList ::= `[` ParamList? `]`

FnParamList ::= `(` ParamList? `)`

ReturnTypeDecl ::= `->` TypeName

OperatorName ::=
    `+`
    | `-`
    | `*`
    | `/`
    | `%`
    | `&`
    | `|`
    | `^`
    | `&&`
    | `||`
    | `>>`
    | `<<`
    | `[` `]`
    | `>`
    | `<`
    | `==`
    | `>=`
    | `<=`
    | `=`
    | `+=`
    | `-=`
    | `*=`
    | `/=`
    | `%=`
    | `&=`
    | `|=`
    | `^=`

FnDecl ::= ( `fn` IDENTIFIER | `operator` OperatorName ) ClosureParamList? FnParamList `const`? `virtual`? `override`? ReturnTypeDecl?

FnDef ::= FnDecl `{` FnStmt* `}`

CoroutineDecl ::= `async` IDENTIFIER FnParamList `const`? `virtual`? `override`? ReturnTypeDecl?

CoroutineDef ::= CoroutineDecl `{` FnStmt* `}`

ForStmt ::=
    `for` `(` VarDefEntry* `;` Expr? `;` Expr? `)` FnStmt
    
ForEachStmt ::=
    `foreach` `(` VarDefEntry `:` Expr `)` FnStmt
    
WhileStmt ::=
    `while` `(` Expr `)` FnStmt
    
DoWhileStmt ::=
    `do` FnStmt `while` `(` Expr `)`
    
WithStmt ::=
    `with` `<` IDENTIFIER InheritedTypeConstraint? ImplConstraint? `>` FnStmt ElseBranch?
    
ReturnStmt ::=
    `return` Expr? `;`
    
YieldStmt ::=
    `yield` Expr? `;`

BreakStmt ::= `break` `;`

ContinueStmt ::= `continue` `;`

LabelStmt ::= `:` IDENTIFIER

CaseLabelStmt ::= `case` Expr `:`

DefaultLabelStmt ::= `default` `:`

SwitchStmt ::= `switch` `const`? `(` Expr `)` `{` FnStmt* `}`

IfStmt ::= `if` `(` Expr `)` FnStmt ElseBranch?

ElseBranch ::= `else` FnStmt

FnStmt ::=
    Expr `;`
    | VarDef `;`
    | ConstDef `;`
    | ForStmt
    | ForEachStmt
    | WhileStmt
    | DoWhileStmt
    | WithStmt
    | ReturnStmt
    | YieldStmt
    | BreakStmt
    | ContinueStmt
    | LabelStmt
    | CaseLabelStmt
    | DefaultLabelStmt
    | SwitchStmt
    | IfStmt
    
IdRefExpr ::=
    IdRef
    | Expr `.` IdRef
    
AccessorFieldAccessingExpr ::= IdRef `var`
    
UnaryExpr ::=
    `!` Expr
    | `-` Expr
    | `~` Expr
    | `++` Expr
    | `--` Expr
    | Expr `--`
    | Expr `++`
    
BinaryExpr ::=
    Expr `+` Expr
    | Expr `-` Expr
    | Expr `*` Expr
    | Expr `/` Expr
    | Expr `%` Expr
    | Expr `&` Expr
    | Expr `|` Expr
    | Expr `^` Expr
    | Expr `&&` Expr
    | Expr `||` Expr
    | Expr `>>` Expr
    | Expr `<<` Expr
    | Expr `[` Expr `]`
    | Expr `>` Expr
    | Expr `<` Expr
    | Expr `==` Expr
    | Expr `>=` Expr
    | Expr `<=` Expr
    | Expr `=` Expr
    | Expr `+=` Expr
    | Expr `-=` Expr
    | Expr `*=` Expr
    | Expr `/=` Expr
    | Expr `%=` Expr
    | Expr `&=` Expr
    | Expr `|=` Expr
    | Expr `^=` Expr
    
TernaryExpr ::=
    Expr `?` Expr `:` Expr
    
ArgList ::=
    Expr ( `,` Expr )*

CallExpr ::=
    Expr `(` ArgList? `)`
    
CastExpr ::=
    Expr `as` Expr
    
NewExpr ::=
    `new` ( `static` | `local` )? TypeName `(` ArgList? `)`
    
AllocaExpr ::=
    `alloca` TypeName `(` ArgList? `)`
    
CaptureVar ::=
    `&`? IDENTIFIER
    | IDENTIFIER `=` `&`? IDENTIFIER
    
CaptureVarList ::=
    CaptureVar ( `,` CaptureVar )*
    
LambdaCaptureVarList ::=
    `[` CaptureVarList `]`
    
LambdaExpr ::=
    `fn` LambdaCaptureVarList? FnParamList ReturnTypeDecl? `{` FnStmt* `}`
    
SignedIntegerLiteral ::=
    SINT_LITERAL
    | SLONG_LITERAL
    
UnsignedIntegerLiteral ::=
    UINT_LITERAL
    | ULONG_LITERAL
    
IntegerLiteral ::=
    SignedIntegerLiteral
    | UnsignedIntegerLiteral
    
BoolLiteral ::=
    `true`
    | `false`
    
FloatingPointLiteral ::=
    F32_LITERAL
    | F64_LITERAL
    
NullLiteral ::=
    `null`

Literal ::=
    IntegerLiteral
    | BoolLiteral
    | FloatingPointLiteral
    | NullLiteral
    
AccessorThisFieldAccessingExpr ::= `var`

MatchCase ::=
    `case` Expr `:` Expr
    
DefaultMatchCase ::=
    `default` `:` Expr
    
MatchCaseList ::=
    ( MatchCase | DefaultMatchCase )+

MatchExpr ::=
    `match` `const`? `(` Expr `)` ReturnTypeDecl? `{` MatchCaseList? `}`
    
Expr ::=
    `(` Expr `)`
    | IdRefExpr
    | UnaryExpr
    | BinaryExpr
    | TernaryExpr
    | CallExpr
    | CastExpr
    | Literal
    | NewExpr
    | AllocaExpr
    | LambdaExpr
    | AccessorThisFieldAccessingExpr
    | AccessorFieldAccessingExpr
    | MatchExpr
```
