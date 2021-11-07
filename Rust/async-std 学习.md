#### async-std 学习

`async-std` 是实现了异步的一个库，实现了大多数 rust 标准库的异步版本，所以函数名称都一样。

```rust
use async_std::{fs::File, io, io::prelude::*, task};
use std::thread;
use std::time::Duration;

fn main() {
  	// spawn 使用了 Future，开启一个 Task 运行程序，返回 JoinHandle
  	// spawn 会在后台调用 Future 对象,
  	// async 关键字返回的就是 Future 对象
    let read_task = task::spawn(async {
        let path = "/Users/stefanlei/Desktop/test.csv";
        let result = read_file(&path).await;
        match result {
            Ok(t) => { println!("{}", t) }
            Err(e) => { println!("Error reading file: {:?}", e) }
        };
    });
  	
  	// task::block_on 和线程中 join 是同样的功能，也是一直阻塞，直到 read_task 结束
  	// task::block_on(read_task);
  	
  	// 睡眠3秒，防止主线程关闭.
    thread::sleep(Duration::from_secs(3));
}

async fn read_file(path: &str) -> io::Result<String> {
    let mut file = File::open(path).await?;
    let mut contents = String::new();
    file.read_to_string(&mut contents).await?;
    Ok(contents)
}
```

