# Dubbo 

<img src="images/dubbo.png" />

## 课程目标

- 了解系统架构演变历史
- 了解微服务应用演变历史
- 掌握RPC调用服务器的原理
- 掌握Dubbo下的服务开发和治理



## 系统架构演变史

B/S软件架构的出现标志着web开发时代的到来，当初应用都是比较简单，功能单一，访问量少。这些因素就决定了当年的应用结构是比较简单的

<img src="images/20190614091933.png" />

随着家用电脑的普及，互联网的兴起，上网的人越来越多，逐渐显得简单的架构无法支撑应用程序的正常运行，软件科学家们就开始探讨如何去设计软件的架构，最初比较突出的两大问题在于1台服务器无法保证应用程序和数据库程序两个老虎的性能，科学家就提出把应用程序和数据库程序分别安装在2台服务器中，通过网络来进行数据传输，再通过应用程序中加本地缓存的方式来解决不常变动数据的查询性能问题，此时出现了最早期的分布式系统架构

<img src="images/20190614092149.png" />

伴随着大数据时代的到来，这种简单是分布式系统架构也开始扛不住了，服务器宕机，成了常见的事情，科学家又开始探索，如何提高系统的稳定性和可用性，于是提出了，应用服务集群和数据库服务器集群，通过负载均衡的方式来维持系统的高可用和稳定性，以应对这个时代的高并发

<img src="images/20190614094910.png" />

在某些业务中，关系型数据库明显不适合，如：点赞，评论，全文检索等。这些业务场景使用关系型数据库来完成会严重的影响数据库的性能，科学家们又拓展出非关系型数据库（NoSQL），用于项目中的某个特殊的业务场景，去弥补关系型数据的短板，此时这样的系统架构基本满足当前时代下的需求

<img src="images/20190614095116.png" />



## 微服务应用演变史

JavaEE应用三层架构，取得巨大成功后，在很长的一段时间内，都没有出现过问题，直到大数据时代的到来某些数据量巨大的公司开始遇到了新的挑战。某些业务方法频繁调用，需要配置大量的资源，某些业务方法很少调用，只需配置少量资源，如：商城应用中的服务

<img src="images/20190614095910.png" />

在商城的应用中，订单服务，积分服务，商品服务的使用频率远远大于其他服务，如果仅仅通过布置多台服务器的方式来缓解压力的造成的问题，此时会造成服务器成本的大量提升，并且资源得不到充分的利用，造成浪费，遇到某个关系型数据库不好处理的业务场景，在技术选择的时候也要充分去考虑系统稳定和冲突问题，于是科学家们又探索出基于服务的分布式应用架构，服务拆分越细，数量就越多，这种应用架构就是当下最热门的**微服务应用架构**。目前比较成熟，社区活跃的微服务框架有**Dubbo，SpringCloud**

<img src="images/20190614101336.png" />

- 生产者：提供业务逻辑实现的角色，对外提供服务
- 消费者：调用生产者提供的服务，使用其业务功能

如：订单服务器和商品服务分别对外提供了操作订单和商品的业务功能，此时它们都属于生产者，但是订单服务在查询订单的时候需要去获取当前订单有哪些商品，因此订单服务就要调用商品服务提供的功能，所以订单服务又是商品服务的消费者，表现层相关组件直接生产者提供的服务，因此它们都是消费者。服务间的调用都是使用RPC远程调用协议

### 微服务应用的优势

- 降低系统的耦合度
- 服务器之间都是相互独立，互补干扰，此时每个服务都能更好的选择符合业务场景的技术
- 充分使用服务器的硬件资源，避免造成不必要的浪费
- 降低维护成本和难度
- 提高应用系统的稳定性

### 分布式和微服务的区别

分布式服务顾名思义服务是分散部署在不同的机器上的，一个服务可能负责几个功能，是一种面向SOA架构的，服务之间也是通过rpc来交互或者是webservice来交互的。逻辑架构设计完后就该做物理架构设计，系统应用部署在超过一台服务器或虚拟机上，且各分开部署的部分彼此通过各种通讯协议交互信息，就可算作分布式部署，生产环境下的微服务肯定是分布式部署的，分布式部署的应用不一定是微服务架构的，比如集群部署，它是把相同应用复制到不同服务器上，但是逻辑功能上还是单体应用

简单来说微服务就是很小的服务，小到一个服务只对应一个单一的功能，只做一件事。这个服务可以单独部署运行，服务之间可以通过RPC来相互交互，每个微服务都是由独立的小团队开发，测试，部署，上线，负责它的整个生命周期。



