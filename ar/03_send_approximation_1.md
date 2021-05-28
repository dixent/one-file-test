# `Send` تقريب

بعض `async fn` state آمنة لإرسالها عبر سلاسل الرسائل ، في حين أن البعض الآخر ليس كذلك. يتم تحديد ما إذا كان غير `async fn` `Future` هو `Send` لا من خلال ما إذا كان النوع `Send` `.await` عبر نقطة .await. يبذل المترجم قصارى جهده لتقريب الوقت الذي يمكن فيه الاحتفاظ بالقيم عبر `.await` ، ولكن هذا التحليل متحفظ للغاية في عدد من الأماكن اليوم.

على سبيل المثال ، ضع في اعتبارك نوعًا بسيطًا غير `Send` ، ربما نوع يحتوي على `Rc` :

```rust
use std::rc::Rc;

# [derive(Default)]

struct NotSend(Rc<()>);
```

يمكن أن تظهر المتغيرات من النوع `NotSend` `async fn` s غير المتزامن حتى عندما يجب `Send` `Future` الناتج الذي تم إرجاعه بواسطة `async fn` :

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

ومع ذلك ، إذا قمنا بتغيير `foo` لتخزين `NotSend` في متغير ، فإن هذا المثال لم يعد يجمع:

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

هذا الخطأ صحيح. إذا قمنا بتخزين `x` في متغير ، فلن يتم إسقاطه إلا بعد `.await` ، وعند هذه النقطة `async fn` على مؤشر ترابط مختلف. نظرًا لأن `Rc` ليس `Send` ، فإن السماح له بالسفر عبر الخيوط سيكون غير سليم. أحد الحلول البسيطة لهذا هو `drop` `Rc` قبل `.await` ، لكن للأسف هذا لا يعمل اليوم.

للتغلب على هذه المشكلة بنجاح ، قد تضطر إلى تقديم نطاق كتلة يغلف أي متغيرات `Send` وهذا يجعل من السهل على المترجم أن أقول أن هذه المتغيرات لا يعيش عبر `.await` نقطة.

```rust
async fn foo() {
    {
        let x = NotSend::default();
    }
    bar().await;
}
```
