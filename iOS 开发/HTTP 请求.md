#### URLSession (原生) 

1. 设置请求`url`
2. 设置`URLRequest`对象，配置请求相关信息
3. 创建会话配置`URLSessionConfiguration`
4. 创建会话`URLSession`
5. 创建任务和设置请求回调，并发起请求

**以下代码只能在 ios 中运行，无法在 swift 命令行程序中运行**

```swift
import SwiftUI

let urlString = "https://api.apiopen.top/getJoke?page=1&count=2&type=video"

struct ContentView: View {
    
    var body: some View {
        VStack{
      
            Button(action: {
                self.fetch()  // 点击按钮来触发请求
            }, label: {
                Text("请求")
            })
        }
    }
    
    func fetch(){
        guard let url = URL(string: urlString) else {
            return
        }
        
        let request = URLRequest(url: url)
        
        URLSession.shared.dataTask(with: request){ (data, response, error) in
            guard let data = data else {
                return
            }
            
            let news = try! JSONDecoder().decode(News.self, from: data)
            print(news)
            print(news.message)
            
        }.resume()  // resume 是必须的，否则不会发起网络请求
    }
}

struct ContentView_Previews: PreviewProvider {
    static var previews: some View {
        ContentView()
    }
}


struct NewsItem: Codable {
    var sid: String
    var text: String
    var type: String
}

struct News: Codable {
    var code: Int
    var message: String
    var result:[NewsItem]
}


```

---



#### Alamofire （第三方库)

