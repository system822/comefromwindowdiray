#### 什么是API(应用程序编程接口) ####

API（Application Programming Interface，[应用程序](https://baike.baidu.com/item/%E5%BA%94%E7%94%A8%E7%A8%8B%E5%BA%8F)接口）是一些预先定义的[函数](https://baike.baidu.com/item/%E5%87%BD%E6%95%B0)，或指软件系统不同组成部分衔接的约定。 [1]  目的是提供[应用程序](https://baike.baidu.com/item/%E5%BA%94%E7%94%A8%E7%A8%8B%E5%BA%8F)与开发人员基于某[软件](https://baike.baidu.com/item/%E8%BD%AF%E4%BB%B6)或硬件得以访问一组[例程](https://baike.baidu.com/item/%E4%BE%8B%E7%A8%8B)的能力，而又无需访问原码，或理解内部工作[机制](https://baike.baidu.com/item/%E6%9C%BA%E5%88%B6)的细节。



####  API的产生

研发人员A开发了软件A，研发人员B正在研发软件B。

有一天，研发人员B想要调用软件A的部分功能来用，但是他又不想从头看一遍软件A的源码和功能实现过程，怎么办呢？

研发人员A想了一个好主意：我把软件A里你需要的功能打包好；你把这个包放在软件B里，就能直接用我的方法了！

其中，API就是研发人员A说的那个方法。

![img](https://pic2.zhimg.com/80/v2-4160a3b3d7361a1d75fa0174f8e3e83e_hd.jpg)



应用接口:  很多情况下,需要把系统的功能作为服务暴露给外部的其他应用使用,就需要把系统中的服务作为API接口暴露出去,一般分为公共接口(发短信,天气服务)和私用接口(公司内部使用的);



#### Web 技术的发展阶段

Web 开发技术的发展可以粗略划分成以下几个阶段

静态内容阶段：在这个最初的阶段，使用 Web 的主要是一些研究机构。Web 由大量的静态 HTML 文档组成。
CGI 程序阶段：在这个阶段，Web 服务器增加了一些编程 API。通过这些 API 编写的应用程序，可以向客户端提供一些动态变化的内容。。
脚本语言阶段：在这个阶段，服务器端出现了 ASP、PHP、JSP、ColdFusion 等支持 session 的脚本语言技术，浏览器端出现了 Java Applet、JavaScript 等技术。使用这些技术，可以提供更加丰富的动态内容。
瘦客户端应用阶段：在这个阶段，在服务器端出现了独立于 Web 服务器的应用服务器。同时出现了 Web MVC 开发模式，各种 Web MVC 开发框架逐渐流行，并且占据了统治地位。基于这些框架开发的 Web 应用，通常都是瘦客户端应用，因为它们是在服务器端生成全部的动态内容。
RIA 应用阶段：在这个阶段，出现了多种 RIA（Rich Internet Application）技术，大幅改善了 Web 应用的用户体验。应用最为广泛的 RIA 技术是 DHTML+Ajax。Ajax 技术支持在不刷新页面的情况下动态更新页面中的局部内容。同时诞生了大量的 Web 前端 DHTML 开发库，例如 Prototype、Dojo、ExtJS、jQuery/jQuery UI 等等。
移动 Web 应用阶段：在这个阶段，出现了大量面向移动设备的 Web 应用开发技术。除了 Android、iOS、Windows Phone 等操作系统平台原生的开发技术之外，基于 HTML5 的开发技术也变得非常流行。



#### RESTful API 的产生

在互联网行业，实践总是走在理论的前面。Web 发展到了 1995 年，沿用了多年、主要面向静态文档的 HTTP/1.0 协议已经无法满足 Web 应用的开发需求，因此需要设计新版本的 HTTP 协议， HTTP/1.1 协议的第一个草稿是在 1996 年 1 月发布的，经过了三年多时间的修订，于 1999 年 6 月成为了 IETF 的正式规范。HTTP/1.1 协议设计的极为成功，以至于发布之后整整 10 年时间里，都没有多少人认为有修订的必要。在 HTTP/1.1 协议中， 有一种新的架构风格，它的名字就是“REST”——Representational State Transfer（表述性状态转换）的缩写。



**为什么有这套风格?**

客户端虽然有很多类型, 但是只要服务端统一提供API接口, 多个客户端基于相同的协议来调用该API接口即可获取数据

不同开发者对API接口的设计习惯不同 , 比如可能会出现这种情况

```
新增员工:
http://localhost/employee/save
http://localhost/employee/add
http://localhost/employee/new
http://localhost/employee/xinzeng
http://localhost/employee/append
http://localhost/employee?cmd=add

而且发送的请求方式以及响应结果也比较可能随意
```

因此 , RESTful 风格的API 就诞生了.



#### 基本概念

REST指的是一组架构约束条件和原则 , 满足这些约束条件和原则的应用程序或设计就是 RESTful 应用.



**Resource**

在REST中一切都被认为是资源 , 每种资源对应一个特定的URI

所谓"资源"，就是网络上的一个实体，或者说是网络上的一个具体信息。它可以是一段文本、一张图片、一首歌曲、一种服务，总之就是一个具体的实在。你可以用一个URI（统一资源定位符）指向它，每种资源对应一个特定的URI。要获取这个资源，访问它的URI就可以，因此URI就成了每一个资源的地址或独一无二的识别符。

http://www.wolfcode.cn/employees
http://www.wolfcode.cn/depts



**Representation**

"资源"是一种信息实体，它可以有多种外在表现形式。 这个表现的意思 ，就是"资源"具体呈现出来的形式。

比如，文本可以用txt格式表现，也可以用HTML格式、XML格式、JSON格式表现，甚至可以采用二进制格式；图片可以用JPG格式表现，也可以用PNG格式表现。

URI只代表资源的实体，不代表它的形式。它的具体表现形式，应该在HTTP请求的头信息中用Accept和Content-Type字段指定，这两个字段才是对"表现"的描述。

accept:application/json 
content-type:application/json 

Accept与Content-Type的区别
1.Accept属于请求头， Content-Type属于实体头。 
Http报头分为通用报头，请求报头，响应报头和实体报头。 
请求方的http报头结构：通用报头|请求报头|实体报头 
响应方的http报头结构：通用报头|响应报头|实体报头

2.Accept代表发送端（客户端）希望接受的数据类型。 
比如：Accept：application/json; 
代表客户端希望接受的数据类型是json类型,后台返回json数据

Content-Type代表发送端（客户端|服务器）发送的实体数据的数据类型。
比如：Content-Type：application/json; 
代表发送端发送的数据格式是json, 后台就要以这种格式来接收前端发过来的数据。



**State Transfer**

访问一个网站，就代表了客户端和服务器的一个互动过程。在这个过程中，势必涉及到资源和资源状态变化。

互联网通信协议HTTP协议，是一个无状态协议。这意味着，所有资源的状态都保存在服务器端。因此，如果客户端想要操作服务器，必须通过某种手段，让服务器端发生"资源状态转换"。

改变（服务器端）资源的状态

http://www.wolfcode.cn/employees
新增：员工资源 从无到有 状态的变化
更新：员工资源 从某个状态变成另外一种状态的转换
删除：员工资源 从有到无 状态的变化



**Uniform Interface**

REST要求，必须通过统一的接口来对资源执行各种操作。对于每个资源只能执行一组有限的操作。

HTTP1.1协议为例：
7个HTTP方法：GET/POST/PUT/DELETE/PATCH/HEAD/OPTIONS
HTTP头信息（可自定义）
HTTP响应状态代码（可自定义）
这些就是HTTP1.1协议提供的统一接口

以前
http://www.wolfcode.cn/employee/saveOrUpdate.do?id=1&name=xx

现在
http://www.wolfcode.cn/employees
新增：POST
更新：PUT
删除：DELETE
查询：GET



注意:

REST只是一种设计风格 , 而不是标准 , 只是提供了一组设计原则和约束条件

主要用于客户端和服务器交互, 让设计者和使用更方便 , 让API更简洁,  更有层次 , 更易于实现缓存等机制





#### RESTful设计

**1.资源设计**

在RESTful架构中，每个网址代表一种资源（resource），所以网址中不能有动词，只能有名词，而且所用的名词往往与数据库的表名对应。一般来说，数据库中的表都是同种记录的"集合"（collection），所以URI中的名词也应该使用复数。

https://api.example.com/v1/zoos：动物园资源
https://api.example.com/v1/animals：动物资源
https://api.example.com/v1/employees：饲养员资源

参考例子
https://api.github.com/
http://docs.jiguang.cn/jmessage/server/rest_api_im/
https://www.cnblogs.com/terrysun/archive/2012/12/11/2813039.html



**2.动作设计**

GET（SELECT）：从服务器取出资源（一项或多项）。
POST（CREATE）：在服务器新建一个资源。
PUT（UPDATE）：在服务器更新资源（客户端提供改变后的完整资源）。PUT更新整个对象 
PATCH（UPDATE）：在服务器更新资源（客户端提供改变的属性【补丁】）。 PATCH更新个别属性  
DELETE（DELETE）：从服务器删除资源。

HEAD：获得一个资源的元数据，比如一个资源的hash值或者最后修改日期； 
OPTIONS：获得客户端针对一个资源能够实施的操作；(获取该资源的api(能够对资源做什么操作的描述))



示例:

GET /zoos：列出所有动物园  zoo表
POST /zoos：新建一个动物园
GET /zoos/{id}：获取某个指定动物园的信息   /zoos/1 路径传参        /zoos/1
PUT /zoos/{id}：更新某个指定动物园的信息（提供该动物园的全部信息）
PATCH /zoos/{id}：更新某个指定动物园的信息（提供该动物园的部分信息）
DELETE /zoos/{id}：删除某个动物园 
GET /zoos/{id}/animals：列出某个指定动物园的所有动物 



**3.返回结果**

**返回值**

GET /collection：返回资源对象的列表（数组）
GET /collection/resource：返回单个资源对象
POST /collection：返回新生成的资源对象
PUT /collection/resource：返回完整的资源对象
PATCH /collection/resource：返回完整的资源对象
DELETE /collection/resource：返回一个空文档

**状态码**

200 OK - [GET]：服务器成功返回用户请求的数据。
201 CREATED - [POST/PUT/PATCH]：用户新建或修改数据成功。
202 Accepted - [*]：表示一个请求已经进入后台排队（异步任务）
204 NO CONTENT - [DELETE]：用户删除数据成功。
400 INVALID REQUEST - [POST/PUT/PATCH]：用户发出的请求有错误，服务器没有进行新建或修改数据的操作，该操作是幂等的。
401 Unauthorized - [*]：表示用户没有权限（令牌、用户名、密码错误）。
403 Forbidden - [*] 表示用户得到授权（与401错误相对），但是访问是被禁止的。
404 NOT FOUND - [*]：用户发出的请求针对的是不存在的记录，服务器没有进行操作，该操作是幂等的。
406 Not Acceptable - [GET]：用户请求的格式不可得（比如用户请求JSON格式，但是只有XML格式）。
410 Gone -[GET]：用户请求的资源被永久删除，且不会再得到的。
422 Unprocesable entity - [POST/PUT/PATCH] 当创建一个对象时，发生一个验证错误。
500 INTERNAL SERVER ERROR - [*]：服务器发生错误，用户将无法判断发出的请求是否成功。



#### RESTful开发框架

常见的有 SpringMVC , jersey , play



#### API接口测试工具

Postman, Insomnia



#### 使用SpringMVC开发RESTful接口

**需求：**
1.获取所有的员工



2.获取某个员工的信息



3.删除一个员工



4.获取某个员工某个月的薪资记录



5.更新员工数据



**相关注解:**

**@RestController**

**@PathVariable**

**@RequestMapping **

**@RequestBody**





#### 传统的开发模式与前后端分离模式对比



**传统的开发模式**

前端写好静态的html页面交给后端开发，后端把html改成模板，然后使用模板引擎去套模板，比如jsp，freemarker等
后端人员在开发过程中如果发现页面有问题，要返回给前端修改，前端再交给后端，直至功能实现。

问题：前后端严重耦合
1.前端需要改bug调试时，需要在当前电脑安装一整套后端的开发工具，启动后端程序。
2.还要求后端人员会html，js等前端语言。
3.前端页面也会嵌入很多后端的代码
4.一旦后端换了一套语言，前端也需要重新开发
5.沟通成本，调试成本，前后端开发进度相互影响，从而大大降低开发效率



**前后端分离**

前后端分离并不只是开发模式，也是web应用的一种架构模式。
在开发阶段，前后端人员约定好数据交互接口，即可并行开发与测试。

前端开发完成可以独自进行mock测试，后端也可以使用postman等接口测试工具进行测试。
最后可进行功能联调测试。

优点：
1.前后端责任清晰，后端专注于数据上，前端专注于视觉上。
2.无需等待对方的开发工作结束，提高开发效率。
3.可应对复杂多变的前端需求。
4.增强代码可维护性



![img](https://5b0988e595225.cdn.sohucs.com/images/20181024/9b1e1acfa78e488d924150aec2902ab3.jpeg)

