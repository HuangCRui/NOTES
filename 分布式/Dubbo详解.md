



# 分布式系统架构解决方案——Dubbo







# dubbo概述





## 1.1 什么是分布式系统



- 《分布式系统原理与范型》中定义：
  - “分布式系统是若干独立计算机的集合，这些计算机对于用户来说就像单个相关系统”
  - 分布式系统distributed system，是建立在网络之上的软件系统
  - 简单来说：多个（**不同职责**）人共同来完成一件事！
  - 任何一台服务器都无法满足淘宝的双十一的数据吞吐量，一定是**很多台服务器公共来完成的**





### 1.1.1 单一应用架构

- 当网站流量很小时，只需要一个应用，将所有的功能**部署到一起**（所有业务都放在一个tomcat服务器里），从而减少部署节点和成本
- 此时，用于**简化 增删改查工作量**的**数据访问框架**（ORM->对象关系映射Object Relational Mapping，如Mybatis）就是关键
- 例如，某个超时的收银系统，某个公司的员工管理系统



![image-20210303004106209](../picture/Dubbo%E8%AF%A6%E8%A7%A3/image-20210303004106209.png)



**优点：**

- 小项目开发快，成本低
- 架构简单
- 易于测试
- 易于部署



**缺点：**

- 大项目模块耦合严重，不易开发、维护，沟通成本高
- 新增业务困难
- 核心业务与边缘业务**混合在一块**，出现问题互相影响





### 1.1.2 垂直应用架构





- 当访问量逐渐增大，单一应用增加机器带来的加速度越来越小，将应用拆成几个 **互不相干**的几个应用，以提高效率
- 大模块按照mvc分层模式，进行拆分成多个互不相关的小模块，并且每个小模块都有独立的服务器
- 此时：用于加速前端页面开发的web框架(**MVC**)是关键，因为每个小应用都有独立的页面



![image-20210303005223194](../picture/Dubbo%E8%AF%A6%E8%A7%A3/image-20210303005223194.png)



缺点：模块之间不可能完全没有交集，**公用模块无法重复利用**，开发性的浪费



> https://baijiahao.baidu.com/s?id=1685206708309954802&wfr=spider&for=pc
>
> 每一个小应用都有一个独立的页面，在进行整个系统的调用时，将各部分应用分别调用，得到各自的页面，然后整合成为一个完整功能
>
> **但是不可能每一个应用都能做到真正的独立，所以肯定有公共部分被重复加载了**









### 1.1.3 分布式服务架构



- 当垂直应用越来越多，应用之间交互不可避免，**将核心业务抽取出来，作为独立的业务**，逐渐形成稳健的服务中心，使**前端**应用能更快地**响应**多变的市场需求
- 此时，用户提高业务复用及整合的**分布式服务框架**(**RPC**)远程调用是关键



**有需要就单独调用某个服务，没有需要就放在那**

垂直架构：捆绑消费，**不管用还是不用，都要携带这些服务**

![image-20210303011921973](../picture/Dubbo%E8%AF%A6%E8%A7%A3/image-20210303011921973.png)





RPC：独立的应用服务器之间，要依靠**RPC（Remote Procedure Call远程调用）**才能调用

- 物流服务不忙，有100台服务器：商品服务特别忙，也是1--台服务器
  - **如何做到资源优化调配**









### 1.1.4 流动计算架构

- 当服务越来越多，容量的评估，小服务**资源的浪费**等问题逐渐呈现，此时需要增加一个**调度中心**基于**访问压力实时管理集群容量**，提高集群利用率；
- 此时，用于**提高机器利用率的资源调度和治理中心（SOA）是关键**



![image-20210303015912344](../picture/Dubbo%E8%AF%A6%E8%A7%A3/image-20210303015912344.png)



> **SOA：面向服务架构  Service-Oriented Architecture，简单理解就是“服务治理”，例如公交车站的调度员**













## Dubbo简介





- Dubbo是**分布式服务框架**，是阿里巴巴的开源项目，现交给Apache进行维护
- Dubbo致力于提高性能和透明化的RPC远程服务调用方案，以及SOA服务治理方案
- 简单来说，Dubbo是个服务框架，如果没有分布式的需求，是不需要用的







