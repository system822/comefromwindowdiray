# MongoDB 

<img src="images/logo.jpg" />

## 课程目标

- 了解MongoDB数据库结构
- 掌握基本的CRUD和高级查询操作
- 掌握Spring Data MongoDB的使用



## MongoDB介绍

### 简介

MongoDB是一个基于分布式文件存储的数据库。由C++语言编写。旨在为WEB应用提供可扩展的高性能数据存储解决方案。
MongoDB是一个介于关系数据库和非关系数据库之间的产品，是非关系数据库当中功能最丰富，最像关系数据库的。它支持的数据结构非常松散，是类 似json的bson格式，因此可以存储比较复杂的数据类型。Mongo最大的特点是它支持的查询语言非常强大，其语法有点类似于面向对象的查询语言，几乎可以实现类似关系数据库单表查询的绝大部分功能，而且还支持对数据建立索引。

### 特点

- 高性能
- 易部署
- 易使用
- 非常方便的存储数据

### 对比MySQL

<img src="images/20190620150418.png" />

MongoDB的优势

<img src="images/1.png" />

MongoDB的劣势

<img src="images/2.png" />

由于MongoDB独特的数据处理方式，可以将热点数据加载到内存，故而对查询来讲，会非常快（当然也会非常消耗内存）；同时由于采用了BSON的方式存储数据，故而对JSON格式数据具有非常好的支持性以及友好的表结构修改性，文档式的存储方式，数据友好可见；数据库的分片集群负载具有非常好的扩展性以及非常不错的自动故障转移（大赞）。

不足：数据库的查询采用了特有的查询方式，有一定的学习成本（不高）；索引不咋滴；锁只能提供到collection级别，还做不到行级锁；没有事务机制（不能回滚啊）；学习资料肯定没有MySQL的多。

### 与Redis的对比

<https://www.php.cn/redis/421928.html>

### 数据库结构

MongoDB属于NoSQL数据库，自然也是没有表相关概念的，该数据库存储使用的是集合，集合中存储的是文档(树状结构数据)

<img src="images/20190531120519.png" />

https://baijiahao.baidu.com/s?id=1628146397605671081&wfr=spider&for=pc

## 安装和连接

### 安装客户端和服务器

根据安装包的提示操作，选择自定义安装，其中只安装服务端和客户端即可



<img src="images/20190531112156.png" />

<img src="images/20190531112347.png" />

**最后千万不要勾选**

<img src="images/20190531112734.png" />

### 连接服务器

安装好后，默认会启动MongoDB服务器，可以在服务列表中找到**MongoDB Server**查看是否处于运行状态，已经处于运行状态则直接进入**安装目录/Server/bin**,运行mongo.exe，出现以下画面表示安装成功了

补充：MongoDB默认端口27017

<img src="images/20190531113302.png" />



## 基本操作

MongoDB的操作有很多，需要掌握更多技能的同学可以查询在线的文档，这里主要是讲这么去学习这个工具

在线文档教程: https://www.runoob.com/mongodb/mongodb-tutorial.html

### 语法
```
mongo localhost/admin -uroot -p ：链接数据库
show dbs: 查询所有数据库
use 数据库名: 创建并且选中数据库，数据库已经存在则直接选中
db: 查询当前选择的数据库
db.dropDatabase(): 删除当前选中的数据库
show collections: 查询当前库中的集合
db.createCollection("集合名"): 创建集合
db.集合名.drop(): 删除集合

注意: db.集合名 == db.getCollection("集合名")
```



## 新增操作

### 数据类型

在MongoDB中支持以下的数据类型

