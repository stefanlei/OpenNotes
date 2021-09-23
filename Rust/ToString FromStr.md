#### ToString 

把其他类型转换为 `String`

---

要把任何类型转换成 `String`，只需要实现那个类型的 [`ToString`](https://doc.rust-lang.org/std/string/trait.ToString.html) trait。然而不要直接这么做，您应该实现[`fmt::Display`](https://doc.rust-lang.org/std/fmt/trait.Display.html) trait，它会自动提供 [`ToString`](https://doc.rust-lang.org/std/string/trait.ToString.html)，并且还可以用来打印类型，就像 [`print!`](https://rustwiki.org/zh-CN/rust-by-example/hello/print.html) 一节中讨论的那样。

```rust
struct Number {
    value: i32,
}

impl ToString for Number {
    fn to_string(&self) -> String {
        format!("{}", self.value)
    }
}

fn main() {
    let a = Number{ value:13 }
		// 这里需要调用 to_string
    println!("{}", a.to_string());
}
```

---

#### FromStr 

把 `&str` 转化为其他类型

我们经常需要把字符串转成数字。完成这项工作的标准手段是用 parse 函数。我们得 提供要转换到的类型，这可以通过不使用类型推断，或者用 “涡轮鱼” 语法（turbo fish，<>）实现。

只要对目标类型实现了 FromStr trait，就可以用 parse 把字符串转换成目标类型。 标准库中已经给无数种类型实现了 FromStr。如果要转换到用户定义类型，只要手动实现 FromStr 就行。

```rust

#[derive(Debug)]
struct Number {
    value: i32,
}


impl FromStr for Number {
    type Err = ();

    fn from_str(s: &str) -> Result<Self, Self::Err> {
        Ok(Number {
            value: s.parse().unwrap()
        })
    }
}

fn main() {
    let aaa: Number = "233".parse().unwrap();
    println!("{:?}", aaa);
}
```

