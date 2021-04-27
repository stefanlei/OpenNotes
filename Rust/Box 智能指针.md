#### Box 智能指针（指向堆上数据，并且可以确定大小)

**最简单直接的智能指针是 *box*，其类型是 `Box<T>`。 box 允许你将一个值放在堆上而不是栈上。留在栈上的则是指向堆数据的指针**



##### Box 创建递归类型

```rust
// 这种是错误的，因为编译器不知道 List 占用多少空间
enum List {
    Cons(i32, List),
    Nil,
}

// 这样是可以的，因为 Box 指针的大小是确定的
enum List{
		Cons(i32,Box<List>),
		Nil
}

fn main(){
  let list = Cons(1, Box::new(Cons(2, Box::new(Cons(3, Box::new(Nil))))));
}

```