## RPC调用原理

### 序列化和反序列化

>序列化：把Java对象转变成二进制数据
>
>反序列化：把二进制数据转变成Java对象
>
>注意：在反序列的过程中，需要知道该二进制数据还原成哪个Java对象，因此必须要还有序列化的ID号，在Java规范中，必须实现Serializable接口以开启序列化功能

```java
@Setter@Getter@ToString
@NoArgsConstructor
@AllArgsConstructor
public class User implements Serializable {
    private Long id;
    private String name;
}
```

```java
public class App {
    @Test
    public void test1() throws Exception {
        User u = new User(10L, "逍遥");
        
        ObjectOutputStream out = 
            new ObjectOutputStream(new FileOutputStream("f:/user.txt"));
        // 序列化
        out.writeObject(u);
        out.close();
    }

    @Test
    public void test2() throws Exception {
        User u = null;
        
        ObjectInputStream in = 
            new ObjectInputStream(new FileInputStream("f:/user.txt"));
        // 反序列化
        u = (User) in.readObject();
        in.close();

        System.out.println(u);
    }
}
```

### 流程图

<img src="images/20190614150029.png" />

RPC远程调用底层的核心技术就是序列化和反序列化

1. 生产者发布服务
2. 消费者按照RPC协议要求调用生产者发布的服务
3. 生产者执行方法得到返回的对象
4. 生产者把对象序列化后返回给消费者
5. 消费者接收到数据后对其反序列化，得到Java对象



## Dubbo的介绍

百度百科：<https://baike.baidu.com/item/Dubbo/18907815?fr=aladdin>

官网：<http://dubbo.apache.org/zh-cn/>

Dubbo是阿里巴巴公司开源的一个高性能优秀的服务框架，使得应用可通过高性能的 RPC 实现服务的输出和输入功能，可以和Spring框架无缝集成。
Dubbo是一款高性能、轻量级的开源Java RPC框架，它提供了三大核心能力：面向接口的远程方法调用，智能容错和负载均衡，以及服务自动注册和发现。

### 核心组件

- Remoting: 网络通信框架，实现了 sync-over-async 和 request-response 消息机制.
- RPC: 一个远程过程调用的抽象，支持负载均衡、容灾和集群功能
- Registry: 服务目录框架用于服务的注册和服务事件发布和订阅

### 工作原理

<img src="images/architecture.png" />

1. 生产者启动Dubbo容器，生产服务对象
2. 生产者把生成的服务发布到注册中心
3. 消费者向注册中心订阅服务，把发布的服务，下载到本地缓存，订阅服务后，本地缓存会自动更新生产者发布的服务
4. 当消费者需要调用服务时，按照RPC协议要求，向生产者发起服务的调用
5. 生产者把返回的对象交给Dubbo容器进行序列化处理后返回给消费者
6. 消费者接收到返回的数据后对其反序列化，得到Java对象
7. 监视器对服务性能做监控统计

注意：注册中心和监视器都不是必须的，可以缺少。如：缺少注册中心后消费者就不能自动更新生产者发布的服务信息，当生产者信息发生改变时，消费者很可能调用服务失败。比较出名的注册中心有zookeeper，eruka



## 项目拆分

### 项目结构

<img src="images/20190614173507.png" />

从以上图中我们不难看出，我们至少需要创建5个项目

- product-api：定义商品相关的业务方法
- member-api：定义会员相关的业务方法
- product-sever：商品服务的提供者，同时也是会员服务的消费者，底层会去调用会员服务的相关功能
- member-sever：会员服务的提供者，提供会员业务方法的实现
- website：商品服务的消费者，使用商品服务提供的业务功能

### zookeeper

zookeeper的功能非常多，是大数据技术领域的核心组件之一，但是我们这里用不到那么多的功能，仅仅只是作为注册中心来使用一下

注意：没有注册中心的情况下，Dubbo也是可以正常的运行的

解压zookeeper压缩包，执行bin/zkServer.cmd，看到以下界面表示启动成功，默认端口：2181

<img src="images/20190617154306.png"/>



## 商品服务

使用最底层的代码开发基于Dubbo的微服务应用

### product-api

#### 导入依赖

```
见资料中的pom.txt中的product-api
```

#### 代码

```java
package cn.wolfcode.dubbo.product.domain;

@NoArgsConstructor
@AllArgsConstructor
@Setter@Getter@ToString
public class Product implements Serializable {
    private Long id;
    private String name;
    //暂时不引入会员的相关信息
    //private User user;
}
```

```java
package cn.wolfcode.dubbo.product.service;

public interface IProductService {
    Product get(Long productId, Long userId);
}
```

