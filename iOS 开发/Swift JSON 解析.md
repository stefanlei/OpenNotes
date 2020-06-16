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
    let imageUrl: String
}


let jsonData = jsonString.data(using: .utf8)  // 把字符串转换为 Data 对象

let decoder = JSONDecoder() // 创建一个 JSON 解码对象

let course = try! decoder.decode(Course.self, from: jsonData!) // 解析

print(course)
print(course.name) // 然后就可以和对象一样使用了
print(course.link)




```

