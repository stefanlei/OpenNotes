#### From 和 Into

[`From`](https://doc.rust-lang.org/std/convert/trait.From.html) 和 [`Into`](https://doc.rust-lang.org/std/convert/trait.Into.html) 两个 trait 是内部相关联的，实际上这是它们实现的一部分。如果我们能够从类型 B 得到类型 A，那么很容易相信我们也能把类型 B 转换为类型 A

---

#### From

[`From`](https://doc.rust-lang.org/std/convert/trait.From.html) trait 允许一种类型定义 “怎么根据另一种类型生成自己”，因此它提供了一种类型转换的简单机制。在标准库中有无数 `From` 的实现，规定原生类型及其他常见类型的转换功能。

```rust
// 例如
fn main() {
    let name = String::from("stefanlei");
}
```

也可以为我们自己的类型定义 From `trait`

```rust
#[derive(Debug)]
struct Number {
    value: i32,
}

impl From<i32> for Number {
    fn from(item: i32) -> Self {
        Number {
            value: item
        }
    }
}

fn main() {
    let a = Number::from(23);
    println!("{:?}", a);
}
```

---

#### Into

`Into` trait 可以把其他类型转换为定义的类型，但是需要在定义的时候，显式的指定类型以及实现了 `From` trait

```rust
#[derive(Debug)]
struct Number {
    value: i32,
}

impl From<i32> for Number {
    fn from(item: i32) -> Self {
        Number {
            value: item
        }
    }
}

fn main() {
    let a = Number::from(23);
  
  	// 这里就是使用的 into
    // 需要显式指定 Number 类型
    let b: Number = 12i32.into();

    println!("{:?}", a);
    println!("{:?}", b);
}
```

