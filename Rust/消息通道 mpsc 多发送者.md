#### mpsc 多发送者

由于子线程会拿走发送者的所有权，因此我们必须对发送者进行克隆，然后让每一个线程拿走一份他的拷贝

```rust
#![allow(dead_code, unused_imports, unused)]

use std::sync::mpsc;
use std::thread;
use std::time::Duration;

fn main() {
    let (tx, rx) = mpsc::channel();
    let tx1 = tx.clone();

    thread::spawn(move || {
        tx.send(String::from("hi from raw tx"));
    });

    thread::spawn(move || {
        tx1.send(String::from("hi from cloned tx"));
    });

    for received in rx{
        println!("Got: {}", received);
    }
}
```

代码并无太大区别，就多了一个对发送者的克隆`let tx1 = tx.clone();`，然后一个子线程拿走`tx`的所有权，另一个子线程拿走`tx1`的所有权，皆大欢喜。

但是有几点需要注意:

- 需要所有的发送者都被`drop`掉后，接收者`rx`才会收到错误，进而跳出`for`循环，最终结束主线程
- 这里虽然用了`clone`但是并不会影响性能，因为它并不在热点代码路径中，仅仅会被执行一次
- 由于两个子线程谁先创建完成是未知的，因此哪条消息先发送也是未知的，最终主线程的输出顺序也不确定