### 1.2.1 RPC

- RPC  RemoteProcedure Call，是指**远程过程调用**，是一种**进程间通信方式**
- **RPC基本的通信原理**
  1. 在客户端将**对象进行序列化**
  2. 底层通信框架使用**netty**（基于tcp协议的socket），将**序列化的对象发给服务方提供方**
  3. 服务提供方通过socket得到数据文件之后，进行**反序列化**，获得**要操作的对象**
  4. **对象数据操作完毕**，将新的对象序列化，再通过服务提供方的socket**返回**给客户端
  5. 客户端获得序列化数据，再反序列化，得到最近的数据对象，至此-完成一次请求



![image-20210303021414402](../picture/Dubbo%E8%AF%A6%E8%A7%A3/image-20210303021414402.png)



RPC两个核心模块：**通讯（socket）， 序列化**，动态代理.....



> RPC就是打长途电话







### 1.2.2 节点角色



| 节点      | 角色说明                                                     |
| --------- | ------------------------------------------------------------ |
| Provider  | **提供服务**  的服务方                                       |
| Consumer  | **调用**远程服务的消费方                                     |
| Registry  | 服务注册与发现的**注册中心**，**所有的服务**都在这中心里面注册，在这里寻找具体服务 |
| Monitor   | 统计服务的调用次数和调用时间的**监控中心**，在内存中累计调用次数和调用时间，定时每分钟发送一次**统计数据**到监控中心 |
| Container | 服务运行**容器**，整合所有服务的中心                         |



![image-20210303022851982](../picture/Dubbo%E8%AF%A6%E8%A7%A3/image-20210303022851982.png)



### 1.2.3 调用关系



0. 服务**容器负责启动**，加载，**运行服务提供者**。

1. 服务提供者在启动时，**向注册中心注册自己提供的服务**。
2. 服务消费者在启动时，**向注册中心订阅自己所需的服务**。  （服务提供者和消费者都要注册到注册中心）
3. 注册中心返回服务提供者**地址列表给消费者**，如果有变更，注册中心将基于长连接推送**变更数据**给消费者。
4. 服务消费者，从提供者地址列表中，基于**软负载均衡算法**，选一台**提供者**进行调用，如果调用失败，再选另一台调用。
5. 服务消费者和提供者，在内存中**累计调用次数和调用时间**，定时每分钟发送一次统计数据到**监控中心**



> dubbo架构的特点：连通性、健壮性、伸缩性、以及向未来架构的升级性
>
> https://dubbo.apache.org/zh/docs/v2.7/user/preface/architecture/









# 2.快速入门



## 2.1 注册中心





### 2.1.1 Zookeeper



- 官方推荐使用Zookeeper注册中心
- **注册中心负责服务地址的注册与查找**，相当于**目录服务**
- 服务提供者和消费者只在**启动时与注册中心交互**，注册中**不转发请求**，压力较小
- Zookeeper是apache hadoop 的子项目，是一个树形的目录服务，支持**变更推送**，适合作为dubbo的服务注册中心，工业强度较高，可用于生产环境



> **dubbo即是求职的人，也是招聘单位------->服务提供者和消费者，而Zookeeper就是人才市场/招聘网站------>注册中心**











### 2.1.2 安装zookeeper



![image-20210303024503459](../picture/Dubbo%E8%AF%A6%E8%A7%A3/image-20210303024503459.png)





需要把之前配置的Zookeeper集群的部署注释掉：



![image-20210303024653413](../picture/Dubbo%E8%AF%A6%E8%A7%A3/image-20210303024653413.png)

只留一个zookeeper就够了





此时Zookeeper是单独运行着：

![image-20210303114021475](../picture/Dubbo%E8%AF%A6%E8%A7%A3/image-20210303114021475.png)















## 2.2 服务提供方

1. 使用springboot搭建
2. 提供一个服务接口即可



### 依赖

```xml
<dependency>
    <groupId>org.apache.zookeeper</groupId>
    <artifactId>zookeeper</artifactId>
    <version>3.6.2</version>
</dependency>
<dependency>
    <groupId>com.github.sgroschupf</groupId>
    <artifactId>zkclient</artifactId>
    <version>0.1</version>
</dependency>
<dependency>
    <groupId>com.alibaba.spring.boot</groupId>
    <artifactId>dubbo-spring-boot-starter</artifactId>
    <version>RELEASE</version>
</dependency>
```