```
String（字符串）: mongodb中的字符串是UTF-8有效的 
Integer（整数）: 存储数值。整数可以是32位或64位，具体取决于您的服务器
Boolean（布尔）: 存储布尔(true/false)值
Double（双精度）: 存储浮点值
Arrays（数组）: 将数组或列表或多个值存储到⼀个键中
Timestamp（时间戳）: 存储时间戳
Object（对象）: 嵌⼊式⽂档
Null （空值）: 存储Null值
Symbol（符号）: 与字符串相同，⽤于具有特定符号类型的语⾔
Date（⽇期）: 以UNIX时间格式存储当前⽇期或时间
Object ID（对象ID） : 存储⽂档ID
Binary data（⼆进制数据）: 存储⼆进制数据
Code（代码）: 将JavaScript代码存储到⽂档中
Regular expression（正则表达式）: 存储正则表达式
```

### 语法

往集合中新增文档，当集合不存在时会自动先创建集合，再往集合中添加文档，但是不要依赖自动创建集合的特性

```
方式一: db.集合名.insert( 文档 ) : 往集合中插入一个文档

方式二: 
变量=文档
db.集合名.insert(变量)

方式三:
db.集合名.save(文档)

批量插入
通过for循环

查询所有文档: db.集合名.find({}): 查询集合中所有文档
```
存储在 MongoDB 集合中的每个文档(document)都有一个默认的主键_id，这个主键名称是固定的，它可以MongoDB 支持的任何数据类型，默认是 ObjectId。在关系数据库 schema设计中，主键大多是数值型的，比如常用的 int 和 long，并且更通常的是主键的取值由数据库自增获得，这种主键数值的有序性有时也表明了某种逻辑。反观 MongoDB，它在设计之初就定位于分布式存储系统，所以它原生的不支持自增主键。

### 实例

```
//插入员工对象
db.users.insert({id: NumberLong(1), name: "zhangsan", age: NumberInt(18)})
db.users.insert({id: 2, name: "zhangsan", age: 18})

注意：直接写数字默认是Duble类型，其实在使用时是不影响的，仅仅只是类型问题
```



## 更新操作

修改集合中已经存在的文档

### 语法
```
db.集合名.update(
  <query>，
  <update>，
  {
        upsert: <boolean>，
        multi: <boolean>，
        writeConcern: <document>
  }
)

简化方法：
更新1个：db.集合名.updateOne( ... )
更新所有：db.集合名.updateMany( ... )
```
**参数说明：**

- **query** : update的查询条件，类似sql update查询内where后面的。
- **update** : update的对象和一些更新的操作符（如$，$inc...）等，也可以理解为sql update查询内set后面的
- **upsert** : 可选，这个参数的意思是，如果不存在update的记录，是否插入objNew，true为插入，默认是false，不插入。
- **multi** : 可选，mongodb 默认是false，只更新找到的第一条记录，如果这个参数为true，就把按条件查出来多条记录全部更新。
- **writeConcern** :可选，抛出异常的级别

### 实例

```
//把一个带有name=zhangsan的文档，修改其age值为30
db.employees.updateOne({name: "zhangsan"}, {$set: {age: 30}})

//修改所有name=zhangsan的文档，修改其name=lisi，age=20
db.employees.updateMany({name: "zhangsan"}， {$set: {name: "lisi", age: 30}})

//修改所有的文档，修改其name=xxx，age=10
db.employees.updateMany({}， {$set: {name: "xxx", age: 10}})
```

## 删除操作

删除集合中的文档

### 语法
```
db.集合名.remove(
  <query>，
  {
        justOne: <boolean>，
        writeConcern: <document>
  }
)

简化方法：
删除1个：db.集合名.deleteOne( ... )
删除所有：db.集合名.deleteMany( ... )
```
**参数说明：**

- **query** :（可选）删除的文档的条件。
- **justOne** : （可选）如果设为 true 或 1，则只删除一个文档，如果不设置该参数，或使用默认值 false，则删除所有匹配条件的文档。
- **writeConcern** :（可选）抛出异常的级别

### 实例

```
//删除_id=xxx的文档
db.users.deleteOne({_id: ObjectId("xxx")})

//删除所有带有name=bunny的文档
db.users.deleteMany({name: "bunny"})

//删除当前数据库中所有文档
db.users.deleteMany({})
```

## 查询操作

