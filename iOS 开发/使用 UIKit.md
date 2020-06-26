#### 在 SwiftUI 中使用 UIKit

参考链接 https://juejin.im/post/5dbbdd836fb9a0208a3a7d71



在 SwiftUI 中使用 UIKit 需要用到`UIViewRepresentable`和`UIViewControllerRepresentable`两个协议，分别对应在 SwiftUI 中使用`UIView`和`UIViewController`。

```swift
// 使用 UIKit 中的 UITextView

import Foundation
import UIKit
import SwiftUI

// 这里使用 UIViewRepresentable
struct TextView: UIViewRepresentable {
    
    // 这个就接受来自外界的值
    @Binding var text: String
    
  	// 返回一个 UITextView 组件
    func makeUIView(context: Context) -> UITextView {
        print("makeUIView")
		
        let textView = UITextView()
      
      	// 设置代理 默认写法，一般不改
        textView.delegate = context.coordinator
        
        print("textView.delegate")
        return textView
    }
    
  	// 更新时的回调
    func updateUIView(_ uiView: UITextView, context: Context) {
        // uiView 更新时会会调用这里, 初始化时也会调用
        print("updateUIView")
        uiView.text = self.text
    }
    
  	// 返回一个代理人
    func makeCoordinator() -> Coordinator {
        print("makeCoordinator")
        return Coordinator(self)
    }
    
    
  	// 代理人需要继承 NSObject 以及 对应的 Delegate 
    class Coordinator: NSObject, UITextViewDelegate {
        var textView: TextView
        
        init(_ textView: TextView) {
            self.textView = textView
        }
        
        func textViewDidChange(_ textView: UITextView) {
            print("textViewDidChange")
            self.textView.text = textView.text
        }
    }
}

```