### 目录结构

![image-20210303174926722](../picture/Dubbo%E8%AF%A6%E8%A7%A3/image-20210303174926722.png)

**切记：服务提供方和服务消费方的接口包名一定要相同**





### 配置文件



```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:dubbo="http://code.alibabatech.com/schema/dubbo"

       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://code.alibabatech.com/schema/dubbo
       http://code.alibabatech.com/schema/dubbo/dubbo.xsd">

    <!-- 消费方应用名，用于计算依赖关系，不是匹配条件，不要与提供方一样 -->
    <dubbo:application name="dubbo-consumer"/>
    <!-- 可直接连接远程zookeeper服务地址  注册中心的地址-->
    <dubbo:registry address="zookeeper://192.168.150.130:2181"/>
    <dubbo:consumer check="false"/>
    扫描类，将什么包下的类作为服务提供类
    <dubbo:annotation package="com.example.service.impl"/>

</beans>
```



### 服务接口



```java
package com.example.service.impl;

import com.alibaba.dubbo.config.annotation.Service;
import com.example.service.HelloService;

@Service//dubbo中的@Service注解
public class HelloServiceImpl implements HelloService {
    @Override
    public String sayHello(String name) {
        return "hello "+name+"!!!";
    }
}
```







```java
@ComponentScan(basePackages = {"com.example.*"})
@ImportResource(value = {"classpath:spring.xml"})
@SpringBootApplication
public class DubboServerApplication {
    public static void main(String[] args) {
        SpringApplication.run(DubboServerApplication.class, args);
    }
}
```













## 2.3 服务消费方



pom依赖与服务方一致，只需要将配置文件中的端口改为8081



### 目录结构



![image-20210303175500831](../picture/Dubbo%E8%AF%A6%E8%A7%A3/image-20210303175500831.png)







### 配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:dubbo="http://code.alibabatech.com/schema/dubbo"

       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://code.alibabatech.com/schema/dubbo
       http://code.alibabatech.com/schema/dubbo/dubbo.xsd">

    <dubbo:application name="dubbo-consumer"/>
    <dubbo:registry address="zookeeper://192.168.150.130:2181"/>
    <dubbo:annotation package="com.example.controller"/>


</beans>
```





### 接口

**包名和类名需要和服务提供方相同**

```java
package com.example.service;

public interface HelloService {

    //声明而已，具体实现会远程调用dubbo-server的实现类
    String sayHello(String name);
}
```





**在消费方中，调用服务方的service接口方法**



```java
@Controller
public class HelloAction {

    //不能自动注入autowired
    @Reference//远程去服务方将service的实现类注入进来
    private HelloService helloService;

    @GetMapping("/hello")
    @ResponseBody
    public String sayHi(@RequestParam("name")String name){
        return helloService.sayHello(name);
    }
}
```





![image-20210303175339995](../picture/Dubbo%E8%AF%A6%E8%A7%A3/image-20210303175339995.png)









> 两个项目中间**通过Zookeeper进行通讯**：
>
> ![image-20210303175910137](../picture/Dubbo%E8%AF%A6%E8%A7%A3/image-20210303175910137.png)







# 3.监控中心



在开发时，需要知道注册中心都注册了哪些服务，以便我们开发和测试

图形化显示注册中心的  服务列表

通过部署一个web应用版的**管理中心**来实现









## 3.1 服务管理端



### 3.1.1 安装管理端

1. 解压dubbo.admin-master.zip



2. 修改配置文件

   ![image-20210303180319815](../picture/Dubbo%E8%AF%A6%E8%A7%A3/image-20210303180319815.png)

   ![image-20210303180436440](../picture/Dubbo%E8%AF%A6%E8%A7%A3/image-20210303180436440.png)

端口：7001

![image-20210303180532143](../picture/Dubbo%E8%AF%A6%E8%A7%A3/image-20210303180532143.png)





3. 返回到项目根目录dubbo-admin目录，使用maven打包： `mvn clean package`

   ![image-20210303181045677](../picture/Dubbo%E8%AF%A6%E8%A7%A3/image-20210303181045677.png)

4. 在dos下运行target目录中的jar文件：dubbo-admin-0.0.1-SNAPSHOT.jar

   `java -jar java -jar dubbo-admin-0.0.1-SNAPSHOT.jar`



5. 访问：`http://localhost:7001/`

   第一次访问账号和密码都是root

