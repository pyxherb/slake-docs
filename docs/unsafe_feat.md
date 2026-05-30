# Unsafe Features

Slake supports unsafe functionalities as unsafe features, the user may test
the features with `unsafe(feature_name)`.

Sometimes the specification introduces some conceptual unsafe features that is
used for internal use only, language level viability of these features is not
guaranteed.

## Feature List

$\text{unsafe\_enabled}$ - Indicates if setting unsafe features is allowed in this
context.

$\text{local\_delete}$ - Controls if the codes can delete local objects in this
context.