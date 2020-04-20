# Redis 

<img src="images/logo.png"/>

## 课程目标

- 了解NoSQL数据库
- 掌握Redis的基本操作
- 掌握Java操作Redis
- 掌握Redis的应用场景(面试必问)



## Redis介绍

### NoSQL的简介

- NoSQL，全称是”Not Only Sql”,指的是非关系型的数据库。这类数据库主要有这些特点:非关系型的、分布式的、开源的、水平可扩展的。原始的目的是为了大规模 web 应用，这场全新的数据库革命运动早期就有人提出，发展至 2009 年趋势越发高涨。NoSQL 的拥护者们提倡运用非关系型的数据存储，通常的应用如:模式自由、支持简易复制、简单的 API、最终的一致性(非 ACID)、大容量数据等。NoSQL 被我们用得最多的当数 key-value 存储，当然还有其他的文档型的、列存储、图型数据库、xml 数据库等。相对于目前铺天盖地的关系型数据库运用，这一概念无疑是一种全新思维的注入。

### 为什么诞生NoSQL

- 随着互联网 web2.0 网站的兴起，非关系型的数据库现在成了一个极其热门的新领域，系数据库产品的发展非常迅速，而传统的关系型数据库在应付 web2.0 网站，特􏰂是超大规 模和高并发的 SNS 类型的 web2.0 纯动态网站已经显得力不从心，暴露了很多难以克服的问题，比如

  ````
  1 、 Highperformance- 对数据库高并发读写的需求

  web2.0 网站要根据用户个性化信息来实时生成动态页面和提供动态信息，所以基本上无法使用动态页面静态化技术，因此数据库并发负载非常高，往往要达到每秒上万次读写请求。关系型数据库应付上万次 SQL 查询还勉强顶得住，但是应付上万次 SQL 写数据请求，硬盘IO 就已经无法承受了，其实对于普通的 BBS 网站，往往也存在对高并发写请求的需求。

  2 、HugeStorage- 对海量数据的高效率存储和访问的需求

  对于大型的 SNS 网站，每天用户产生海量的用户动态信息，以国外的 Friend feed 为例，一个月就达到了 2.5 亿条用户动态，对于关系数据库来说，在一张 2.5 亿条记录的表里面进行SQL 查询，效率是极其低下乃至不可忍受的。再例如大型 web 网站的用户登录系统，例如腾讯，盛大，动辄数以亿计的帐号，关系数据库也很难应付。

  3、HighScalability&&HighAvailability- 对数据库的高可扩展性和高可用性的需求

  在基于 web 的架构当中，数据库是最难进行横向扩展的，当一个应用系统的用户量和访问量与日俱增的时候，你的数据库却没有办法像 web server 和 app server 那样简单的通过添加更多的硬件和服务节点来扩展性能和负载能力。对于很多需要提供 24 小时不间断服务的网站来说，对数据库系统进行升级和扩展是非常痛苦的事情，往往需要停机维护和数据迁移，可是停机维护随之带来的就是公司收入的减少
  ````

  在上面提到的“三高”需求面前，关系数据库遇到了难以克服的障碍，而对于 web2.0 网站
  来说，关系数据库的很多主要特性却往往无用武之地，例如:

  ```
  1、数据库事务一致性需求
  很多 web 实时系统并不要求严格的数据库事务，对读一致性的要求很低，有些场合对写一 致性要求也不高。因此数据库事务管理成了数据库高负载下一个沉重的负担。

  2、数据库的写实时性和读实时性需求 对关系数据库来说，插入一条数据之后立刻查询，是肯定可以读出来这条数据的，但是对于 很多 web 应用来说，并不要求这么高的实时性。

  3、对复杂的 SQL 查询，特􏰂􏰂􏰂􏰂是多表关联查询的需求
  任何大数据量的 web 系统，都非常忌讳多个大表的关联查询，以及复杂的数据分析类型的 复杂 SQL 报表查询，特􏰂是 SNS 类型的网站，从需求以及产品设计角度，就避免了这种情 况的产生。往往更多的只是单表的主键查询，以及单表的简单条件分页查询，SQL 的功能被 极大的弱化了。

  ```

  因此，关系数据库在这些越来越多的应用场景下显得不那么合适了，为了解决这类问题的 NoSQL 数据库应运而生。

### NoSQL和关系型数据库的区别

