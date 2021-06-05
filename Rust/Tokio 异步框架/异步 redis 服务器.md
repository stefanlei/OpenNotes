#### 异步 redis 服务器

```rust
// main.rs
// 异步并发模式

use tokio::net::{TcpStream, TcpListener};
use mini_redis::{Connection, Frame, Command};
use mini_redis::Command::{Get, Set};
use std::collections::HashMap;
use std::sync::{Arc, Mutex};
use bytes::Bytes;
use mini_redis::frame::Frame::Null;

// 类型别名
type Db = Arc<Mutex<HashMap<String, Bytes>>>;

#[tokio::main]
async fn main() {
    let mut listener = TcpListener::bind("127.0.0.1:6379").await.unwrap();
		
  
  	// 共享的数据
    let db = Arc::new(Mutex::new(HashMap::new()));


    loop {
        let (socket, addr) = listener.accept().await.unwrap();
        println!("Connection from {}", addr.ip());

        let db = db.clone();
				// 每次都用一个新的任务来处理，所有任务共享 db
        tokio::spawn(async move {
            process(socket, db).await;
        });
    }
}


async fn process(socket: TcpStream, db: Db) {

    // 通过 mini-redis 提供 connection , 用来处理解析 socket中的帧
    let mut connection = Connection::new(socket);

    while let Some(frame) = connection.read_frame().await.unwrap() {
        let response = match Command::from_frame(frame).unwrap() {
            Set(cmd) => {
                let mut db = db.lock().unwrap();
                db.insert(cmd.key().to_string(), cmd.value().clone());
                Frame::Simple("OK".to_string())
            }
            Get(cmd) => {
                let db = db.lock().unwrap();
                if let Some(value) = db.get(cmd.key()) {
                    Frame::Bulk(value.clone())
                } else {
                    Frame::Null
                }
            }
            cmd => Null
        };

        // 写回响应到客户端
        connection.write_frame(&response).await.unwrap();
    }
}



```

