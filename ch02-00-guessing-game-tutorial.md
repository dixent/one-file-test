This line creates a new variable named `foo` and binds it to the value of the
`bar` variable. In Rust, variables are immutable by default. We’ll be
discussing this concept in detail in the [“Variables and
Mutability”][variables-and-mutability]<!-- ignore --> section in Chapter 3.
The following example shows how to use `mut` before the variable name to make
a variable mutable:

```rust,ignore
let foo = 5; // immutable
let mut bar = 5; // mutable
```
