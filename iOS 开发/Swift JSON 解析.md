#### Swift JSON 解析

```swift
// 首先需要一个 json 字符串
var jsonString = """
{
  "id": 1,
  "name": "Instagram Firebase",
  "link": "https://www.letsbuildthatapp.com/course/instagram-firebase",
  "imageUrl": "https://letsbuildthatapp-videos.s3-us-west-2.amazonaws.com/04782e30-d72a-4917-9d7a-c862226e0a93",
  "number_of_lessons": 49
}
"""

// 需要一个对应数据结构的 class 或者 struct ,需要继承 Codable 协议
struct Course: Codable {
    let id: Int   
    let name: String
    let link: String
    let imageUrl: String?   // 如果确定这个字段一定有，那就不需要加可选。
}


let jsonData = jsonString.data(using: .utf8)  // 把字符串转换为 Data 对象

let decoder = JSONDecoder() // 创建一个 JSON 解码对象

let course = try! decoder.decode(Course.self, from: jsonData!) // 解析

print(course)
print(course.name) // 然后就可以和对象一样使用了
print(course.link)




```

> 嵌套 JSON

```swift
var jsonString = """
{
  "code": 200,
  "message": "成功!",
  "result": [
    {
      "sid": "31436204",
      "text": "前面的车都能过"
    },
    {
      "sid": "31431508",
      "text": "时代在进步，少年爱少妇"
    }
  ]
}
"""

class NewsItem: Codable {
    var sid: String
    var text: String
}

class News: Codable {
    var code: Int
    var message: String?
    var result:[NewsItem]
}

let jsonData = jsonString.data(using: .utf8)


do {
    let news = try JSONDecoder().decode(News.self, from: jsonData!)
    print(news.message!)
    print(news.result[0].text)
}catch{
    print(error.localizedDescription)
    print(error)
}

```

