***计算机网络基础***

    1.说一下HTTP 协议头字段说上来几个，是否尽可能详细的掌握HTTP协议。一次完整的HTTP事务是怎样的一个过程?
-  response Header 

    Cache-Control。
    Pragma。
    Connection。
    Date
    Transfer-Encoding
    Upgrade
    Via        
-   request header

    Accept
    Accept-Charset
    Accept-Encoding
    Accept-Language
    Authorization
    Host
    Referer
    User-Agent


- Http协议定义了很多与服务器交互的方法，最基本的有4种，分别是GET、POST、PUT、DELETE。一个URL地址用于描述一个网络上的资源，而HTTP中的GET、POST、PUT、 DELETE就对应着对这个资源的查、改、增、删4个操作，我们最常见的就是GET和POST了。GET一般用于获取/查询资源信息，而POST一般用于更新资源信息

- **过程**：
域名解析 --> 发起TCP的3次握手 --> 建立TCP连接后发起http请求 --> 服务器响应http请求，浏览器得到html代码 --> 浏览器解析html代码，并请求html代码中的资源（如js、css、图片等） --> 浏览器对页面进行渲染呈现给用户。



    2.cookies 是干嘛的，服务器和浏览器之间的 cookies 是怎么传的
    请描述一下cookies，sessionStorage和localStorage的区别
    
- cookies 是干嘛的
       
     >HTTP Cookie,通常直接叫做cookie,最初是在客户端用于储存会话信息。该标准要求服务器对任意HTTP请求发送Set-Cookie HTTP头作为响应的一部分，其中包含会话信息。
- 服务器和浏览器之间的 cookies 是怎么传的。
  
     >当用户第一次访问服务器时，服务器会在响应消息中增加Set-Cookie头字段，将用户信息以Cookie的形式发送给浏览器。一旦用户浏览器接受了服务器发送的Cookie信息，就会将它保存在浏览器的缓冲区中，这样，当浏览器后续访问该服务器时，都会在请求消息中将用户信息以Cookie的形式发送给Web服务器，从而使服务器端分辨出当前请求是由哪个用户发出的
-  请描述一下cookies，sessionStorage和localStorage的区别。
     - cookie是网站为了标示身份而储存在用户本地终端上的数(通常经过加密)。
     - cookie数据始终在同源的http请求中携带(即使不需要)，记会在浏览器和服务器间来回传递。
     - seessionStorage和localStorage不会自动吧数据发送给服务器。
  
    **储存大小**

        cookie数据大小不能超过4k。
        seessionStorage和localStorage 虽然也有储存大小的限制，但比cookie大得多，可以达到5M或更大。
    **有期时间**

        localStorage  储存持久数据，浏览器关闭后数据不丢失除非主动删除数据；
        seessionStorage  数据在挡墙浏览器窗口关闭后自动删除。
        cookie  设置的cookie过期时间之前一直有效，即使窗口或浏览器关闭。

    3)从敲入 URL 到渲染完成的整个过程，包括 DOM 构建的过程，说的约详细越好。
      > 1. 输入URL，浏览器根据域名寻找IP地址
      2. 浏览器发送一个HTTP请求给服务器，如果服务器返回以301之类的重定向，浏览器根据相应头中的location再次发送请求
       3. 服务器接受请求，处理请求生成html代码，返回给浏览器，这时的html页面代码可能是经过压缩的
       4. 浏览器接收服务器响应结果，如果有压缩则首先进行解压处理
       5. 浏览器开始显示HTML 
       6. 浏览器发送请求，以获取嵌入在HTML中的对象。在浏览器显示HTML时，它会注意到需要获取其他地址内容的标签。这时，浏览器会发送一个获取请求来重新获得这些文件——包括CSS/JS/图片等资源，这些资源的地址都要经历一个和HTML读取类似的过程。所以浏览器会在DNS中查找这些域名，发送请求，重定向等等

      **DOM 构建的过程**

       1. 解析HTML
       2. 构建DOM树
       3. DOM树与CSS样式进行附着构造呈现树（render树）
       4. 布局
       5. 绘制
    4)是否了解Web注入攻击，说下原理，最常见的两种攻击(XSS 和 CSRF)了解到什么程度。
      
     - SQL注入

        >把SQL命令插入到表单或输入URL查询字符串提交，欺骗服务器达到执行恶意的SQL目的
    
     - XSS(Cross Site Script)，跨站脚本攻击

        >攻击者在页面里插入恶意代码，当用户浏览该页之时，执行嵌入的恶意代码达到攻击目的

     - CSRF(Cross Site Request Forgery)，跨站点伪造请求

        >伪造合法请求，让用户在不知情的情况下以登录的身份访问，利用用户信任达到攻击目的

    **XSS原理及防范**

    >Xss(cross-site scripting)攻击指的是攻击者往Web页面里插入恶意html标签或者javascript代码。比如：攻击者在论坛中放一个看似安全的链接，骗取用户点击后，窃取cookie中的用户私密信息；或者攻击者在论坛中加一个恶意表单，当用户提交表单的时候，却把信息传送到攻击者的服务器中，而不是用户原本以为的信任站点

    **sql注入原理**

    >就是通过把SQL命令插入到Web表单递交或输入域名或页面请求的查询字符串，最终达到欺骗服务器执行恶意的SQL命令


    5)是否了解公钥加密和私钥加密。如何确保表单提交里的密码字段不被泄露。验证码是干嘛的，是为了解决什么安全问题。

    **了解公钥加密和私钥加密**
        
        一般情况下，私钥用于对数据进行签名
        一般情况下，公钥用于对签名经行验证
        HTTP网站在浏览器端使用公钥加密敏感数据，然后再服务器端使用私钥解密

    **如何确保表单提交里的密码字段不被泄露**

        使用post
    
    **验证码是干嘛的，是为了解决什么安全问题。**

        有效防止这种问题对某一个特定注册用户用特定程序暴力破解方式进行不断的登陆尝试.

    6)编码常识：文件编码、URL 编码、Unicode编码 什么含义。一个gbk编码的页面如何正确引

***开发工具***：


    1)是否了解开源的架构工具 bower、npm、yeoman、gulp、webpack，有无用过，有无写过，一个 npm 的包里的 package.json 具备的必要的字段都有哪些(名称、版本号，依赖)

    2)github常用不常用，关注过哪些项目

    3)会不会用 ps 扣图，png、jpg、gif 这些图片格式解释一下，分别什么时候用。如何优化图像、图像格式的区别

    4)说一下你常用的命令行工具

    5)会不会用git，说上来几个命令，说一下git和svn的区别，有没有用git解决过冲突
