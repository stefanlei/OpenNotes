#### Rc 引用计数智能指针

- RC 可以让一个值有多个所有者，也就是调用 clone 产生一个指针指向该值。
- 通过不可变引用， `Rc<T>` 允许在程序的多个部分之间只读地共享数据。
- RC 只能获得不可变引用
- RC 不是原子计数，所以只能在单线程内使用，不能在多线程使用。

```rust
use std::rc::Rc;

fn main() {
    let five = Rc::new(5);
  	
  	// 这样 five 和 five1 都有对 5 的不可变引用
    let five1 = five.clone();
  
  	
    //Rc实现了Deref trait,可以自动解引用,因此下面打印成功
    println!("{}", five1);
    //可以通过调用strong_count查看引用计数
    println!("{}", Rc::strong_count(&five1));
}
```



---

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

