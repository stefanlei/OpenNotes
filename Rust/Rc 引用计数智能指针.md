#### Rc 引用计数智能指针

为了启用多所有权，Rust 有一个叫做 `Rc<T>` 的类型。其名称为 **引用计数**（*reference counting*）的缩写。引用计数意味着记录一个值引用的数量来知晓这个值是否仍在被使用。如果某个值有零个引用，就代表没有任何有效引用并可以被清理。

```rust
#![allow(warnings, unused)]

use crate::List::{Cons, Nil};
use std::rc::Rc;

#[derive(Debug)]
struct CustomSmartPointer {
    data: String,
}

enum List {
    Cons(i32, Rc<List>),
    Nil,
}


fn main() {
    let a = Rc::new(Cons(5, Rc::new(Cons(10, Rc::new(Nil)))));
    let b = Cons(3, Rc::clone(&a));
    let c = Cons(4, Rc::clone(&a));
}
```

