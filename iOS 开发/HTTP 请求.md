#### URLSession (原生)

1. 设置请求`url`
2. 设置`URLRequest`对象，配置请求相关信息
3. 创建会话配置`URLSessionConfiguration`
4. 创建会话`URLSession`
5. 创建任务和设置请求回调，并发起请求



```swift
let urlString = "http://v.juhe.cn/toutiao/index?type=top&key=3dc86b09a2ee2477a5baa80ee70fcdf5"

let url = URL(string: urlString) // 设置 url

var request  = URLRequest.init(url: url)  // 设置 request 对象

// 设置请求相关属性
request.httpMethod = HTTPMethod.post.rawValue
request.setValue("application/json", forHTTPHeaderField: "Content-Type")
let postData = ["username":"hibo","password":"123456"]
request.httpBody = try?JSONSerialization.data(withJSONObject: postData, options: [])
request.timeoutInterval = 30
request.cachePolicy = .useProtocolCachePolicy

// 发起请求
URLSession.shared.dataTask(with: request) { (data, response, error) in
    print("*******网络请求*******")
    do {
        let list =  try JSONSerialization.jsonObject(with: data!, options: .allowFragments)
        print(list)
    }catch{
        print(error)
    }
}.resume()

```

**原生的方式 非常麻烦**

---



#### Alamofire （第三方库)

