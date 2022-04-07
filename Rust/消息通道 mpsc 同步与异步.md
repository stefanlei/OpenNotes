#### mpsc 同步与异步

##### 异步通道

默认就是 异步通道，就是发送端可以一直发送，不受接收端的影响。

之前我们使用的都是异步通道：**发送消息是非阻塞的，无论接收者是否正在接收消息，消息发送者在发送消息时都不会阻塞**

```rust
use std::sync::mpsc;
use std::thread;
use std::time::Duration;
fn main() {
    let (tx, rx)= mpsc::channel();

    let handle = thread::spawn(move || {
        println!("发送之前");
        tx.send(1).unwrap();
        println!("发送之后");
    });

    println!("睡眠之前");
    thread::sleep(Duration::from_secs(3));
    println!("睡眠之后");

    println!("receive {}", rx.recv().unwrap());
    handle.join().unwrap();
}

```

从输出还可以看出，`发送之前`和`发送之后`是连续输出的，没有受到接收端主线程的任何影响，因此通过`mpsc::channel`创建的通道是异步通道。

---

##### 同步通道

与异步通道相反，同步通道 **发送消息是阻塞的，只有在消息被接收后才解除阻塞**

```rust
use std::sync::mpsc;
use std::thread;
use std::time::Duration;
fn main() {
  	// 这里面的 mpsc::sync_channel(); 就是只同步通道，其中的 0 代表，在发送多少条消息后，再阻塞。
		// 该值可以用来指定同步通道的消息缓存条数，当你设定为N时，发送者就可以无阻塞的往通道中发送N条消息。
    // 当消息缓冲队列满了后，新的消息发送将被阻塞
    let (tx, rx)= mpsc::sync_channel(0);

    let handle = thread::spawn(move || {
        println!("发送之前");
        tx.send(1).unwrap();
        println!("发送之后");
    });

    println!("睡眠之前");
    thread::sleep(Duration::from_secs(3));
    println!("睡眠之后");

    println!("receive {}", rx.recv().unwrap());
    handle.join().unwrap();
}

```



