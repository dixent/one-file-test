## Los ciclos de referencia pueden perder memoria

Las garantías de seguridad de la memoria de Rust hacen que sea difícil, pero no imposible, crear accidentalmente memoria que nunca se limpia (conocida como *pérdida de memoria* ). Prevenir por completo las pérdidas de memoria no es una de las garantías de Rust de la misma manera que lo es rechazar las carreras de datos en tiempo de compilación, lo que significa que las pérdidas de memoria son seguras en Rust. Podemos ver que Rust permite pérdidas de memoria usando `Rc<T>` y `RefCell<T>` : es posible crear referencias donde los elementos se refieren entre sí en un ciclo. Esto crea pérdidas de memoria porque el recuento de referencia de cada elemento en el ciclo nunca alcanzará 0, y los valores nunca se descartarán.

### Crear un ciclo de referencia

Veamos cómo puede ocurrir un ciclo de referencia y cómo prevenirlo, comenzando con la definición de la enumeración de la `List` y un método de `tail` en el Listado 15-25:

<span class="filename">Nombre de archivo: src / main.rs</span>

<!-- Hidden fn main is here to disable the automatic wrapping in fn main that
doc tests do; the `use List` fails if this listing is put within a main -->

```rust
# fn main() {}
use std::rc::Rc;
use std::cell::RefCell;
use crate::List::{Cons, Nil};

#[derive(Debug)]
enum List {
    Cons(i32, RefCell<Rc<List>>),
    Nil,
}

impl List {
    fn tail(&self) -> Option<&RefCell<Rc<List>>> {
        match self {
            Cons(_, item) => Some(item),
            Nil => None,
        }
    }
}
```

<span class="caption">Listado 15-25: Una definición de lista de contras que contiene un <code>RefCell<T></code> para que podamos modificar a qué se refiere una variante <code>Cons</code></span>

Estamos utilizando otra variante de la `List` definición del listado 15-5. El segundo elemento en la variante `Cons` ahora es `RefCell<Rc<List>>` , lo que significa que en lugar de tener la capacidad de modificar el valor `i32` como lo hicimos en el Listado 15-24, queremos modificar qué valor de `List` apunta una variante `Cons` a. También estamos agregando un método de `tail` para que sea conveniente para nosotros acceder al segundo elemento si tenemos una variante `Cons` .

En el Listado 15-26, estamos agregando una función `main` que usa las definiciones del Listado 15-25. Este código crea una lista en `a` y una lista de `b` que apunta a la lista en `a` . Luego modifica la lista en `a` para señalar `b` , creando un ciclo de referencia. Hay `println!` declaraciones en el camino para mostrar cuáles son los recuentos de referencia en varios puntos de este proceso.

<span class="filename">Nombre de archivo: src / main.rs</span>

```rust
# use crate::List::{Cons, Nil};
# use std::rc::Rc;
# use std::cell::RefCell;
# #[derive(Debug)]
# enum List {
#     Cons(i32, RefCell<Rc<List>>),
#     Nil,
# }
#
# impl List {
#     fn tail(&self) -> Option<&RefCell<Rc<List>>> {
#         match self {
#             Cons(_, item) => Some(item),
#             Nil => None,
#         }
#     }
# }
#
fn main() {
    let a = Rc::new(Cons(5, RefCell::new(Rc::new(Nil))));

    println!("a initial rc count = {}", Rc::strong_count(&a));
    println!("a next item = {:?}", a.tail());

    let b = Rc::new(Cons(10, RefCell::new(Rc::clone(&a))));

    println!("a rc count after b creation = {}", Rc::strong_count(&a));
    println!("b initial rc count = {}", Rc::strong_count(&b));
    println!("b next item = {:?}", b.tail());

    if let Some(link) = a.tail() {
        *link.borrow_mut() = Rc::clone(&b);
    }

    println!("b rc count after changing a = {}", Rc::strong_count(&b));
    println!("a rc count after changing a = {}", Rc::strong_count(&a));

    // Uncomment the next line to see that we have a cycle;
    // it will overflow the stack
    // println!("a next item = {:?}", a.tail());
}
```

<span class="caption">Listado 15-26: Creación de un ciclo de referencia de dos valores de <code>List</code> que se apuntan entre sí</span>

Creamos una instancia `Rc<List>` contiene un valor `List` en la variable `a` con una lista inicial de `5, Nil` . Luego creamos una instancia `Rc<List>` contiene otro valor de `List` en la variable `b` que contiene el valor 10 y apunta a la lista en `a` .

