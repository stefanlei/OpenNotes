#### Vector 排序

**对整数进行排序**

```rust
fn main() {
    let mut vec = vec![1, 5, 10, 2, 15];
    println!("排序前 {:?}", vec);
    // 稳定的排序
    vec.sort();
    // 不稳定的排序，这种速度更快
    // vec.sort_unstable()
    println!("排序后 {:?}", vec);
}
```



**对浮点进行排序**

```rust
fn main() {
    let mut vec = vec![1.1, 1.15, 5.5, 1.123, 2.0];
    println!("  排序前： {:?}", vec);
    vec.sort_by(|a, b| a.partial_cmp(b).unwrap());
    println!("  排序后： {:?}", vec);
}
```



**对结构体进行排序**

```rust
#[derive(Debug, Eq, Ord, PartialEq, PartialOrd)]
struct Person {
    name: String,
    age: u32,
}

impl Person {
    pub fn new(name: &str, age: u32) -> Self {
        Person {
            name: name.to_string(),
            age,
        }
    }
}

fn main() {
    let mut people = vec![
        Person::new("stefan", 25),
        Person::new("Jack", 18),
        Person::new("Liu", 60)
    ];

    println!("排序前 {:?}", people);
    // 根据获得的自然顺序进行排序 （name 和 age) 对 people 进行排序
    people.sort();
    println!("排序后 {:?}", people);

    // 根据 age 进行排序
    people.sort_by(|a, b| b.age.cmp(&a.age));
    println!("排序后 {:?}", people);
}
```

