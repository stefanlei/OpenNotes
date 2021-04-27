##### 普通引用

```rust
#![allow(warnings, unused)]


fn main() {
    let x = 5;
    let y = &x;
  
    assert_eq!(5, x);
    assert_eq!(5, *y); // 这个 * 就是解引用运算符,一旦解引用了 y，就可以访问 y 所指向的整型值并可以与 5 做比较。
}
```



##### 使用 Box<T>

```rust
#![allow(warnings, unused)]


fn main() {
    let x = 5;
    let y = Box::new(x); // 可以使用 Box<T> 代替引用来重写，解引用运算符也一样能工作
  
    assert_eq!(5, x);
    assert_eq!(5, *y);
}
```



##### 自定义智能指针

**从根本上说，`Box<T>` 被定义为包含一个元素的元组结构体，所以示例 15-8 以相同的方式定义了 `MyBox<T>` 类型。我们还定义了 `new` 函数来对应定义于 `Box<T>` 的 `new` 函数：**



为了实现 trait，需要提供 trait 所需的方法实现。`Deref` trait，由标准库提供，要求实现名为 `deref` 的方法，其借用 `self` 并返回一个内部数据的引用

```rust
#![allow(warnings, unused)]


use std::ops::Deref;

struct MyBox<T>(T);


impl<T> MyBox<T> {
    fn new(x: T) -> MyBox<T> {
        MyBox(x)
    }
}

impl<T> Deref for MyBox<T> {
    type Target = T;
		
  	// 这里就是返回第一个变量的引用
    fn deref(&self) -> &T {
        &self.0
    }
}


fn main() {
    let x = 5;
    let y = MyBox::new(x);
    assert_eq!(5, x);
    assert_eq!(5, *y); // 当我们在执行 * 的时候，实际上是执行 *(y.deref())
}
```



##### 函数和方法的隐式解引用强制多态

**解引用强制多态**（*deref coercions*）是 Rust 在函数或方法传参上的一种便利。其将实现了 `Deref` 的类型的引用，自动转换为函数形参所要求的类型的引用

```rust
fn hello(name: &str) {
    println!("hello {}", name);
}

fn main() {
  	
  	// MyBox 是上面的实现了 Deref trait 的自定义类型
    let m = MyBox::new(String::from("Rust")); 
 		// 这里可以直接传递一个 &MyBox 给 &str，因为内部会自动依次调用 deref 方法
    hello(&m);
}
```

---



##### 针对可变引用的 `DerefMut` trait

**DerefMut** 是可变引用的 trait ，和 **Deref** 类似

---

在以下三种情况下，Rust 会自动强制解引用多态

- 当 `T: Deref<Target=U>` 时从 `&T` 到 `&U`。
- 当 `T: DerefMut<Target=U>` 时从 `&mut T` 到 `&mut U`。
- 当 `T: Deref<Target=U>` 时从 `&mut T` 到 `&U`。