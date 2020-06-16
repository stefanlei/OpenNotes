#### Realm Swift 

##### 简介

[Realm](http://realm.io/) 是一个跨平台的移动数据库引擎，于 2014 年 7 月发布，准确来说，它是专门为移动应用所设计的数据持久化解决方案之一。

Realm 可以轻松地移植到您的项目当中，并且绝大部分常用的功能（比如说插入、查询等等）都可以用一行简单的代码轻松完成！

Realm 并不是对 Core Data 的简单封装，相反地， Realm 并不是基于 Core Data ，也不是基于 SQLite 所构建的。它拥有自己的数据库存储引擎，可以高效且快速地完成数据库的构建操作。



[参考链接]: https://realm.io/cn/docs/swift/latest/



##### 安装

> CocoaPods 安装方式

1. [安装 CocoaPods 1.1.0 或者更高版本](https://guides.cocoapods.org/using/getting-started.html)；
2. 执行 `pod repo update`，从而让 CocoaPods 更新至目前最新可用的 Realm 版本；
3. 在您的 Podfile 中，将 `use_frameworks!` 和 `pod 'RealmSwift'` 添加到主应用目标和测试目标中；
4. 在命令行中执行 `pod install`；
5. 使用由 CocoaPods 生成的 `.xcworkspace` 文件来编写工程。



##### 使用

> Use with Swift

```swift
// 数据类文件

import RealmSwift
// 继承 Object 是必须的，同时 @objc 和 dynamic 也是必须的
class User: Object{
    @objc dynamic var name: String = ""
    @objc dynamic var age: Int = 0
}

class User: Object{
    @objc dynamic var name: String?
    @objc dynamic var age: Int?
}
```

```swift
// 添加数据
import RealmSwift
do {
  let realm = try Realm() // 获取一个默认配置的 Realm 数据库
  
  var user = User()       // 给对象设置值， User 表
  user.name = "stefanlei"
  user.age = 18
   
  try realm.write({       // 写入数据
    realm.add(user) 
  })
  
}catch{
  print(error.localizedDescription)
}
```

```swift
// 查询数据
import RealmSwift
do {
  let realm = try Realm() // 获取一个默认配置的 Realm 数据库
  
 	let test1 = try realm.objects(User.self)   // 查询所以数据 User.self 是数据类
 	
  let test2 = try realm.objects(User.self).filter("name = 'jack'")  // 查询 name 为 jack 
  
  print(test1)
  print(test1.count)
  
}catch{
  print(error.localizedDescription)
}
```

