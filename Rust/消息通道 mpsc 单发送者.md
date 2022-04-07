#### 消息通道 mpsc

Rust 是在标准库里提供了消息通道(`channel`)，你可以将其想象成一场直播，多个主播联合起来在搞一场直播，最终内容通过通道传输给屏幕前的我们，其中主播被称之为**发送者**，观众被称之为**接收者**，显而易见的是：一个通道应该支持多个发送者和接收者。

标准库提供了通道`std::sync::mpsc`，其中`mpsc`是*multiple producer, single consumer*的缩写，代表了该通道支持多个发送者，但是只支持唯一的接收者。

---

##### 例子 

##### 阻塞方法

```rust
use std::sync::mpsc;
use std::thread;
use std::time::Duration;

fn main() {
    let (tx, rx) = mpsc::channel();

    thread::spawn(move || {
        tx.send(1);
    });

    // 这里会阻塞，rx 可以接收到 tx 发送的 1
    print!("receive {}", rx.recv().unwrap());
}
```

##### 非阻塞方法

```rust
use std::sync::mpsc;
use std::thread;

fn main() {
    let (tx, rx) = mpsc::channel();

    thread::spawn(move || {
        tx.send(1).unwrap();
    });
		
  	// 这里不会阻塞
  	// try_recv会尝试立即读取一次消息，因为消息没有发出，此次读取最终会报错，且主线程运行结束
  	// receive Err(Empty)
    println!("receive {:?}", rx.try_recv());
}


```

---

#### 使用 channel 需要遵循所有权机制

使用通道来传输数据，一样要遵循 Rust 的所有权规则：

- 若值的类型实现了`Copy`特征，则直接复制一份该值，然后传输过去，例如之前的`i32`类型
- 若值没有实现`Copy`，则它的所有权会被转移给接收端，在发送端继续使用该值将报错

##### 错误的案例

```rust
use std::sync::mpsc;
use std::thread;

fn main() {
    let (tx, rx) = mpsc::channel();

    thread::spawn(move || {
        let s = String::from("我，飞走咯!");
        tx.send(s).unwrap();
      	// 这里会报错，因为 s 的所有权已经传递给了接收端。
      	// 所以需要使用 tx.send(s.clone()) 才可以。
        println!("val is {}", s);  
    });

    let received = rx.recv().unwrap();
    println!("Got: {}", received);
}

```

---

#### 使用 for 进行循环接收

```rust
#![allow(dead_code, unused_imports, unused)]

use std::sync::mpsc;
use std::thread;
use std::time::Duration;

fn main() {
    let (tx, rx) = mpsc::channel();
    thread::spawn(move || {
        let vals = vec![
            String::from("hi"),
            String::from("from"),
            String::from("the"),
            String::from("thread"),
        ];

        for val in vals {
            tx.send(val);
        }
    });
		
  	// 循环阻塞、从 rx 迭代器中取数据
    for receive in rx {
        println!("receive {}", receive);
    }
}
```

在上面代码中，主线程和子线程是并发运行的，子线程在不停的**发送消息 -> 休眠 1 秒**，与此同时，主线程使用`for`循环**阻塞**的从`rx`**迭代器**中接收消息，当子线程运行完成时，发送者`tx`会随之被`drop`，此时`for`循环将被终止，最终`main`线程成功结束。

---

##### 传递多种数据类型

之前提到过，一个消息通道只能传输一种类型的数据，如果你想要传输多种类型的数据，可以为每个类型创建一个通道，你也可以使用枚举类型来实现

```rust
#![allow(dead_code, unused_imports, unused)]

use std::sync::mpsc;
use std::sync::mpsc::{RecvError, Sender};
use std::sync::mpsc::Receiver;
use std::thread;
use std::time::Duration;

enum Fruit {
    Apple(u8),
    Orange(String),
}

fn main() {
    let (tx, rx): (Sender<Fruit>, Receiver<Fruit>) = mpsc::channel();
    tx.send(Fruit::Orange("sweet".to_string())).unwrap();
    tx.send(Fruit::Apple(2)).unwrap();

    for _ in 0..2 {
        match rx.recv().unwrap() {
            Fruit::Apple(count) => {
                println!("receive {} apple", count);
            }
            Fruit::Orange(flavor) => {
                println!("receive {} oranges", flavor);
            }
        }
    }
}

```

如上所示，枚举类型还能让我们带上想要传输的数据，但是有一点需要注意，Rust 会按照枚举中占用内存最大的那个成员进行内存对齐，这意味着就算你传输的是枚举中占用内存最小的成员，它占用的内存依然和最大的成员相同, 因此会造成内存上的浪费。