Modificamos `a` para que apunte a `b` lugar de `Nil` , creando un ciclo. Hacemos eso usando el método `tail` para obtener una referencia a `RefCell<Rc<List>>` en `a` , que colocamos en el `link` variable. Luego usamos el método `borrow_mut` en `RefCell<Rc<List>>` para cambiar el valor dentro de un `Rc<List>` que contiene un valor `Nil` a `Rc<List>` en `b` .

Cuando ejecutamos este código, conservamos el último `println!` comentado por el momento, obtendremos esta salida:

```text
a initial rc count = 1
a next item = Some(RefCell { value: Nil })
a rc count after b creation = 2
b initial rc count = 1
b next item = Some(RefCell { value: Cons(5, RefCell { value: Nil }) })
b rc count after changing a = 2
a rc count after changing a = 2
```

El recuento de referencia de las instancias `Rc<List>` en `a` y `b` es 2 después de cambiar la lista en `a` para apuntar a `b` . Al final de `main` , Rust intentará soltar `b` primero, lo que disminuirá el recuento de la instancia `Rc<List>` en `b` en 1.

Sin embargo, debido `a` que a todavía hace referencia a la `Rc<List>` que estaba en `b` , esa `Rc<List>` tiene un recuento de 1 en lugar de 0, por lo que la memoria que `Rc<List>` tiene en el montón no se eliminará. La memoria simplemente se quedará allí con una cuenta de 1, para siempre. Para visualizar este ciclo de referencia, hemos creado un diagrama en la Figura 15-4.

<img alt="Reference cycle of lists" class="center" src="../img/trpl15-04.svg">

<span class="caption">Figura 15-4: ciclo de referencia A de las listas de <code>a</code> y <code>b</code> apuntando uno al otro</span>

Si descomenta la última `println!` y ejecute el programa, Rust intentará imprimir este ciclo con `a` apunte a `b` apuntando a `a` y así sucesivamente hasta que desborde la pila.

En este caso, justo después de crear el ciclo de referencia, el programa finaliza. Las consecuencias de este ciclo no son muy graves. Sin embargo, si un programa más complejo asignara mucha memoria en un ciclo y lo retuviera durante mucho tiempo, el programa usaría más memoria de la necesaria y podría abrumar al sistema, haciendo que se quedara sin memoria disponible.

Crear ciclos de referencia no se hace fácilmente, pero tampoco es imposible. Si tiene `RefCell<T>` que contienen valores `Rc<T>` o combinaciones anidadas similares de tipos con mutabilidad interior y recuento de referencias, debe asegurarse de no crear ciclos; no puedes confiar en Rust para atraparlos. La creación de un ciclo de referencia sería un error lógico en su programa que debería utilizar pruebas automatizadas, revisiones de código y otras prácticas de desarrollo de software para minimizar.

Otra solución para evitar los ciclos de referencia es reorganizar sus estructuras de datos para que algunas referencias expresen su propiedad y otras no. Como resultado, puede tener ciclos formados por algunas relaciones de propiedad y algunas relaciones de no propiedad, y solo las relaciones de propiedad afectan si se puede descartar o no un valor. En el Listado 15-25, siempre queremos que las variantes `Cons` tengan su propia lista, por lo que no es posible reorganizar la estructura de datos. Veamos un ejemplo usando gráficos compuestos de nodos primarios y nodos secundarios para ver cuándo las relaciones de no propiedad son una forma apropiada de prevenir los ciclos de referencia.

### Prevención de ciclos de referencia: convertir un `Rc<T>` en un `Weak<T>`

Hasta ahora, hemos demostrado que llamar a `Rc::clone` aumenta el `strong_count` de una instancia `Rc<T>` , y una instancia `Rc<T>` solo se limpia si su `strong_count` es 0. También puede crear una *referencia débil* al valor dentro de una instancia de `Rc<T>` llamando a `Rc::downgrade` y pasando una referencia a `Rc<T>` . Cuando llama a `Rc::downgrade` , obtiene un puntero inteligente de tipo `Weak<T>` . En lugar de aumentar el `strong_count` en la instancia `Rc<T>` en 1, llamar a `Rc::downgrade` aumenta el `weak_count` en 1. El tipo `Rc<T>` usa `weak_count` para realizar un seguimiento de cuántas referencias `Weak<T>` existen, de forma similar a `strong_count` . La diferencia es que la cuenta `weak_count` no necesita ser 0 para que la instancia `Rc<T>` se limpie.

