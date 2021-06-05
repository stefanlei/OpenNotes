#### Arc 原子引用计数智能指针

Arc 实现了并发原语，所以可以替代 Rc 在多线程里面使用

```rust
#![allow(warnings, unused)]

use std::sync::Mutex;
use std::sync::Arc;
use std::thread;
use std::rc::Rc;

fn main() {
  
  	// 要配合锁使用 Mutex
    let counter = Arc::new(Mutex::new(0));

    let mut handles = vec![];

    for _ in 0..10 {
        let counter = Arc::clone(&counter);
        let handle = thread::spawn(move || {
            let mut num = counter.lock().unwrap();
            *num = *num + 1;
        });
        handles.push(handle);
    }

    for handle in handles {
        handle.join();
    }

    println!("Result: {}", *counter.lock().unwrap());
}
```