![image-20210303181311526](../picture/Dubbo%E8%AF%A6%E8%A7%A3/image-20210303181311526.png)





进行搜索：

提供者：

![image-20210303181445586](../picture/Dubbo%E8%AF%A6%E8%A7%A3/image-20210303181445586.png)



消费者：

![image-20210303181722798](../picture/Dubbo%E8%AF%A6%E8%A7%A3/image-20210303181722798.png)







## 3.2 监控统计中心



Monitor：统计中心，记录服务被调用多少次等

1. 解压dubbo-minitor-simple

   ![image-20210303182203082](../picture/Dubbo%E8%AF%A6%E8%A7%A3/image-20210303182203082.png)

2. 修改配置文件 `dubbo.properties`,并修改访问端口

![image-20210303182506021](../picture/Dubbo%E8%AF%A6%E8%A7%A3/image-20210303182506021.png)







```xml
让监控 去 注册中心 自动找服务
<dubbo:monitor protocol="registry">
```



都不能用了...







# 4.综合实战

https://dubbo.apache.org/zh/docs/v2.7/user/examples/



## 4.1 配置说明





### 4.1.1 启动时检查



https://dubbo.apache.org/zh/docs/v2.7/user/examples/preflight-check/



Dubbo 缺省会在启动时**检查依赖的服务是否可用**，**不可用时会抛出异常**，**阻止 Spring 初始化完成**，以便上线时，能及早发现问题，默认 `check="true"`。

可以通过 `check="false"` 关闭检查，比如，测试时，有些服务不关心，或者出现了循环依赖，必须有一方先启动。





关闭某个服务的启动时检查 (没有提供者时报错)：

```xml
<dubbo:reference interface="com.foo.BarService" check="false" />
```

关闭所有服务的启动时检查 (没有提供者时报错)：

```xml
<dubbo:consumer check="false" />
```

关闭注册中心启动时检查 (注册订阅失败时报错)：

```xml
<dubbo:registry check="false" />
```





**这就需要在消费方编写初始化容器的main方法启动**，如果使用tomcat启动方式，必须访问一次action方法（服务方法）才能初始化spring

![image-20210303194956441](../picture/Dubbo%E8%AF%A6%E8%A7%A3/image-20210303194956441.png)



**添加日志文件;og4j.properties**

可以看到输出：

![image-20210303195650948](../picture/Dubbo%E8%AF%A6%E8%A7%A3/image-20210303195650948.png)



> Failed to check the status of the **service** **com.example.service.HelloService**. **No provider** available for the service com.example.service.HelloService from the url zookeeper://192.168.150.130:2181



加上这句配置：

```
<dubbo:consumer check="false"/>
```

不会报错



建议不要写false。需要log4j日志文件来查看是否报错



### 4.1.2 超时时间



- 由于网络或者服务端不可靠，会导致调用构成中出现不确定的阻塞状态（超时）
- 为了避免超时导致客户端资源（线程）挂起耗尽，必须设置超时时间
- 在服务提供者添加如下配置

```xml
<dubbo:provider timeout="2000"/>
```



- 可以将服务实现HelloServiceImpl.java中加入模拟的网络延迟进行测试

```java
@Service
public class HelloServiceImpl implements HelloService {
    @Override
    public String sayHello(String name) throws InterruptedException {
        Thread.sleep(4000);
        return "hello "+name+"!!!";
    }
}
```





![image-20210303200616456](../picture/Dubbo%E8%AF%A6%E8%A7%A3/image-20210303200616456.png)



在消费者调用服务方法的时候，被睡了4秒，服务端认为网络延迟导致超时，抛出异常。





**配置原则：**

- dubbo推荐**在Provider上**尽量**多配置COnsumer端属性**
  - 作为服务的**提供者**，比服务使用方**更清楚服务性能参数**，如调用的超时时间，合理的重试次数等等
  - 在Provider配置后，Consumer不配置则会使用Provider的配置值，即Provider配置可以**作为消费者的缺省值**