Las referencias fuertes son cómo puede compartir la propiedad de una instancia `Rc<T>` . Las referencias débiles no expresan una relación de propiedad. No causarán un ciclo de referencia porque cualquier ciclo que involucre algunas referencias débiles se interrumpirá una vez que el recuento de referencia fuerte de los valores involucrados sea 0.

Debido a que el valor al que hace referencia `Weak<T>` podría haberse eliminado, para hacer algo con el valor al que apunta un `Weak<T>` , debe asegurarse de que el valor aún exista. Haga esto llamando al método de `upgrade` en una instancia `Weak<T>` , que devolverá una `Option<Rc<T>>` . Obtendrá un resultado de `Some` si el valor `Rc<T>` aún no se ha eliminado y un resultado de `None` si el valor `Rc<T>` se ha eliminado. Debido a que la `upgrade` devuelve una `Option<T>` , Rust se asegurará de que se manejen el caso `Some` y el caso `None` , y no habrá un puntero no válido.

Como ejemplo, en lugar de utilizar una lista cuyos elementos solo saben sobre el siguiente elemento, crearemos un árbol cuyos elementos sepan sobre sus elementos secundarios *y* sus elementos principales.

#### Crear una estructura de datos de árbol: un `Node` con nodos secundarios

Para comenzar, construiremos un árbol con nodos que conozcan sus nodos secundarios. Crearemos una estructura llamada `Node` que contenga su propio valor `i32` , así como referencias a sus valores de `Node` secundarios:

<span class="filename">Nombre de archivo: src / main.rs</span>

```rust
use std::rc::Rc;
use std::cell::RefCell;

#[derive(Debug)]
struct Node {
    value: i32,
    children: RefCell<Vec<Rc<Node>>>,
}
```

Queremos que un `Node` dueño de sus hijos, y queremos compartir esa propiedad con variables para que podamos acceder a cada `Node` en el árbol directamente. Para hacer esto, definimos los elementos `Vec<T>` como valores de tipo `Rc<Node>` . También queremos modificar qué nodos son hijos de otro nodo, por lo que tenemos un `RefCell<T>` en `children` alrededor del `Vec<Rc<Node>>` .

A continuación, utilizaremos nuestra definición de estructura y crearemos una instancia de `Node` llamada `leaf` con el valor 3 y sin hijos, y otra instancia llamada `branch` con el valor 5 y `leaf` como uno de sus hijos, como se muestra en el Listado 15-27:

<span class="filename">Nombre de archivo: src / main.rs</span>

```rust
# use std::rc::Rc;
# use std::cell::RefCell;
#
# #[derive(Debug)]
# struct Node {
#     value: i32,
#    children: RefCell<Vec<Rc<Node>>>,
# }
#
fn main() {
    let leaf = Rc::new(Node {
        value: 3,
        children: RefCell::new(vec![]),
    });

    let branch = Rc::new(Node {
        value: 5,
        children: RefCell::new(vec![Rc::clone(&leaf)]),
    });
}
```

<span class="caption">Listado 15-27: Crear un nodo <code>leaf</code> sin hijos y un nodo <code>branch</code> con <code>leaf</code> como uno de sus hijos</span>

Clonamos el `Rc<Node>` en la `leaf` y lo almacenamos en la `branch` , lo que significa que el `Node` en la `leaf` ahora tiene dos propietarios: `leaf` y `branch` . Podemos ir de `branch` en `leaf` través de `branch.children` , pero no hay forma de ir de `leaf` en `branch` . La razón es que la `leaf` no tiene referencia a la `branch` y no sabe que están relacionados. Queremos que la `leaf` sepa que la `branch` es su padre. Lo haremos a continuación.

#### Agregar una referencia de un niño a su padre

Para que el nodo secundario tenga conocimiento de su elemento primario, debemos agregar un campo `parent` a nuestra definición de estructura de `Node` . El problema está en decidir cuál debería ser el tipo de `parent` . Sabemos que no puede contener un `Rc<T>` , porque eso crearía un ciclo de referencia con `leaf.parent` apuntando a `branch` y `branch.children` apuntando a `leaf` , lo que haría que sus valores `strong_count` nunca sean 0.

Pensando en las relaciones de otra manera, un nodo padre debería ser propietario de sus hijos: si un nodo padre se cae, sus nodos hijos también deberían caerse. Sin embargo, un elemento secundario no debe ser el propietario de su elemento primario: si descartamos un nodo secundario, el elemento primario aún debe existir. ¡Este es un caso para referencias débiles!

