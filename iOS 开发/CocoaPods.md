

#### CocoaPods 

##### 简介

CocoaPods是一个用Ruby写的、负责管理iOS项目中第三方开源库的工具，CocoaPods能让我们集中的、统一管理第三方开源库，为我们节省设置和更新第三方开源库的时间。

参考链接 “https://www.jianshu.com/p/f43b5964f582”

##### 安装

```sh
sudo gem install -n /usr/local/bin cocoapods
pod setup
# 国内镜像
git clone https://mirrors.tuna.tsinghua.edu.cn/git/CocoaPods/Specs.git  ~/.cocoapods/repos/trunk
```



##### 使用

1. 新建一个 Xcode 工程，进入到工程目录下面
2. `pod init`
3. `vim Podfile`
4. 添加需要的第三方库

5. `pod install`



```
# Uncomment the next line to define a global platform for your project
# platform :ios, '9.0'

target 'LearnRealm' do
  # Comment the next line if you don't want to use dynamic frameworks
  use_frameworks!
	
  # Pods for LearnRealm
  # 这里就是需要的第三方库
  pod 'RealmSwift'
end
```

