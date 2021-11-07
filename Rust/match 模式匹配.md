#### match 模式匹配

match 返回值

```rust
use std::{fs::File, io, io::prelude::*};
use std::io::Error;

fn main() {
    let path = "/Users/stefanlei/Desktop/test.csv";
    let res = read_file(&path);
  
    let mut text = String::new();
 		
  	// match 返回值
    text = match res {
        Ok(t) => t,  // 不需要花括号，直接返回
        _ => String::new()  // 使用 _=> 表示其他情况都这样做
    };

    println!("{}", text);
}


fn read_file(path: &str) -> io::Result<String> {
    let mut file = File::open(path).unwrap();
    let mut contents = String::new();
    file.read_to_string(&mut contents).unwrap();
    return Ok(contents);
}
```

---

match 中修改值

```rust
use std::{fs::File, io, io::prelude::*};
use std::io::Error;

fn main() {
    let path = "/Users/stefanlei/Desktop/test.csv";
    let res = read_file(&path);
    let mut text = String::new();
  	
  	// 在 match 中，原来的值改变，需要用花括号{}
    match res {
        Ok(t) => {
            text = t;
        }
        _ => {
            text = String::new()
        }
    };

    println!("{}", text);
}


fn read_file(path: &str) -> io::Result<String> {
    let mut file = File::open(path).unwrap();
    let mut contents = String::new();
    file.read_to_string(&mut contents).unwrap();
    return Ok(contents);
}
```