- 在关系型数据库数据都是存放在表中,有分类存放,连接查询,主键,外键等概念
- NoSQL泛指非关系型数据库,采用区别于关系型数据库的设计,主要是针对关系型数据库性能瓶颈来设计的,专门处理关系型数据库不擅长做的业务场景,不同的NoSQL针对的点不一样,大致分为以下几类:
  - 键值存储: Redis 多用于项目的高速缓存
  - 文档存储: MongoDB 广泛用于社交类应用
  - 文件存储: FastDFS  多用于以文件为载体的在线服务,如相册网站/视频网站等等
  - 列式存储: HBase 主要用于数据分析领域

### Redis的数据结构

Redis是属于键值存储的NoSQL数据库,该数据库主要是提供高性能的键值存储,可以简答的理解为是一个极其高性能的超大Map,由于这种数据库的数据结构比较简单,所以没有关系型数据库那么多的功能,如:

- Redis中事务只有成功,没有失败

- 没有表的概念和表相关的操作
- 单线程执行,没有线程安全问题

Redis支持的数据类型:

- string
- hash
- list
- set
- zset

### Redis特点

1. 性能高,能达到10w次读/s,8w次写/s
2. 具有比较简单的ACID，如：没有事务回滚
3. 单线程操作,每个操作都是原子操作,没有并发相关问题

### 那些人在用Redis

```
比较著名的公司有：
github、blizzard、stackoverflow、flickr

国内
新浪微博（全球最大的redis集群）
	2200+亿 commands/day 5000亿Read/day 500亿Write/day
	18TB+ Memory
	500+ Servers in 6 IDC 2000+instances
淘宝
腾讯微博
```

### 怎么学习Redis

```
redis在线入门 ： http://try.redis.io/
redis 中文资料站： http://www.redis.cn/
https://www.runoob.com/redis/redis-tutorial.html
```



## 安装Redis

傻瓜式安装,下一步,下一步就可以了

Redis默认端口是: 6379

安装好后会自动启动服务器,并且没有密码

进入到安装目录下,使用cmd命令行运行redis-cli.exe程序,出现下面界面表示安装成功

<img src="images/cli.png"/>



## Redis支持的数据类型

在线教程: https://www.runoob.com/redis/redis-tutorial.html

Redis命令格式:  类型命令 key 参数数据

### string类型

普通的字符串类型,表示一个简单值

<img src="images/string.png" />
```
set key value -> 存入键值对
get key -> 根据键取出值
getset key value -> 返回旧值后存入新值
incr key -> 把值递增1
decr key -> 把值递减1
incrby key num -> 偏移值
append key 'value' -> 原值后拼接新内容
setnx key value -> 存入键值对,键存在时不存入
setex key timeout value -> 存入键值对,timeout表示失效时间,单位s
ttl ->可以查询出当前的key还剩余多长时间过期
setrange key index value -> 修改键对应的值,index表示开始的索引位置
mset k1 v1 k2 v2 ... -> 批量存入键值对
mget k1 k2 ... -> 批量取出键值
del key -> 根据键删除键值对
```
### hash类型

hash类型 / hash对象,其实就是Map类型,其内部又可以有多个键值对,可以用于存储对象

<img src="images/hash.png" />
```
hset key hashkey hashvalue -> 存入一个hash对象
hget key hashkey -> 根据hash对象键取去值
hincrby key hashkey 递增值 -> 递增hashkey对应的值
hexists key hashkey -> 判断hash对象是含有某个键
hlen key -> 获取hash对象键的数量
hkeys key -> 获取hash对象的所有键
hvals key -> 获取hash对象的所有值
hgetall key -> 获取hash对象的所有数据
hdel key hashkey -> 根据hashkey删除hash对象键值对
同样有hsetnx,其作用跟用法和setnx一样
```
### list类型

list类型更多的倾向队列,能直接操作首尾元素

<img src="images/list.png" />

```
rpush key value -> 往列表右边添加数据
lpush key value -> 往列表左边添加数据
lpop key -> 弹出列表最左边的数据
rpop key -> 弹出列表最右边的数据
lrange key start end -> 范围显示列表数据,全显示则设置0 -1 
linsert key before/after refVal newVal -> 参考值之前/后插入数据
lset key index value -> 根据索引修改数据
lrem key count value -> 在列表中按照个数删除数据
ltrim key start end -> 范围截取列表
lindex key index -> 根据索引取列表中数据
llen key -> 获取列表长度
```

