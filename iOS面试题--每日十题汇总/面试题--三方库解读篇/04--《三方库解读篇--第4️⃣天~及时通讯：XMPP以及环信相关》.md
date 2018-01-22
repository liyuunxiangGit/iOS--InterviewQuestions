# 及时通讯：XMPP以及环信相关


----  

* Socket又称"套接字”
* 网络上的两端通过建立一个双向的通信连接实现数据的交换，这个端就称为一个Socket端。
* 应用程序通常通过"套接字"向网络发出请求或者应答网络请求
￼
![](https://github.com/liyuunxiangGit/iOS--InterviewQuestions/blob/master/imageFile/*%20Socket又称%22套接字”.png)
#### 是否使用过XMPP,XMPP的实现原理
* XMPP是一个即时通讯的协议，它规范了用于即时通信在网络上数据传输格式的，比如登录，获取好友列表等等的格式。XMPP在网络传输的数据是XML格式
* 比如登录：把用户名和密码放在xml的标签中，传输到服务器
* XMPP是一个基于个Socket通过的网络协议，目的是为了保存长连接，以实现即时通讯功能
* XMPP的客户端是使用一个XMPPFramework框架实现
* XMPP的服务器是使用Openfire,一个开源的服务器
* 客户端获取到服务器发送过来的好友消息，客户端需要对XML进行解析，使用的解析框架的KissXML框架，而不是NSXMLParser/GDataXML

￼![](https://github.com/liyuunxiangGit/iOS--InterviewQuestions/blob/master/imageFile/XMPP的实现原理1.png)
![](https://github.com/liyuunxiangGit/iOS--InterviewQuestions/blob/master/imageFile/XMPP的实现原理2.png)
￼

#### 在使用XMPP的时候有没有需要什么困难
* 发送附件（图片，语音，文档…）时比较麻烦
* XMPP框架没有提供发送附件的功能，需要自己实现
* 实现方法，把文件上传到文件服务器，上传成功后获取文件保存路径，再把附件的路径发送给好友
是否使用过环信，简单的说下环信的实现原理
* 环信是一个即时通信的服务提供商
* 环信使用的是XMPP协议，它是再XMPP的基础上进行二次开发，对服务器Openfire和客户端进行功能模型的添加和客户端SDK的封装，环信的本质还是使用XMPP，基本于Socket的网络通信
* 环信内部实现了数据缓存，会把聊天记录添加到数据库，把附件(如音频文件，图片文件)下载到本地，使程序员更多时间是花到用户体验上
* 环信内部已经实现了视频，音频，图片，其它附件发送功能
* 环信使用公司可以节约时间成本
* 不需要公司内部搭建服务器
* 客户端的开发，使用环信SDK比使用XMPPFramework更简洁方便


