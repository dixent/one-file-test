# `Send` приближение

Some `async fn` state machines are safe to be sent across threads, while others are not. Whether or not an `async fn` `Future` is `Send` is determined by whether a non-`Send` type is held across an `.await` point. The compiler does its best to approximate when values may be held across an `.await` point, but this analysis is too conservative in a number of places today.

Например, рассмотрим простой не `Send` тип, возможно, тип, который содержит `Rc` :

```rust
use std::rc::Rc;

#[derive(Default)]
struct NotSend(Rc<()>);
```

Переменные типа `NotSend` могут кратковременно появляться как временные в `async fn` даже если результирующий тип `Future` возвращаемый `async fn` должен быть `Send` :

```rust
async fn bar() {}
async fn foo() {
    NotSend::default();
    bar().await;
}

fn require_send(_: impl Send) {}

fn main() {
    require_send(foo());
}
```

Однако если мы изменим `foo` для хранения `NotSend` в переменной, этот пример больше не будет компилироваться:

```rust
async fn foo() {
    let x = NotSend::default();
    bar().await;
}
```

```
error[E0277]: `std::rc::Rc<()>` cannot be sent between threads safely
  --> src/main.rs:15:5
   |
15 |     require_send(foo());
   |     ^^^^^^^^^^^^ `std::rc::Rc<()>` cannot be sent between threads safely
   |
   = help: within `impl std::future::Future`, the trait `std::marker::Send` is not implemented for `std::rc::Rc<()>`
   = note: required because it appears within the type `NotSend`
   = note: required because it appears within the type `{NotSend, impl std::future::Future, ()}`
   = note: required because it appears within the type `[static generator@src/main.rs:7:16: 10:2 {NotSend, impl std::future::Future, ()}]`
   = note: required because it appears within the type `std::future::GenFuture<[static generator@src/main.rs:7:16: 10:2 {NotSend, impl std::future::Future, ()}]>`
   = note: required because it appears within the type `impl std::future::Future`
   = note: required because it appears within the type `impl std::future::Future`
note: required by `require_send`
  --> src/main.rs:12:1
   |
12 | fn require_send(_: impl Send) {}
   | ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

error: aborting due to previous error

For more information about this error, try `rustc --explain E0277`.
```

Эта ошибка верна. Если мы сохраним `x` в переменной, он не будет `.await` до тех пор, пока не будет завершен `.await` , после чего `async fn` `.await` `async fn` может выполняться в другом потоке. Поскольку `Rc` не является `Send` , разрешение проходить через потоки было бы нецелесообразным. Одним из простых решений этого было бы `drop` `Rc` до `.await` , но, к сожалению, сегодня это не работает.

Для того , чтобы успешно работать вокруг этой проблемы, возможно , придется ввести блок сферу заключающую какой - либо не `Send` переменные. Это облегчает компилятору понять, что эти переменные не живут через точку `.await` .

```rust
async fn foo() {
    {
        let x = NotSend::default();
    }
    bar().await;
}
```
