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



```swift
// 使用 ImagePicker 图片选择器

import Foundation
import SwiftUI
import UIKit



struct ImagePickerView: UIViewControllerRepresentable {
    
    
    var sourceType: UIImagePickerController.SourceType = .camera
    
    @Binding var image: UIImage?
    @Binding var isShown: Bool
    
    
    func makeUIViewController(context: Context) -> UIImagePickerController {
        let imagePicker = UIImagePickerController()
        imagePicker.sourceType = self.sourceType
        imagePicker.delegate = context.coordinator
        // 设置是否允许编辑
        imagePicker.allowsEditing = true
        return imagePicker
    }
    
    
    func makeCoordinator() -> Coordinator {
        return Coordinator(image: self.$image, isShown: self.$isShown)
    }
    
    // UI 更新时候的回调，基本不操作这个，都是操作代理
    func updateUIViewController(_ uiViewController: UIImagePickerController, context: Context) {
        print(11111)
    }
    
    class Coordinator: NSObject, UIImagePickerControllerDelegate, UINavigationControllerDelegate{
        
        @Binding var image: UIImage?
        @Binding var isShown: Bool
        
        init(image: Binding<UIImage?>, isShown: Binding<Bool>) {
            self._image = image
            self._isShown = isShown
        }
        
        
        // 选中图片的回调 需要把当前 sheet 关闭，所以 isShown 设置为 false
        func imagePickerController(_ picker: UIImagePickerController, didFinishPickingMediaWithInfo info: [UIImagePickerController.InfoKey : Any]) {
            
            // 因为启用了可以编辑所以这里使用了 editedImage
            if let uiImage = info[UIImagePickerController.InfoKey.editedImage] as? UIImage{
                self.image = uiImage
                self.isShown = false
            }
        }
        
        // 取消按钮的回调 需要把当前 sheet 关闭，所以 isShown 设置为 false
        func imagePickerControllerDidCancel(_ picker: UIImagePickerController) {
            self.isShown = false
        }
    }
}

```