### product-server

生产者项目，生产服务对外发布，由Dubbo框架来提供RPC规范的实现，项目中包含service的实现

#### 导入依赖

```
见资料中的pom.txt中的product-server
```

#### 代码

```java
package cn.wolfcode.dubbo.product.data;

//模拟商品的数据库
public abstract class ProductData {
    private static Map<Long, Product> datas = new HashMap<>();

    static {
        datas.put(1L, new Product(1L, "IPhone XR", null));
        datas.put(2L, new Product(2L, "华为P30", null));
        datas.put(3L, new Product(3L, "小米9", null));
    }

    public static Product get(Long id) {
        return datas.get(id);
    }
}
```

```java
package cn.wolfcode.dubbo.product.service.impl;

public class ProductServiceImpl implements IProductService {
   public Product get(Long productId, Long userId) {        
       Product product = ProductData.get(productId);
       return product;
   }
}
```

```java
public class ProductApp {
    public static void main(String[] args) throws Exception {
        //1:创建应用配置对象,设置服务名称
        ApplicationConfig applicationConfig = new ApplicationConfig("product-server");
        //2:创建协议配置对象,设置协议和端口
        ProtocolConfig protocolConfig = new ProtocolConfig("dubbo", 20880);
        //3:创建注册中心配置对象,设置注册中心地址
        RegistryConfig registryConfig = new RegistryConfig("zookeeper://127.0.0.1:2181");
        //4:创建服务对象,设置服务参数,发布的服务接口和真实服务对象
        ServiceConfig<IProductService> serviceConfig = new ServiceConfig<>();
        serviceConfig.setApplication(applicationConfig);
        serviceConfig.setProtocol(protocolConfig);
        serviceConfig.setRegistry(registryConfig);
        //发布服务接口和真实服务对象
        serviceConfig.setInterface(IProductService.class);
        serviceConfig.setRef(new ProductServiceImpl());
        //5:发布服务
        serviceConfig.export();

        //使用阻塞模拟服务器在运行
        System.in.read();
    }
}
```

### website

消费者调用发布的服务，获取服务对象的代理引用，底层通过RPC远程调用方式来获取到调用的结果

#### 导入依赖

```
见资料中的pom.txt中的website
```

#### 代码

```java
public class WebsiteApp {
    public static void main(String[] args) {
        //1:创建应用配置对象,设置名称
        ApplicationConfig applicationConfig = new ApplicationConfig("website");
        //2:创建注册中心配置对象,设置注册中心地址
        RegistryConfig registryConfig = new RegistryConfig("zookeeper://127.0.0.1:2181");
        //3:创建引用配置对象,设置引用参数
        ReferenceConfig<IProductService> referenceConfig = new ReferenceConfig<>();
        referenceConfig.setApplication(applicationConfig);
        referenceConfig.setRegistry(registryConfig);
        referenceConfig.setInterface(IProductService.class);
        /*如果没有注册中心则需要配置URL地址       
        referenceConfig.setUrl("服务地址");*/
        //4:引用Dubbo提供的动态代理对象
        IProductService productService = referenceConfig.get();
        //5:调用方法获取结果
        Product p = productService.get(1L, null);
        System.out.println(p);
    }
}
```



## SpringBoot集成Dubbo

以上项目在搭建环境时就已经按照SpringBoot方式导入了依赖，只需要做少量的修改就能完成

### product-server

```properties
#application.properties
#配置应用名称
dubbo.application.name=product-server
#配置注册中心地址
dubbo.registry.address=zookeeper://127.0.0.1:2181
#配置协议
dubbo.protocol.name=dubbo
#配置端口
dubbo.protocol.port=20880
#允许bean覆盖
spring.main.allow-bean-definition-overriding=true
```

```java
package cn.wolfcode.dubbo.product.service.impl;

@Service //发布服务对象，这里的@Service注解不是Spring提供的，而是由Dubbo框架提供的
public class ProductServiceImpl implements IProductService {
   public Product get(Long productId, Long userId) {
        Product product = ProductData.get(productId);
        return product;
    }
}
```

```java
package cn.wolfcode.dubbo.product;

@SpringBootApplication
@EnableDubbo //开启扫描Dubbo的Service注解
public class ProductProvider {
    public static void main(String[] args) {
        SpringApplication.run(WebsiteApp.class, args);
    }
}
```

### website

```properties
#配置应用名称
dubbo.application.name=webSite
#配置注册中心地址
dubbo.registry.address=zookeeper://127.0.0.1:2181
#允许bean覆盖
spring.main.allow-bean-definition-overriding=true
```

