```rust
use std::io::Write;

fn trait_obj(w: &Write) {
    generic(w);
}

fn generic<W: Write>(_w: &W) {}
```
