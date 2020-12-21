```rust
use std::io::Write;

fn trait_obj(w: &amp;Write) {
    generic(w);
}

fn generic&lt;W: Write&gt;(_w: &amp;W) {}
```
