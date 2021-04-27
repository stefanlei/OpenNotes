##### 实现 Web 服务器

```rust
#![allow(warnings, unused)]

use std::net::TcpListener;
use std::net::TcpStream;
use std::io::{prelude, Read, Write};
use std::fs;

fn main() {
    let addr = "127.0.0.1:7878";
    let listen = TcpListener::bind(addr).unwrap();

    println!("Listen on {} ", addr);

    for stream in listen.incoming() {
        let stream = stream.unwrap();
        handle_connection(stream);
    }
}

fn handle_connection(mut stream: TcpStream) {
    let mut buffer = [0; 1024];
    stream.read(&mut buffer).unwrap();
		
  	// 访问首页
    let get = b"GET / HTTP/1.1\r\n";

    let (status_line, filename) = if buffer.starts_with(get) {
        ("HTTP/1.1 200 OK\r\n\r\n", "hello.html")
    } else {
        ("HTTP/1.1 404 NOT FOUND\r\n\r\n", "404.html")
    };

    let contents = fs::read_to_string(filename).unwrap();
    let response = format!("{}{}", status_line, contents);
    stream.write(response.as_bytes()).unwrap();
    stream.flush().unwrap();
}

```