### 语法
```
db.集合名.find(query， projection)
```
- **query** ：可选，使用查询操作符指定查询条件
- **projection** ：可选，使用投影操作符指定返回的键。查询时返回文档中所有键值， 只需省略该参数即可（默认省略）

### 实例

```
方式一
//查询所有文档
db.users.find({})

方式二
var cursor = db.things.find();
while (cursor.hasNext()) printjson(cursor.next());

方式三
db.things.find().forEach(printjson);
```

### 查询总条数

```
方式一 db.users.find().count()
方式二 db.users.count()
```

### 排序查询

```
db.users.find({}).sort({字段: 1}) -> 按照字段升序排列
db.users.find({}).sort({字段: -1}) -> 按照字段降序排列
```
### 分页查询
```
sikp(num): 跳过num个文档，相当于start
limit(num): 限制显示num个文档，相当于pageSize
如：
db.users.find({}).skip(num).limit(num)

需求：按照年龄降序排列，查询第2页，每页显示3个
```



## 高级查询

高级查询在数据库中是经常使用的，在MongoDB中也同样是提供了高级查询的功能

### 等值

```
name=zhangsan
语法 -> find({字段: 值}) 
findOne 语法 db.集合.findOne({字段:值})
```

需求

```
需求1：查询所有name=zhangsan的文档
需求2: 查询年龄age=18
```

### 比较查询

```
语法 -> find({字段: {比较操作符: 值， ...}})
```
#### 比较操作符

