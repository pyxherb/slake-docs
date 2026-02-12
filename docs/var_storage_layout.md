# Variable Storage Layout

## Static Fields/Object Fields/Array Elements/Structure Fields

```txt
(↓ From high address to low address)

+------------------------+
| Null Flag (If present) |
+------------------------+
| Actual data            |
+------------------------+
```

* Note: The type information is externally stored.

## Arguments/Coroutine Arguments

Arguments and coroutine arguments are entirely wrapped inside a value.

## Local Variables/Coroutine Local Variables

```txt
(↓ From high address to low address)

+-------------------------------------+
| Actual data                         |
+-------------------------------------+
| Null Flag (If present)              |
+-------------------------------------+
| Extra Type Information (If present) |
+-------------------------------------+
| Type modifier (1 byte)              |
+-------------------------------------+
| Type ID (1 byte)                    | <--- Where the local variable reference refers to
+-------------------------------------+
| (Padding of allocation record)      |
+-------------------------------------+
| Allocation Record                   |
+-------------------------------------+
```
