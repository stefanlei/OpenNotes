#### 通过 Drop 运行清理代码

```rust
#![allow(warnings, unused)]

#[derive(Debug)]
struct CustomSmartPointer {
    data: String,
}

impl Drop for CustomSmartPointer {
    fn drop(&mut self) {
        println!("Dropping CustomSmartPointer with data `{}`!", self.data);
    }
}

fn main() {
  	// 当 data 离开作用域的时候，会自动调用 drop 方法
    let data = CustomSmartPointer { data: String::from("hello") };
    println!("{:?}", data);
}
```





##### 通过 std::mem::drop 提早丢弃值

```rust
#![allow(warnings, unused)]

#[derive(Debug)]
struct CustomSmartPointer {
    data: String,
}

impl Drop for CustomSmartPointer {
    fn drop(&mut self) {
        println!("Dropping CustomSmartPointer with data `{}`!", self.data);
    }
}

fn main() {
    let data = CustomSmartPointer { data: String::from("hello") };
    println!("{:?}", data);
  	// 通过 drop 函数可以，提前析构
    drop(data);
    println!("finish");
}
```