- (>) 大于 - $gt
- (<) 小于 - $lt
- (>=) 大于等于 - $gte
- (<= ) 小于等于 - $lte
- (!=) 不等 - $ne
- 集合运算 - $in(包含)   (如:{name: {$in: ["xiaoyao"，"bunny"]}}
- 判断存在 - $exists    如:{name: {$exists:true}}
- 取模运算 - $mod 如:{age:{$mod:[10,1]}}  表示模于10余1的文档

需求

```
需求1 查询年龄大于30的文档
需求2 查询年龄大于30小于37的文档
需求3 查询年龄大于等于25小于40的文档
需求4 查询名字不等于bunny的文档
需求5 查询文档中带有name的文档
需求6 查询年龄模以5余0的文档
需求7 查询名字是bunny或者是zhangsan的文档
```

### 逻辑查询

```
语法 -> find({逻辑操作符: [条件1， 条件2， ...]})
```
#### 逻辑操作符

- (&&) 与 - $and
- (||) 或 - $or
- (!) 非 - $not

```
注意:not的语法和上面有所不同
 { field: { $not: { <operator-expression> } } }
```

需求

```
需求1：查询所有name=bunny或者age<30的文档
需求2: 查询所有name=bunny并且age=18的文档
需求3: 查询所有age<30的使用非
```

### 模糊查询 

MongoDB的模糊查询使用的是正则表达式的语法     如:{name: {$regex: /^.\*keyword\.*$/}}

实际上MongoDB也是不擅长执行模糊查询的，在实际开发中也是不使用的，**该功能了解即可**

```
需求1：查询所有name含有wang并且30<=age<=32的文档
```



## 设置用户

以下操作必须在cmd命令行中操作，执行以下命令

```
//1.选中admin数据库
use admin
//2.往里面添加一个超级管理员账号
db.createUser({user:"root", pwd: "admin", roles:["root"]})
//user:账号    pwd:密码    roles:角色->root超级管理员
```

修改MongoDB的配置文件：**安装目录/Server/bin/mongod.cfg**

```yaml
#约在29行位置，配置开启权限认证
security:
  authorization: enabled
```

完成上面配置后重启服务器

在登录时就必须要输入密码，否则不能执行任何的命令

<img src="images/20190601092036.png" />

## 文档的设计

需求:朋友圈与点赞功能设计

原来MySQL实现查询朋友圈如何实现,有了MongoDB又如何实现.

## Spring Data MongoDB

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
    
    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
    </dependency>

    <!--spring boot data mongodb-->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-mongodb</artifactId>
    </dependency>
</dependencies>

<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
        </plugin>
    </plugins>
</build>
```

### 配置连接参数

```properties
# application.properties
# 配置数据库连接
#格式: mongodb://账号:密码@ip:端口/数据库?认证数据库
spring.data.mongodb.uri=mongodb://root:admin@localhost/mongotest?authSource=admin

# 配置MongoTemplate的执行日志
logging.level.org.springframework.data.mongodb.core=debug
```

### domain

```java
@AllArgsConstructor
@NoArgsConstructor
@Setter@Getter@ToString
@Document("users") //设置文档所在的集合
public class User {
    @Id //文档的id使用ObjectId类型来封装，并且贴上@Id注解
    private ObjectId _id;
    
    private Long id;
    private String name;
    private Integer age;
}
```

### MongoRepository

该接口对MongoDB数据的常用操作进行了封装，我们只需要写个接口去继承该接口就能直接拥有的CRUD等基本操作，再学习下JPA的方法命名规范，可以毫不费力的完成各种复杂的高级操作

```java
/**
 * 自定义一个接口继承MongoRepository，
 * 泛型1：domain类型
 * 泛型2：文档主键类型
 * 贴上@Repository注解，底层会创建出动态代理对象，交给Spring管理
 */
@Repository
public interface UserMongoRepository extends MongoRepository<User, ObjectId> {
    // 使用Spring Data命名规范做高级查询
    List<User> findByName(String name);
}
```

#### Spring Data 方法命名规范


|       关键字        |                    例子                    |              JPQL               |
| :--------------: | :--------------------------------------: | :-----------------------------: |
|       And        | findByNameAndAge(String name, Integer age) |   where name = ? and age = ?    |
|        Or        | findByNameOrAge(String name, Integer age) |    where name = ? or age = ?    |
|        Is        |         findByName(String name)          |         where name = ?          |
|     Between      | findByAgeBetween(Integer min, Integer max) |    where age between ? and ?    |
|     LessThan     |      findByAgeLessThan(Integer age)      |          where age < ?          |
|  LessThanEqual   |   findByAgeLessThanEqual(Integer age)    |         where age <= ?          |
|   GreaterThan    |    findByAgeGreaterThan(Integer age)     |          where age > ?          |
| GreaterThanEqual |  findByAgeGreaterThanEqual(Integer age)  |         where age >= ?          |
|      After       |              等同于GreaterThan              |                                 |
|      Before      |               等同于LessThan                |                                 |
|      IsNull      |            findByNameIsNull()            |       where name is null        |
|    IsNotNull     |          findByNameIsNotNull()           |     where name is not null      |
|       Like       |       findByNameLike(String name)        |        where name like ?        |
|     NotLike      |      findByNameNotLike(String name)      |      where name not like ?      |
|   StartingWith   |   findByNameStartingWith(String name)    |      where name like '?%'       |
|    EndingWith    |    findByNameEndingWith(String name)     |      where name like '%?'       |
|    Containing    |    findByNameContaining(String name)     |      where name like '%?%'      |
| OrderByXx[desc]  |    findByIdOrderByXx[Desc] (Long id)     | where id = ? order by Xx [desc] |
|       Not        |        findByNameNot(String name)        |         where name != ?         |
|        In        |       findByIdIn(List\<Long\> ids)       |       where id in ( ... )       |
|      NotIn       |     findByIdNotIn(List\<Long\> ids)      |     where id not in ( ... )     |
|       True       |              findByXxTrue()              |         where Xx = true         |
|      False       |             findByXxFalse()              |        where Xx = false         |
|    IgnoreCase    |    findByNameIgnoreCase(String name)     |     where name = ? (忽略大小写)      |

#### 实例代码

```java
@Autowired
private UserMongoRepository repository;

// 插入/更新一个文档
@Test
public void testSaveOrUpdate() throws Exception {
    User user = new User(null, 5L, "bunny", 20);
    // 主键为null则新增，不为null则更新
    repository.save(user);
}

// 删除一个文档
@Test
public void testDelete() throws Exception {
    repository.deleteById(new ObjectId("xxx"));
}

// 查询一个文档
@Test
public void testGet() throws Exception {
    Optional<User> optional = repository.findById(new ObjectId("xxx"));
    optional.ifPresent(System.err::println);
}

// 查询所有文档
@Test
public void testList() throws Exception {
    // 查询所有文档
    List<User> list = repository.findAll();
    list.forEach(System.err::println);
}
```

### MongoTemplate

该对象有SpringBoot完成了自动配置，存入Spring容器中，我们直接注入就可以使用了，依靠该对象能完成任何的MongoDB操作，一般和MongoRepository分工合作，多数用于复杂的高级查询以及底层操作

```java
//注入MongoTemplate
@Autowired
private MongoTemplate template;
```

#### 条件限定

Query对象用于封装查询条件，配合Criteria一起使用，来完成各种条件的描述

```java
//一个Criteria对象可以理解为是一个限定条件
Criteria.where(String key).is(Object val); //设置一个等值条件
Criteria.orOperator(Criteria ...); //设置一组或的逻辑条件

//模糊查询（了解）
Criteria.where(String key).regex(String regex); //使用正则表达式匹配查询

注意：Criteria对象可以由其静态方法和构造器获取
Criteria封装了所有对条件的描述，常见的有以下方法
lt / lte / gt / gte / ne / ...
```

最后通过Query对象的addCriteria把条件封装到Query对象中

```java
Query对象.addCriteria(Criteria criteria); //添加查询条件
Query对象.skip(start).limit(pageSize); //分页查询
```

#### API方法

```java
//根据条件查询集合中的文档
List<T> mongoTemplate.find(Query query, Class<T> type, String collectionName);
```

#### 实例代码

```java
@Autowired
private MongoTemplate mongoTemplate

// 分页查询文档，显示第2页，每页显示3个，按照id升序排列
@Test
public void testQuery1() throws Exception {
    // 创建查询对象
    Query query = new Query();
    // 设置分页信息
    query.skip(3).limit(3);
    // 设置排序规则
    query.with(Sort.by(Sort.Direction.ASC,"id"));

    List<User> list = mongoTemplate.find(query, User.class, "users");
    list.forEach(System.err::println);
}

// 查询所有name为bunny的文档
@Test
public void testQuery2() throws Exception {
    // 构建限制条件 {"name": "bunny"}
    Criteria criteria = Criteria.where("name").is("bunny");
    // 创建查询对象
    Query query = new Query();
    // 添加限制条件
    query.addCriteria(criteria);

    List<User> list = mongoTemplate.find(query, User.class, "users");
    list.forEach(System.err::println);
}

// 查询所有name为bunny或者age<30的文档
@Test
public void testQuery3() throws Exception {
    // 构建限制条件 { "$or": [{"name": "bunny"}, {"age": {"$lt": 30}}] }
    Criteria criteria = new Criteria().orOperator(
        Criteria.where("name").is("bunny"),
        Criteria.where("age").lt(30)
    );
    // 创建查询对象
    Query query = new Query();
    // 添加限制条件
    query.addCriteria(criteria);

    List<User> list = mongoTemplate.find(query, User.class, "users");
    list.forEach(System.err::println);
}

// 查询所有name含有wang并且30<=age<=32的文档
@Test
public void testQuery4() throws Exception {
    // 构建限制条件 { "$and" : [{"name": {"$regex": ".*wang.*"} }, {"age": {"$gte": 30, "$lte": 32 } }] }
    Criteria criteria = new Criteria().andOperator(
        Criteria.where("name").regex(".*wang.*"),
        Criteria.where("age").gte(30).lte(32)
    );
    // 创建查询对象
    Query query = new Query();
    // 添加限制条件
    query.addCriteria(criteria);

    List<User> list = mongoTemplate.find(query, User.class, "users");
    list.forEach(System.err::println);
}
```