### 4.1.3 重试次数



- 当出现失败，自动切换并重试其它服务器，dubbo重试的缺省值是2次，我们可以自行设置

- 在Provider提供方进行配置：

  ```xml
  <dubbo:provider timeout="2000" retries="3"/>
  ```



```java
@Service
public class HelloServiceImpl implements HelloService {
    @Override
    public String sayHello(String name) throws InterruptedException {
        System.out.println("-------------------调用一次-------------------");
        Thread.sleep(3000);
        return "hello "+name+"!!!";
    }
}
```



除了本次失败，还会再尝试3次。**如果都失败了，就一共失败四次。。。**

![image-20210303202745993](../picture/Dubbo%E8%AF%A6%E8%A7%A3/image-20210303202745993.png)



- 并不是所有的方法都适合设置重试次数：
  - **幂等方法**：适合（当参数一样，无论执行多少次，结果是一样的，例如：**查询，修改**）
  - **非幂等方法**：不适合，（**当参数一样，执行结果不一样，例如：删除，添加**）
- 单独设置某个方法
  - 提供方接口添加sayNo()方法并实现

```java
@Override
public String sayNo() throws InterruptedException {
    System.out.println("------------sayNo方法调用了一次-----------");
    Thread.sleep(3000);
    return "no";
}
```

**在消费者端决定哪一个方法尝试多少次**

```xml
<!--    相当于@Reference 来注入到容器中-->
    <dubbo:reference interface="com.example.service.HelloService" id="helloService">
        <dubbo:method name="sayHello" retries="3"/>
        <dubbo:method name="sayNo" retries="0"/> 不重试
    </dubbo:reference>
```



直接@autowired注入就行

```java
//不能自动注入autowired
//@Reference//远程去服务方将service的实现类注入进来
@Autowired
private HelloService helloService;
```



消费者调用：

```java
@GetMapping("/hello")
@ResponseBody
public String sayHi(@RequestParam("name")String name){
    return helloService.sayHello(name);
}

@GetMapping("/no")
@ResponseBody
public String sayNo(){
    return helloService.sayNo();
}
```



![image-20210303205056789](../picture/Dubbo%E8%AF%A6%E8%A7%A3/image-20210303205056789.png)







### 4.1.4 多版本



- 一个借口，多个（版本的）实现类，可以使用定义版本的方式引入
- 为HelloService接口定义两个实现类，提供者修改配置



服务接口的**实现类**中设置**版本号**，如果是xml配置文件方式需要指明实现类

![image-20210304100314127](../picture/Dubbo%E8%AF%A6%E8%A7%A3/image-20210304100314127.png)

```java
@Service(version = "1.0.0")
public class HelloServiceImpl implements HelloService {
    @Override
    public String sayHello(String name) throws InterruptedException {
        System.out.println("-------------------调用一次-------------------");
//        Thread.sleep(3000);
        return "hello "+name+"!!!====v1.0.0====";
    }
```



consumer中选择版本号进行调用：

(可以将版本号改为“*” -> 表示随机调用)

```java
<!--    相当于@Reference 来注入到容器中-->
    <dubbo:reference interface="com.example.service.HelloService" id="helloService" version="2.0.0">
        <dubbo:method name="sayHello" retries="3"/>
        <dubbo:method name="sayNo" retries="0"/>
    </dubbo:reference>
```

![image-20210304095858290](../picture/Dubbo%E8%AF%A6%E8%A7%A3/image-20210304095858290.png)









### 4.1.5 本地存根



- 目前我们的分布式架构搭建起来有一个严重的问题，就是好所有的操作全都是 **消费者发起，由服务提供者执行**
- 消费者什么也不干，只管调用；而服务提供者很累。。例如简单的参数验证，消费者完全能够胜任，把合法的参数再发送给提供者执行，效率高了，提供者也没那么累了
- 例如：去房产局办理房屋过户，带好自己的证件和资料，如果什么都不带，那么办理过户的手续会很麻烦，需要先调查你有什么贷款、有没有抵押、复印资料等操作。如果都准备好，很快就可以办完
- **先在消费者处理一些业务逻辑，再调用提供者的过程，就是本地存根**
- 在消费者中，创建一个HElloServiceStub类并实现HelloService接口





