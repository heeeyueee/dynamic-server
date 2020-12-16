## 动态服务器

使用json文件来模仿数据库，创建动态服务器，完成注册登录功能，学习浏览器和服务器通信的整个流程

### 区别

静态服务器和动态服务器的主要区别：是否访问了数据库

> 本例中，在db中使用users.json模仿数据库

### cookie

HTTP 是无状态协议，它不对之前发生过的请求和响应的状态进行管理。也就是说，无法根据之前的状态进行本次的请求处理。

假设要求登录认证的 Web 页面本身无法进行状态的管理（不记录已登录的状态），那么每次跳转新页面不是要再次登录，就是要在每次
请求报文中附加参数来管理登录状态。无状态协议当然也有它的优点，由于不必保存状态，可减少服务器的 CPU 及内存资源的消耗。

为了保留无状态协议这个特征的同时又能解决类似的矛盾问题，引入 Cookie 技术。Cookie 技术通过在请求和响应报文中写入Cookie 信息来控制客户端的状态。

<img src="https://raw.githubusercontent.com/heeeyueee/pic/main/img/20201216162625.png" style="zoom:60%;" />

<img src="https://raw.githubusercontent.com/heeeyueee/pic/main/img/20201216162640.png" style="zoom:60%;" />

使用node.js在响应头中设置让浏览器保存的cookie

```javascript
response.setHeader("Set-Cookie", `session_id=${random}; HttpOnly`);
```

* HttpOnly：设置了 HttpOnly 属性的 cookie 不能使用 JavaScript 经由  Document.cookie 属性、XMLHttpRequest 和  Request APIs 进行访问，以防范跨站脚本攻击（XSS）。简单来说，前端无法访问和控制cookie。

### session

会话（session）是一种持久网络协议，在用户（或用户代理）端和服务器端之间创建关联，从而起到交换数据包的作用机制，session在网络协议（例如telnet或FTP）中是非常重要的部分。

本例中，为了防止客户端恶意篡改用户的私密信息，登录成功后响应浏览器时 ，在cookie中设置和user_id对应的session，它是随机的字符串，让客户端进行保存，下次访问服务器时带上这个session，服务器就能比对知道是否是已经登录的用户,session是有时效性的可以删除，这样就保证了安全性。


