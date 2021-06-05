#### 基于 tokio 的异步

---

**redis 服务器**

##### 单个任务（当某个任务被阻塞，那么整个系统都会阻塞）

```rust
use tokio::net::{TcpStream, TcpListener};
use mini_redis::{Connection, Frame};
use std::thread::sleep;
use std::time::Duration;

#[tokio::main]
async fn main() {
    let mut listener = TcpListener::bind("127.0.0.1:6379").await.unwrap();

    loop {
        let (socket, addr) = listener.accept().await.unwrap();
        println!("Connection from {}", addr.ip());
        process(socket).await;
    }
}

async fn process(socket: TcpStream) {
    let mut connection = Connection::new(socket);

    if let Some(frame) = connection.read_frame().await.unwrap() {
        println!("recv: {:?}", frame);

        // 返回一个错误
        let response = Frame::Error("unimplemented".to_string());

        // 这里模拟耗时任务，这里会让整个系统阻塞
        sleep(Duration::from_secs(60));

        connection.write_frame(&response).await.unwrap();
    }
}
```

##### 多个任务并发

```rust
use tokio::net::{TcpStream, TcpListener};
use mini_redis::{Connection, Frame};
use std::thread::sleep;
use std::time::Duration;

#[tokio::main]
async fn main() {
    let mut listener = TcpListener::bind("127.0.0.1:6379").await.unwrap();

    loop {
        let (socket, addr) = listener.accept().await.unwrap();
        println!("Connection from {}", addr.ip());
				
      	// 为每一个入站 socket 链接产生一个新任务. 此socket链接被移动到这个新任务中且在里面处理.
        tokio::spawn(async move {
            process(socket).await;
        });
    }
}

async fn process(socket: TcpStream) {
    println!("process");
    let mut connection = Connection::new(socket);

    if let Some(frame) = connection.read_frame().await.unwrap() {
        println!("recv: {:?}", frame);

        // 返回一个错误
        let response = Frame::Error("unimplemented".to_string());

        // 这里模拟耗时任务
        sleep(Duration::from_secs(60));

        connection.write_frame(&response).await.unwrap();
    }
}
```