- **注意：必须使用构造方法的方式注入**



```xml
<!--    相当于@Reference 来注入到容器中-->
    <dubbo:reference interface="com.example.service.HelloService" id="helloService" version="2.0.0" stub="com.example.stub.HelloServiceStub">
    </dubbo:reference>
```



```java
public class HelloServiceStub implements HelloService {
    //本地存根必须以构造方法的方式注入
    private HelloService helloService;//HelloService的代理对象

    //从容器中获得参数
    public HelloServiceStub(HelloService helloService) {
        this.helloService = helloService;
    }


    @Override
    public String sayHello(String name) {
        if(StringUtils.hasLength(name)){
            return helloService.sayHello(name);
        }
        return "I'm sorry!";
    }

    @Override
    public String sayNo() {
        return helloService.sayNo();
    }
}
```





测试：

![image-20210304102615184](../picture/Dubbo%E8%AF%A6%E8%A7%A3/image-20210304102615184.png)

成功在消费者方预处理数据。











## 4.2 负载均衡策略



- 负载均衡 Load Balance，其实就是将请求分摊到多个操作单元上进行执行，从而共同完成工作任务
- 简单地说，好多台服务器，不能总是让一台服务器干活，应该“雨露均沾”
- dubbo一共提供4‘种策略，缺省为random随机分配调用



![image-20210304103137112](../picture/Dubbo%E8%AF%A6%E8%A7%A3/image-20210304103137112.png)





- **随机**，按权重设置随机概率。
- 在一个截面上碰撞的概率高，但调用量越大分布越均匀，而且按概率使用权重后也比较均匀，有利于动态调整提供者权重。

---

- **轮询**，按公约后的**权重**设置**轮询比率。**
- 存在**慢的提供者累积请求**的问题，比如：第二台机器很慢，但没挂，当请求调到第二台时就卡在那，久而久之，**所有请求都卡在调到第二台上**。

---

- **最少活跃调用数**，相同活跃数的随机，活跃数指调用前后计数差。
- 使**慢的提供者收到更少请求**，因为越慢的提供者的调用前后**计数差会越大，时间长**。

---

- **一致性 Hash**，相同参数的请求总是发到同一提供者。
- 当某一台提供者挂时，原本发往该提供者的请求，基于虚拟节点，**平摊到其它提供者，不会引起剧烈变动。**
- 算法参见：http://en.wikipedia.org/wiki/Consistent_hashing
- 缺省只对第一个参数 Hash，如果要修改，请配置 `<dubbo:parameter key="hash.arguments" value="0,1" />`
- 缺省用 160 份虚拟节点，如果要修改，请配置 `<dubbo:parameter key="hash.nodes" value="320" /`



能够尽可能小地改变已存在的服务请求与处理请求服务器之间的映射关系







---



修改提供者配置并启动3个提供者，让消费者对其进行访问



- tomcat端口8001,8002,8003
- provider端口 20881,20882,20883

```xml
<dubbo:provider timeout="2000" port="20881"/>
```

修改springboot配置，打成3个jar包：

![image-20210304110146387](../picture/Dubbo%E8%AF%A6%E8%A7%A3/image-20210304110146387.png)

在控制台运行：

**可以在dubbo-管理中看到一个服务HelloService’具有三个提供者，也就是三台服务器提供同一个服务**

![image-20210304110138304](../picture/Dubbo%E8%AF%A6%E8%A7%A3/image-20210304110138304.png)





默认基本能做到负载均衡：

![image-20210304110954021](../picture/Dubbo%E8%AF%A6%E8%A7%A3/image-20210304110954021.png)









---

轮询：

```
<dubbo:reference loadbalance="roundrobin" interface="com.example.service.HelloService" id="helloService" version="2.0.0" stub="com.example.stub.HelloServiceStub">
```

按照1-2-3的顺序来访问



---



权重：(**记得关闭轮询策略**)

**在控制台进行权重分配：**

![image-20210304112218285](../picture/Dubbo%E8%AF%A6%E8%A7%A3/image-20210304112218285.png)

改变权重：