Entonces, en lugar de `Rc<T>` , haremos que el tipo de `parent` use `Weak<T>` , específicamente un `RefCell<Weak<Node>>` . Ahora nuestra definición de estructura de `Node` se ve así:

<span class="filename">Nombre de archivo: src / main.rs</span>

```rust
use std::rc::{Rc, Weak};
use std::cell::RefCell;

#[derive(Debug)]
struct Node {
    value: i32,
    parent: RefCell<Weak<Node>>,
    children: RefCell<Vec<Rc<Node>>>,
}
```

Un nodo podrá hacer referencia a su nodo padre pero no es dueño de su padre. En el Listado 15-28, actualizamos `main` para usar esta nueva definición para que el nodo `leaf` tenga una forma de referirse a su `branch` :

<span class="filename">Nombre de archivo: src / main.rs</span>

```rust
# use std::rc::{Rc, Weak};
# use std::cell::RefCell;
#
# #[derive(Debug)]
# struct Node {
#     value: i32,
#     parent: RefCell<Weak<Node>>,
#     children: RefCell<Vec<Rc<Node>>>,
# }
#
fn main() {
    let leaf = Rc::new(Node {
        value: 3,
        parent: RefCell::new(Weak::new()),
        children: RefCell::new(vec![]),
    });

    println!("leaf parent = {:?}", leaf.parent.borrow().upgrade());

    let branch = Rc::new(Node {
        value: 5,
        parent: RefCell::new(Weak::new()),
        children: RefCell::new(vec![Rc::clone(&leaf)]),
    });

    *leaf.parent.borrow_mut() = Rc::downgrade(&branch);

    println!("leaf parent = {:?}", leaf.parent.borrow().upgrade());
}
```

<span class="caption">Listado 15-28: Un nodo <code>leaf</code> con una referencia débil a su <code>branch</code> nodo padre</span>

La creación del nodo `leaf` parece a cómo se veía la creación del nodo `leaf` en el Listado 15-27 con la excepción del campo `parent` : la `leaf` comienza sin un padre, por lo que creamos una nueva instancia de referencia `Weak<Node>` vacía.

En este punto, cuando intentamos obtener una referencia al padre de la `leaf` utilizando el método de `upgrade` , obtenemos un valor `None` . ¡Lo vemos en el resultado de la primera `println!` declaración:

```text
leaf parent = None
```

Cuando creamos el nodo de `branch` , también tendrá una nueva referencia `Weak<Node>` en el campo `parent` , porque la `branch` no tiene un nodo primario. Todavía tenemos la `leaf` como uno de los hijos de la `branch` . Una vez que tengamos la instancia de `Node` en `branch` , podemos modificar la `leaf` para darle una referencia `Weak<Node>` a su padre. Usamos el método `borrow_mut` en `RefCell<Weak<Node>>` en el campo `parent` de `leaf` , y luego usamos la función `Rc::downgrade` para crear una referencia `Weak<Node>` a `branch` desde `Rc<Node>` en `branch.`

Cuando imprimimos el padre de la `leaf` nuevamente, esta vez obtendremos una `branch` retención de `Some` variante: ¡ahora la `leaf` puede acceder a su padre! Cuando imprimimos `leaf` , también evitamos el ciclo que finalmente terminó en un desbordamiento de pila como lo hicimos en el Listado 15-26; las referencias `Weak<Node>` se imprimen como `(Weak)` :

```text
leaf parent = Some(Node { value: 5, parent: RefCell { value: (Weak) },
children: RefCell { value: [Node { value: 3, parent: RefCell { value: (Weak) },
children: RefCell { value: [] } }] } })
```

La falta de salida infinita indica que este código no creó un ciclo de referencia. También podemos decir esto al observar los valores que obtenemos al llamar a `Rc::strong_count` y `Rc::weak_count` .

#### Visualizar cambios en `strong_count` y `weak_count`

Veamos cómo `weak_count` valores `strong_count` y `weak_count` de las instancias `Rc<Node>` creando un nuevo ámbito interno y moviendo la creación de la `branch` a ese ámbito. Al hacerlo, podemos ver qué sucede cuando se crea una `branch` y luego se descarta cuando queda fuera de alcance. Las modificaciones se muestran en el Listado 15-29:

<span class="filename">Nombre de archivo: src / main.rs</span>