### set类型

跟Java中的set集合性质一样,底层使用哈希表实现的,存入的元素是无序不可重复的,我们可以通过Redis提供的命令来取交集,并集,差集

```
sadd key value -> 往set集合中添加元素
smembers key -> 列出set集合中的元素
srem key value -> 删除set集合中的元素
spop key count -> 随机弹出集合中的元素
sdiff key1 key2 -> 返回key1中特有元素
sdiffstore var key1 key2 -> 返回key1中特有元素存入另一个set集合
sinter key1 key2 -> 返回两个set集合的交集
sinterstore var key1 key2 -> 返回两个set集合的交集存入另一个set集合
sunion  key1 key2 -> 返回两个set集合的并集
sunionstore var key1 key2 -> 返回两个set集合的并集存入另一个set集合
smove key1 key2 value -> 把key1中的某元素移入key2中
scard key -> 返回set集合中元素个数
sismember key value -> 判断集合是否包含某个值
srandmember key count -> 随机获取set集合中元素
```

### zset类型

```
zadd key num name -> 存入数值和名称
zrange key start end -> 按照数值升序输出名称
zrangebyscore key min max [withscores] -> 按照数值范围升序输出名称
zrevrange key start end -> 按照数值降序输出名称
zrevrangebyscore key max min [withscores] -> 按照数值范围降序输出名称
zrem key name -> 删除名称和数值
zincrby key num name -> 偏移名称对应的数值
zrank key name -> 升序返回排名
zrevrank key name -> 降序返回排名
zremrangebyscore key max min [withscores] -> 根据分数范围删除元素
zremrangebyrank key start end -> 根据排名删除元素
zcard key -> 返回元素个数
zcount key min max -> 按照分数范围统计个数
```



## Redis的管理命令(了解)

### 管理key的命令

```
exists key -> 判断某个key是否存在
expire key second -> 设置key的过期时间
persist key -> 取消key的过期时间
select index -> 切换数据库索引,范围是0 ~ 15共16个分区
move key index -> 把某个key-value移动到其他索引中
rename oldKey newKey -> 把oldKey重命名为newKey
info -> 查看当前服务器信息
flushdb -> 清空当前库中的数据
flushall -> 清空所有库中的数据
```

### 设置密码

修改**安装目录/redis.windows-service.conf**配置文件,在443行位置,去掉注释

```properties
#在443行位置
requirepass 密码
```

此时我们就需要通过密码认证才能执行命令

方式1: redis客户端中 auth 密码

方式2: 命令行中 redis-cli.exe -a 密码



## Redis的持久化方案

### RDB

【全量】RDB 持久化，是指在指定的时间间隔内将内存中的数据集快照写入磁盘。实际操作过程是，fork 一个子进程，先将数据集写入临时文件，写入成功后，再替换之前的文件，用二进制压缩存储。

### AOF

【增量】AOF持久化，以日志的形式记录服务器所处理的每一个写、删除操作，查询操作不会记录，以文本的方式记录，可以打开文件看到详细的操作记录。

## Jedis

### 连接示意图

<img src="images/connection.png" />

### 准备环境

```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.1.3.RELEASE</version> 
</parent>

<properties>
    <java.version>1.8</java.version>
</properties>

<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
    	<artifactId>spring-boot-starter</artifactId>
    </dependency>
    
    <dependency>
        <groupId>org.springframework.boot</groupId>
    	<artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
    </dependency>
    
    <!-- SpringBoot整合Spring Data Redis -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-redis</artifactId>
    </dependency>

    <!-- redis的驱动包:jedis -->
    <dependency>
        <groupId>redis.clients</groupId>
        <artifactId>jedis</artifactId>
    </dependency>
</dependencies>

<build>
    <plugins>
        <!-- SpringBoot打包插件 -->
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
        </plugin>
    </plugins>
</build>
```

### 模板代码

在jedis中,API方法名跟Redis命令名称是完全一样的,也就是说只要调用jedis对象对应的方法就能完成相应的操作

如:

> set name bunny ===> jedis.set("name", "bunny");
>
> get name ===> String val = jedis.get("name");

```java
@Test
public void testJedisPool() {
    // 1:创建Jedis连接池
    JedisPool pool = new JedisPool("localhost", 6379);
    // 2:从连接池中获取Jedis对象
    Jedis jedis = pool.getResource();
    /* 设置密码
	jedis.auth(密码); */
    // 3:TODO
    System.out.println(jedis);
    // 4:关闭资源
    jedis.close();
    pool.destroy();
}
```