![image-20210304112255492](../picture/Dubbo%E8%AF%A6%E8%A7%A3/image-20210304112255492.png)



测试成功。







## 4.3 高可用





### 4.3.1 zookeeper宕机



zookeeper注册中心宕机，还可以消费dubbo暴露的服务

- 监控中心宕掉不影响使用，只是丢失部分采样数据
- 数据库宕掉后，注册中心仍能通过缓存**提供服务列表查询**，**但不能注册-新-服务**
- 注册中心对等集群，任意一台宕掉后，将自动切换到另一台（zookeeper集群，宕掉半数前都可运行）
- **注册中心全部宕掉后，服务提供者和服务消费者仍能通过==本地缓存==通讯**
- 服务提供者无状态，任意一台宕掉后，不影响使用
- 服务提供者**全部宕掉**后，服务消费者应用**将无法使用**，并无限次重连等待服务提供者恢复



---

在服务端暴露一个**ip+端口**给消费者来调用，而zookeeper集群正是用来保存这些地址的。

如果zookeeper宕掉，可以通过缓存查询。**只是不能重新注册新服务了(没有注册中心了)**

如果服务端也宕掉了，那么就相当于只有地址，但这个地址没有任何服务，无法访问到服务。**只好重连等待**

![image-20210304115106482](../picture/Dubbo%E8%AF%A6%E8%A7%A3/image-20210304115106482.png)



---

测试：

- 正常发出请求

- 关闭zookeeper： `./zkServer.sh stop`

  ![image-20210304115403362](../picture/Dubbo%E8%AF%A6%E8%A7%A3/image-20210304115403362.png)

- 消费者仍然可以正常消费

![image-20210304115411912](../picture/Dubbo%E8%AF%A6%E8%A7%A3/image-20210304115411912.png)













## 4.4 服务降级



- 庇护遇到危险会自己脱落尾巴，目的是丧尸不重要的东西，保住重要的
- 服务降级，就是**根据实际的情况和流量**，对一些服务有策略的**停止**或换种**简单**的方式处理，从而**释放服务器的资源来保证核心业务的正常运行**





### 4.4.1 为什么要服务降级



为什么要使用服务降级，这是防止分布式服务发生 **雪崩效应**

- 雪崩：就是蝴蝶效应，当一个请求发生超时，一直等待着服务响应，那么在高并发情况下，很多请求都是因为这样**一直等着响应**，直到**服务资源耗尽-产生宕机**，而宕机之后导致分布式其他服务**调用该宕机的服务**也会出现资源耗尽**宕机**，这样下去将导致**整个分布式服务都瘫痪**，这就是"雪崩"



- 舍弃没用的服务，来换取其他更重要服务的正常/平稳执行





### 4.4.2 服务降级实现方式



- 第一种，在 **管理控制台配置服务降级：屏蔽和容错**

  - **屏蔽：**（force）表示**消费方**对该服务的方法调用都 直接返回null值，**不发起远程调用**。用来**屏蔽不重要的服务不可用时**对**调用方**的影响

  

  - **容错：**（fail）表示消费方对该服务的方法调用在 **失败后，再返回null值，**不抛异常，用来容忍不重要服务不稳定时对调用方的影响



**在消费者端进行屏蔽，直接返回null**

![image-20210304122534772](../picture/Dubbo%E8%AF%A6%E8%A7%A3/image-20210304122534772.png)



容错：

![image-20210304122838518](../picture/Dubbo%E8%AF%A6%E8%A7%A3/image-20210304122838518.png)

















































































































































































































# 坑



## 究极大坑之  获得的service是null







![image-20210303151121122](../picture/Dubbo%E8%AF%A6%E8%A7%A3/image-20210303151121122.png)



Zookeeper节点中：

![image-20210303151304884](../picture/Dubbo%E8%AF%A6%E8%A7%A3/image-20210303151304884.png)



**问题就出在这里！！一定要使用相同的包结构，接口名称**，才可以找到对应的Service



下图是注册服务提供方和注册消费者后，需要保持一个名称才可以

![image-20210303170649530](../picture/Dubbo%E8%AF%A6%E8%A7%A3/image-20210303170649530.png)





> **消费接口要和提供方接口一样，包名，方法名也要一样**















