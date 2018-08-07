#### 如何理解 REST，以及 RSETful 、RESTful API 

##### REST 是一种架构风格，REST 指的是一组架构约束条件和原则。满足这些约束条件和原则的应用程序或设计就是 RESTful。其核心是面向资源，REST 专门针对网络应用设计和开发方式，以降低开发的复杂性，提高系统的可伸缩性。RESTful API 是一套互联网应用程序的 API 设计理论，具体实现了 RESTful

> REST -- REpresentational State Transfer 
>
> 全称是 Resource Representational State Transfer：通俗来讲就是：`资源`在网络中以某种`表现形式`进行`状态转移`。

- Resource：资源，我们想要访问的数据。 www.baidu.com/index.html   URL/URI
- Representational：某种表现形式，比如用 JSON，XML，JPEG 等； 
- State Transfer：状态变化。通过 HTTP 动词实现、GET POST DELETE PUT HEAD



##### RESTful API 实现了 REST 架构

> 实用的是如何正确地理解 REST 架构和设计好 RESTful API。 

##### 首先为什么要使用 REST 架构呢？

大家都知道"古代"网页是前端后端融在一起的，比如之前的 PHP，JSP 等。在之前的桌面时代问题不大，但是近年来移动互联网的发展，各种类型的 Client 层出不穷，REST 可以通过一套统一的接口为 Web，iOS 和 Android 提供服务。另外对于广大平台来说，比如 Facebook platform，微博开放平台，微信公共平台等，它们不需要有显式的前端，只需要一套提供服务的接口，于是 RESTful 更是它们最好的选择。在 REST 架构下：

 ![](https://pic4.zhimg.com/80/06ee404783540f0af299042057738a99_hd.jpg)

 

 ##### Server 的 API 如何设计才满足 RESTful 要求?

- 未使用 RESTful 

1. 在 RESTful 之前的操作：

   http://127.0.0.1/user/query/1              GET  根据用户id查询用户数据
   http://127.0.0.1/user/save                    POST 新增用户
   http://127.0.0.1/user/update                POST 修改用户信息
   http://127.0.0.1/user/delete                 GET/POST 删除用户信息

- 使用了 RESTful 

1. Server提供的RESTful API中，URL中只使用名词来指定资源，原则上不使用动词。“资源”是REST架构或者说整个网络处理的核心。比如：

   [http://api.qc.com/v1/newsfeed](https://link.zhihu.com/?target=http%3A//api.qc.com/v1/newsfeed):     获取某人的新闻; 
   [http://api.qc.com/v1/friends](https://link.zhihu.com/?target=http%3A//api.qc.com/v1/friends):         获取某人的好友列表;
   [http://api.qc.com/v1/profile](https://link.zhihu.com/?target=http%3A//api.qc.com/v1/profile):          获取某人的详细信息;

2. 用 HTTP 协议里的动词来实现资源的添加，修改，删除等操作。即通过HTTP动词来实现资源的状态扭转：

   >    request.META.get('REQUEST_METHOD')      request.method

   GET          用来获取资源
   POST        用来新建资源（也可以用于更新资源），
   PUT          用来更新资源
   DELETE    用来删除资源。

   比如：
   DELETE    [http://api.qc.com/v1/](https://link.zhihu.com/?target=http%3A//api.qc.com/v1/friends)friends:                          删除某人的好友 （在http parameter 指定好友id）
   POST        [http://api.qc.com/v1/](https://link.zhihu.com/?target=http%3A//api.qc.com/v1/friends)friends:                          添加好友
   PUT          [http://api.qc.com/v1/profile](https://link.zhihu.com/?target=http%3A//api.qc.com/v1/profile):                           更新个人资料

```python
   from django.shortcuts import render, HttpResponse, Http404
   from django.db import transaction
   from django.views.decorators.http import require_http_methods
   # # Create your views here.
   #
   # def test404(request):
   #     raise Http404
   #     return 'ok'

   def adduser(request):
       pass

   def deleteuser(request):
       pass


   def user(request):
       if request.META.get('REQUEST_METHOD') == 'GET':
           pass
       elif request.META.get('REQUEST_METHOD') == 'DELETE':
           pass


   @require_http_methods(['GET'])
   def queryUser(request):
       # 这里执行 查询操作
       pass


   @require_http_methods(['DELETE'])
   def deleteUser(request):
       # 这里执行 删除操作
       pass


   @require_http_methods(['POST'])
   def addUser(request):
       # 这个执行 创建操作
       pass


   @require_http_methods(['PUT'])
   def updateUser(request):
       # 这里执行 更新操作
       pass

```

   禁止使用 GET http://api.qc.com/v1/deleteFriend     这个方式去执行 删除的操作

   - GET 提交有数据大小的限制，一般是不超过1024个字节，而这种说法也不完全准确，HTTP 协议并没有设定 URL 字节长度的上限，而是浏览器做了些处理，所以长度依据浏览器的不同有所不同；POST请求在HTTP协议中也没有做说明，一般来说是没有设置限制的，但是实际上浏览器也有默认。 head

   ![](https://img-blog.csdn.net/20170625151347639?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvY2hlbnhpYW9jaGFu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

3. Server 和 Client 之间传递某资源的一个表现形式，比如用 JSON XML传输文本，或者用 jpgWebP 传输图片等。 

4. 用 HTTP Status Code传递Server的状态信息。比如最常用的 200 表示成功，500 表示 Server内部错误等。 

   ![](https://img-blog.csdn.net/20170625152145836?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvY2hlbnhpYW9jaGFu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

5. 在返回结果用明确易懂的文本（String。注意返回的错误是要给人看的，避免用 1001 这种错误信息），而且适当地加入注释。

主要信息就这么点。最后是要解放思想，Web 端不再用之前典型的 PHP 或 JSP 架构，而是改为前段渲染和附带处理简单的商务逻辑（比如 AngularJS 或者 BackBone 的一些样例）。Web 端和Server 只使用上述定义的API来传递数据和改变数据状态。格式一般是 JSON。iOS 和 Android 同理可得。由此可见，Web，iOS，Android 和第三方开发者变为平等的角色通过一套API来共同消费 Server 提供的服务。