```java
package cn.wolfcode.dubbo.product.web.controller;

@RestController
@RequestMapping("/products")
public class ProductController {
    @Reference //引用Dubbo提供的动态代理对象
    private IProductService productService;
    
    @RequestMapping("/{productId}/{userId}")
    public Product get(@PathVariable Long productId, @PathVariable Long userId) {
        return productService.get(productId, userId);
    }
}
```

```java
package cn.wolfcode.dubbo.product;

@SpringBootApplication
public class Website {
    public static void main(String[] args) {
        SpringApplication.run(Website.class, args);
    }
}
```



## 加入会员服务

### member-api

#### 导入依赖

```
见资料中的pom.txt中的member-api
```

#### 代码

```java
package cn.wolfcode.dubbo.member.domain;

@NoArgsConstructor
@AllArgsConstructor
@Setter@Getter@ToString
public class User implements Serializable {
    private Long id;
    private String name;
}
```

```java
package cn.wolfcode.dubbo.member.service;

public interface IUserService {
    User get(Long id);
}
```

### member-server

#### 导入依赖

```
见资料中的pom.txt中的member-server
```

#### 代码

```properties
dubbo.application.name=product-server
dubbo.registry.address=zookeeper://127.0.0.1:2181
dubbo.protocol.name=dubbo
dubbo.protocol.port=20881

spring.main.allow-bean-definition-overriding=true
```

```java
package cn.wolfcode.dubbo.member.data;

// 模拟数据库
public abstract class UserData {
    private static Map<Long, User> datas = new HashMap<>();

    static {
        datas.put(1L, new User(1L, "逍遥"));
        datas.put(2L, new User(2L, "bunny"));
    }

    public static User get(Long id) {
        return datas.get(id);
    }
}
```

```java
package cn.wolfcode.dubbo.member.service.impl;

@Service //Dubbo提供的注解
public class UserServiceImpl implements IUserService {
    public User get(Long id) {
        return UserData.get(id);
    }
}
```

```java
package cn.wolfcode.dubbo.member;

@SpringBootApplication
@EnableDubbo //开启扫描Dubbo的Service注解
public class MemberProvider {
    public static void main(String[] args) {
        SpringApplication.run(MemberProvider.class, args);
    }
}
```

### 修改商品服务中的ProductServiceImpl

```java
package cn.wolfcode.dubbo.product.service.impl;

@Service
public class ProductServiceImpl implements IProductService {
    @Reference //引入会员服务
    private IUserService userService;

    public Product get(Long productId, Long userId) {        
        Product product = ProductData.get(productId);
        //调用会员服务获取会员信息
        User user = userService.get(userId);
        product.setUser(user);
        return product;
    }
}
```

### @Autowired or @Reference

@Autowired是引用当前项目的Bean对象

@Reference是引用远程项目中Bean对象

其实可以这样区分，就看当前的项目中是否有该Bean对象，有就是@Autowired，没有就是@Reference

### 事务问题

<img src="images/20190803114143.png" />

1. A服务开启事务对B服务发起远程调用
2. B服务掉调用也开启了一个事务，然后进行业务操作，业务正常执行，最终提交事务
3. A服务拿到远程调用的结果继续执行操作，但是后面出现异常了
4. 请问A事务的回滚能影响B服务中的事务吗？

明显事务A和事务B是分别在不同机器上开启的事务，相互独立，他们是两个不同的事务，因此传统的事务管理方式是不能应用在分布式系统中的，在分布式系统中有专门的分布式事务处理方式，如：强一致性，最终一致性



## Dubbo Admin管控台

github源码地址：https://github.com/apache/dubbo-admin/tree/master

该项目是SpringBoot开发的需要使用maven命令打包后运行，或者直接放在idea工具中运行也可以，这里给大家直接发一个老师打包好的应用，该项目的运行必须有注册中心的支持，也就是说必须先运行zookeeper

zookeeper开启后直接使用java命令执行jar包即可

```
java -jar dubbo-admin.jar
```

默认端口：7001 ，账号密码都是root



## Dubbo的服务治理

Dubbo提供的功能还有很多，我们可以从官方文档中去学习怎么做个性的功能配置

文档地址：http://dubbo.apache.org/zh-cn/docs/user/quick-start.html

### 启动时检测

在实际开发中，经常会存在服务交叉引用的情况，如：A服务引用B服务，B服务也引用A服务，那么此时就存在问题，无论哪个服务在启动时都会报错

<img src="images/20190617163351.png" />

