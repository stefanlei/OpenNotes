#### trait 特征

**特征是一组类型可实现的通用接口，类似其他语言中的接口的意思**

---

##### 定义 trait

```rust
// 定义了一个特征
trait Area {
    fn area(&self) -> f64;  // 可以只有函数定义
    fn test(&self) {        // 也可以有函数内容
        println!("test");
    }
}
```

---

##### 使用 trait

```rust
trait Area {
    fn area(&self) -> f64;
    fn test(&self) {
        println!("test");
    }
}

struct Circle {
    radius: f64,
}

impl Area for Circle {
    fn area(&self) -> f64 {
        self.radius * self.radius
    }
}

fn main() {
    let c = Circle { radius: 32.2 };

    println!("{}", c.area());

    c.test();
}
```

---

##### 使用系统内置 trait

如果要使用内置的 `trait` 可以使用 `derive`。这样就不需要自己重写对应的方法了

```rust
#[derive(Debug, PartialEq)]
struct Point {
    x: i32,
    y: i32,
}
```

```rust
// 也可以重写方法来实现
// 这样就是自己给 Point 实现 PartialEq 特征。
impl PartialEq for Point{
    fn eq(&self, other: &Self) -> bool {
        todo!()
    }
}
```

---

##### 实现 Display 特征

```rust
use std::fmt::{Display, Formatter, Result};


#[derive(Debug, PartialEq)]
struct Point {
    x: i32,
    y: i32,
}

impl Display for Point {
    fn fmt(&self, f: &mut Formatter<'_>) -> Result {
        write!(f, "({},{})", self.x, self.y)
    }
}
```

---



##### 使用特征边界 (另一种方法是使用泛型来限制参数类型)

**就是使用 trait 来限制参数类型**

```rust
use rand::seq::index::IndexVec::U32;

fn main() {
    let u = User { name: String::from("stefan"), age: 13 };

    send_data_as_json(&u);
}

trait AsJson {
    fn as_json(&self) -> String;
}

struct User {
    name: String,
    age: u8,
}

impl AsJson for User {
    fn as_json(&self) -> String {
        String::from("{'name':'stefan123'}")
    }
}

// 这里就是使用了 trait 来限制参数类型。
fn send_data_as_json(value: &impl AsJson) {
    println!("-> {}", value.as_json());
}
```