```rust
# use std::rc::{Rc, Weak};
# use std::cell::RefCell;
#
# #[derive(Debug)]
# struct Node {
#     value: i32,
#     parent: RefCell<Weak<Node>>,
#     children: RefCell<Vec<Rc<Node>>>,
# }
#
fn main() {
    let leaf = Rc::new(Node {
        value: 3,
        parent: RefCell::new(Weak::new()),
        children: RefCell::new(vec![]),
    });

    println!(
        "leaf strong = {}, weak = {}",
        Rc::strong_count(&leaf),
        Rc::weak_count(&leaf),
    );

    {
        let branch = Rc::new(Node {
            value: 5,
            parent: RefCell::new(Weak::new()),
            children: RefCell::new(vec![Rc::clone(&leaf)]),
        });

        *leaf.parent.borrow_mut() = Rc::downgrade(&branch);

        println!(
            "branch strong = {}, weak = {}",
            Rc::strong_count(&branch),
            Rc::weak_count(&branch),
        );

        println!(
            "leaf strong = {}, weak = {}",
            Rc::strong_count(&leaf),
            Rc::weak_count(&leaf),
        );
    }

    println!("leaf parent = {:?}", leaf.parent.borrow().upgrade());
    println!(
        "leaf strong = {}, weak = {}",
        Rc::strong_count(&leaf),
        Rc::weak_count(&leaf),
    );
}
```

<span class="caption">Listado 15-29: Crear una <code>branch</code> en un ámbito interno y examinar recuentos de referencias fuertes y débiles</span>

Después de crear la `leaf` , su `Rc<Node>` tiene una cuenta fuerte de 1 y una cuenta débil de 0. En el ámbito interno, creamos una `branch` y la asociamos con la `leaf` , en cuyo punto cuando imprimimos las cuentas, el `Rc<Node>` en `branch` tendrá un recuento fuerte de 1 y un recuento débil de 1 (para `leaf.parent` apuntando a la `branch` con un `Weak<Node>` ). Cuando imprimimos los recuentos en `leaf` , veremos que tendrá un recuento fuerte de 2, porque la `branch` ahora tiene un clon del `Rc<Node>` de `leaf` almacenado en `branch.children` , pero aún tendrá un recuento débil de 0 .

Cuando finaliza el alcance interno, la `branch` queda fuera de alcance y el recuento fuerte de `Rc<Node>` disminuye a 0, por lo que su `Node` se descarta. El recuento débil de 1 de `leaf.parent` no tiene relación con si se cae o no el `Node` , por lo que no tenemos pérdidas de memoria.

Si intentamos acceder al padre de `leaf` después del final del alcance, obtendremos `None` nuevamente. Al final del programa, el `Rc<Node>` en la `leaf` tiene un recuento fuerte de 1 y un recuento débil de 0, porque la `leaf` variable ahora es la única referencia al `Rc<Node>` nuevamente.

Toda la lógica que gestiona los recuentos y la caída de valor está integrada en `Rc<T>` y `Weak<T>` y sus implementaciones del rasgo `Drop` . Al especificar que la relación de un hijo a su padre debe ser una referencia `Weak<T>` en la definición de `Node` , puede hacer que los nodos padres apunten a los nodos hijos y viceversa sin crear un ciclo de referencia y pérdidas de memoria.

## Resumen

Este capítulo cubrió cómo usar punteros inteligentes para hacer diferentes garantías y compensaciones de aquellas que Rust hace por defecto con referencias regulares. El tipo `Box<T>` tiene un tamaño conocido y apunta a los datos asignados en el montón. El tipo `Rc<T>` realiza un seguimiento del número de referencias a datos en el montón para que los datos puedan tener múltiples propietarios. El tipo `RefCell<T>` con su mutabilidad interior nos da un tipo que podemos usar cuando necesitamos un tipo inmutable pero necesitamos cambiar un valor interno de ese tipo; también aplica las reglas de préstamo en tiempo de ejecución en lugar de en tiempo de compilación.

También se discutieron los rasgos `Deref` y `Drop` , que permiten mucha de la funcionalidad de los punteros inteligentes. Exploramos los ciclos de referencia que pueden causar pérdidas de memoria y cómo prevenirlos usando `Weak<T>` .

Si este capítulo ha despertado su interés y desea implementar sus propios punteros inteligentes, consulte ["The Rustonomicon"](https://doc.rust-lang.org/stable/nomicon/) para obtener más información útil.

A continuación, hablaremos sobre la concurrencia en Rust. Incluso aprenderá sobre algunos nuevos punteros inteligentes.