此时我们可以设置**消费者**项目启动时不要去检查服务是否存在，就可以顺利的启动项目了，但是在运行时服务依然不存在的话，还是会报错

```properties
#消费者不检测服务是否存在
dubbo.consumer.check=false
```

### 服务集群

只有一个服务对象时，所有压力都落在同一个对象上，逼近极限时很可能导致服务挂掉，我们可以通过多发布几个服务对象，通过负载均衡策略来缓解单一服务对象压力过大问题

1. 生产者多发布几个服务对象，注意修改多个服务发布的端口

   ```properties
   # 生产者发布端口
   dubbo.protocol.port=20880 #生产者A
   # ------------
   dubbo.protocol.port=20883 #生产者B
   # ------------
   ...
   ```

2. 消费者修改负载均衡策略，有以下选择

   ```
   RandomLoadBalance：随机（random），默认策略
   RoundRobinLoadBalance：轮询（roundrobin）
   ConsistentHashLoadBalance：hash一致（consistenthash）
   LeastActiveLoadBalance：最少活跃（leastactive）
   ```

   ```properties
   #appliaction.properties
   #修改消费者负载均衡策略
   dubbo.consumer.loadbalance=roundrobin
   ```

### 多版本发布

服务在升级改造的过程中，由于不清楚新版本的服务是否存在BUG，往往都是采用过度的方式来进行切换，此时就要求两个版本的服务都要存在

<img src="images/20190618113003.png" />

生产者在生产服务的时候指定该服务的版本号

```java
@Service(version="1.0")
public class UserServiceImpl implements IUserService { ... }

@Service(version="2.0")
public class UserServiceImpl implements IUserService { ... }
```

消费者则必须明确告知引用哪个版本的服务

```java
@Reference(version="1.0")
private IUserService userService;

@Reference(version="2.0")
private IUserService userService;

@Reference(version="*") //随机引用
private IUserService userService;
```

### 服务超时，重试，容错

在服务调用的过程中，有可能服务生产者网络环境差，但是消费者并不知道，依然发出请求，长时间没有回应，此时我们可以设置消费者等待的超时时间，当调用超过设置的时间时放弃等待远程的响应，默认超时时间是：1s

当发生超时时，框架并不会马上就放弃服务的调用，还会进行重试，默认重试次数：2次

我们可以修改消费端的配置来改变默认的超时时间和重试次数

```properties
#消费者设置超时时间1.5s
dubbo.consumer.timeout=1500
#消费者设置重试次数，重试1次
dubbo.consumer.retries=1

#注意：只有幂等性操作才能重试，非幂等性操作是不能重试的
```

此时**因超时调用失败**，出现的报错页面会直接的反馈给消费者，消费者再把报错信息响应出去，用户就会直接看到错误页面，这样不友好，而且用户也看不懂，应该对错误信息进行处理，给用户一个**稍微正常**点的数据，这个就是服务的容错

从Dubbo Admin控制台去配置当前服务的容错，当服务不能正常调用时，返回null代替异常的信息

<img src="images/20190618102305.png" />

另外，服务集群后还能配置集群下的容错机制，有以下策略可以选择：

```
FailoverCluster：失败自动切换，默认策略，用于幂等性操作，如：查询
FailfastCluster：快速失败，只发起一次调用，失败立即报错，用于非幂等性操作，如：插入数据
FailsafeCluster：失败安全，出现异常时，直接忽略。通常用于写入审计日志等操作
FailbackCluster：失败自动恢复，后台记录失败请求，定时重发。通常用于消息通知操作
ForkingCluster：并行调用多个服务器，只要一个成功即返回。通常用于实时性要求较高的读操作，但需要浪费更多服务资源。可通过 forks="2" 来设置最大并行数
BroadcastCluster：广播调用所有提供者，逐个调用，任意一台报错则报错。通常用于通知所有提供者更新缓存或日志等本地资源信息
```

```properties
#消费者配置服务集群容错策略
dubbo.consumer.cluster=failfast
```

### 服务降级

当服务器压力剧增的情况下，根据实际业务情况及流量，对一些服务和页面有策略的不处理或换种简单的方式处理，从而释放服务器资源以保证核心交易正常运作或高效运作。

<img src="images/20190618105205.png" />

此时我们要保证服务B能抗住压力，就只能去牺牲服务A，甚至直接把服务A功能关闭掉，把资源留给服务B使用，此时我们就可以对服务A进行降级处理

从Dubbo Admin控制台去配置当前服务的降级，消费者访问降级的服务时，不发起远程调用请求，直接返回null

<img src="images/20190618110223.png" />