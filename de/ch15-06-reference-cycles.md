## Reference Cycles Can Leak Memory

Rust’s memory safety guarantees make it difficult, but not impossible, to accidentally create memory that is never cleaned up (known as a *memory leak*). Preventing memory leaks entirely is not one of Rust’s guarantees in the same way that disallowing data races at compile time is, meaning memory leaks are memory safe in Rust. We can see that Rust allows memory leaks by using `Rc<T>` and `RefCell<T>`: it’s possible to create references where items refer to each other in a cycle. This creates memory leaks because the reference count of each item in the cycle will never reach 0, and the values will never be dropped.

<img alt="Reference cycle of lists" class="center" src="https://github.com/dixent/one-file-test/blob/master/img/trpl15-04.svg?raw=true">

![test image](https://i.imgur.com/k2y3saw.png)
