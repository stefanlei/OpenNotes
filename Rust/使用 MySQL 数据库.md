#### 使用 MySQL 数据库

[参考链接](https://www.modb.pro/db/50058)

```toml
[dependencies]
mysql = "*" // 通配符*表示可以使用任何版本，通常会拉取最新版本
chrono = "0.4"
```

##### 支持多种方式查询

```rust
use mysql::*;
use mysql::prelude::*;
use chrono::prelude::*;

fn main() {
    let url = "mysql://root:leixianyang@localhost:3306/test";
    let pool = Pool::new(url).unwrap();
    let mut conn = pool.get_conn().unwrap();

    // 流式查询，其实结果数据是逐行读取的，也就是 for_each 的时候查询
    // 好处就是，整个数据永远不会存储在内存中，如果要读取大量数据，使用 query_iter.
    let sql = "select name,age,birthday from user";
    conn.query_iter(sql)
        .unwrap()
        .for_each(|row| {
            let r: (String, i32, NaiveDateTime) = from_row(row.unwrap());
            // println!("{},{},{}", r.0, r.1, r.2);
        });

    // 聚合结果查询,一次性把结果查询到 res 里面。
    let res: Vec<(String, i32, NaiveDateTime)> = conn.query(sql).unwrap();
    for line in res {
        // println!("{},{},{}", line.0, line.1, line.2);
    }


    // 查询结果到 struct
    struct User {
        name: String,
        age: i32,
        birthday: NaiveDateTime,
    }
    // 结果会回调到匿名函数里面 （name,age,birthday) 是入参，User 是返回类型。
    let res = conn.query_map(sql, |(name, age, birthday)| User {
        name,
        age,
        birthday,
    }).unwrap();

    for line in res {
        println!("{},{},{}", line.name, line.age, line.birthday);
    }
}


```

##### 单个数据查询

```rust

    // 单个查询
    let res: (String, i32, NaiveDateTime) = conn.query_first(sql).unwrap().unwrap();
    println!("{},{},{}", res.0, res.1, res.2);
		

		// 单个查询
    let res = conn.query_first(sql).map(|row| {
        row.map(|(name, age, birthday)| User {
            name,
            age,
            birthday,
        })
    });
    match res.unwrap() {
        Some(user) => {
            println!("{},{},{}", user.name, user.age, user.birthday);
        }
        _ => {}
    }
```

##### 参数化查询

```rust
 // 参数化查询
    let res = conn.exec_first("select name,age,birthday from user where name = :name", params! {
        "name"=>"Stefan"
    }).map(|row| {
        row.map(|(name, age, birthday)| User {
            name,
            age,
            birthday,
        })
    });

    match res.unwrap() {
        Some(user) => {
            println!("{},{},{}", user.name, user.age, user.birthday);
        }
        _ => {}
    }
```

