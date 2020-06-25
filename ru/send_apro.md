# `Send` приближение

Некоторые `async fn` автоматы безопасны для отправки между потоками, а другие - нет. Независимо от того или нет `async fn` `Future` является `Send` определяется ли не- `Send` проводятся по типу `.await` точки. Компилятор делает все возможное, чтобы приблизиться, когда значения могут находиться в точке `.await` , но этот анализ слишком консервативен во многих местах сегодня.

Например, рассмотрим простой не `Send` тип, возможно, тип, который содержит `Rc` :

```rust
use std::rc::Rc;

#[derive(Default)]
struct NotSend(Rc<()>);
```

Переменные типа `NotSend` могут кратковременно отображаться как временные в `async fn` даже если результирующий тип `Future` возвращаемый `async fn` должен быть `Send` :

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

This error is correct. If we store `x` into a variable, it won't be dropped until after the `.await`, at which point the `async fn` may be running on a different thread. Since `Rc` is not `Send`, allowing it to travel across threads would be unsound. One simple solution to this would be to `drop` the `Rc` before the `.await`, but unfortunately that does not work today.

Для того , чтобы успешно работать вокруг этой проблемы, возможно , придется ввести блок сферу заключающую какой - либо не `Send` переменные. Это облегчает компилятору понять, что эти переменные не живут в точке `.await` .

```rust
async fn foo() {
    {
        let x = NotSend::default();
    }
    bar().await;
}
```