## Spring Data Redis

### 代码

```properties
#application.properties
spring.redis.host=localhost #默认值
spring.redis.port=6379 #默认值
spring.redis.password=admin #根据情况配置
```

```java
//直接注入框架封装好的StringRedisTemplate
@Autowired
private StringRedisTemplate redisTemplate;

@Test
public void testRedisTemplate() {
    // 操作string
    redisTemplate.opsForValue().xx();
    // 操作hash
    redisTemplate.opsForHash().xx();
    // 操作list
    redisTemplate.opsForList().xx();
    // 操作set
    redisTemplate.opsForSet().xx();
    // 操作zset
    redisTemplate.opsForZSet().xx();
}

注意：以上的xx表示命令的名称，如：set / put / get 等
```



## Redis的应用场景

### 缓存

在项目中我们对于不常变动的数据,我们就能缓存起来,下次需要查询的时候我们可以先从缓存中查询,有则直接返回结果,避免去查询关系型数据库,如果没有缓存再从关系型数据库中查询,并且缓存到Redis中,然后再返回

```java
//业务层方法
public User get(Long id) {
    User user = null;
    //避免多个对象的key一致,采用全限定名:id的形式来作为缓存的key
    String cacheKey = "cn.wolfcode.xxx.domain.User:"+id;
    //从redis中取出数据
    String cacheVal = jedis.get(cacheKey);
    if (cacheVal == null) {
        //redis中没有数据,再从关系型数据库中查询
        user = userMapper.selectByPrimaryKey(id);
        //把查询到的结果存到redis中
        jedis.set(cacheKey, JSON.toJSONString(user));
    } else {
        //redis中有数据,再把该数据解析成一个User对象
        user = JSON.parseObject(cacheVal, User.class);        
    }
    return user;
}
```

### 实时统计点赞总数

项目在启动时从关系型数据库查询出点赞数量后,把该数据存入redis中,当用户每次点赞时,使用redis提供的incr增加点赞总数,当用户再次查询点赞总数时,直接从redis中查询点赞总数,显示到页面中.再配合定时器,每隔一段时间,把redis中的点赞总数存入到关系型数据库中,完成数据的同步

### 热门推荐

每个App都有自己的首页,首页中推荐的热门内容都是一样的,像这些内容就可以使用list形式存入redis中

### 朋友圈点赞

规定内容的存入格式, key:user​:​id:post:id   value:内容

规定点赞的存入格式, key:user​:​id:post​:​id:news   value:list

1. 发一条朋友圈: set user:1post:2 "hello redis"

2. 点赞: lpush user:1:post:2:news '{"id":3,"name":"bigfly","headImg":"xxx.png"}'

   ​          lpush user:1:post:2:news '{"id":4,"name":"wangnima","headImg":"xxx.png"}'

3. 查询有多少人点赞: llen user:1:post:2:news

4. 查询有哪些人点赞: lrange user:1:post:2:news 0 -1

### 抽奖

参与抽奖的人员不能重复的,并且抽到奖后不能再继续参与下轮抽象

sadd lucky xiaoyao bunny bigfly wangnima zhangquandan tangmaru daduizhang yixiaoxing wangdachui

抽3个三等奖,2个二等奖,1个一等奖

spop lucky count

### 好友推荐

系统推荐你可能认识的人

user:1:frends [user:2, user:3]

user:2:frends [user:1, user:4, user:5, user:8]

user:3:frends [user:1, user:6, user:7]

向user:1推荐可能认识的人:

1. 遍历user:1的好友
2. 把user:2 / user:3所有的好友去并集,存储起来 sunionstore tmp user:2 user:3
3. 删除自己user:1 srem tmp user:1
4. 把user:1已经认识的人删除 sdiffstore tmp tmp user:1:frends 
5. 随机推荐 srandmember tmp 2

### Redis完成限流

处于安全考虑，每次进行登录时让用户输入手机验证码，为了短信接口不被频繁访问， 会限制用户每分钟获取验证码的频率。

![4111573636873_.pic_hd](images/4111573636873_.pic_hd.jpg)

### Redis完成排行

排行榜：有序集合经典使用场景。例如视频网站需要对用户上传的视频做排行榜，榜单维护可能是多方面：
按照时间、按照播放量、按照获得的赞数等。

