在高并发、大流量的系统架构中，MQ（Message Queue）消息队列的应用是非常广泛的。从读写分离到服务之间的解耦，从高并发的削峰到热点数据的请求，MQ 的应用是无处不在。

RocketMQ 作为一款分布式的消息中间件，经历了 Metaq1.x、Metaq2.x 的发展和淘宝双十一的洗礼，成为市面上比较流行的 MQ 产品，被各大厂商争相采用。



# 项目架构





## 功能结构图



本项目主要针对“互联网酒店预约系统”开发的，其中酒店的管理人员可以在“酒店管理后台”进行酒店房间的管理，如图1 所示，例如：添加、修改房间。

![image-20210511115809100](../picture/基于RocketMQ的互联网酒店预订系统项目/image-20210511115809100.png)





如图 2 所示，后台管理人员可以通过订单管理列表看到用户的订单信息，从而进行入住和退房操作。

![image-20210511115828705](../picture/基于RocketMQ的互联网酒店预订系统项目/image-20210511115828705.png)



总体而言“酒店管理后台”主要是给酒店的管理人员使用的，包括：**分店管理，房间管理，订单管理，优惠券管理等部分**。

其次，客人可以通过“酒店小程序”进行酒店房间的查询，接着预定、下单、查看优惠卷等动作。如图3 所示，客人可以通过小程序查询酒店，根据列表中显示的酒店，选择位置和价格都适中的，并且针对合适的房型进行预订。

![image-20210511132130905](../picture/基于RocketMQ的互联网酒店预订系统项目/image-20210511132130905.png)



![image-20210511132138155](../picture/基于RocketMQ的互联网酒店预订系统项目/image-20210511132138155.png)



![image-20210511132146736](../picture/基于RocketMQ的互联网酒店预订系统项目/image-20210511132146736.png)



**“酒店小程序“负责提供给客人查找和预订等功能，而“酒店管理后台”负责房型的管理和订单相关操作。**



互联网酒店预订系统的内容分为两块，分别是上面桔红色的“酒店小程序”，这部分是提供给客人使用的。主要功能包括：登录、查看优惠券、预订下单、支付订单、取消订单。

这些功能涵盖了预订房间的整个生命周期，其操作顺序是：用户“登录”，“搜索分店”，“选择分店”、“查看房型”、“选择房型”、“预定下单”，以及“支付订单“或者“取消订单”

与此同时，用户登录以后可以在“优惠券管理”中查看“优惠券”，“我的订单”中查看“待支付订单“和”取消订单“

下面的绿色部分是“酒店管理后台”，主要是给酒店管理人员使用的，其包括：添加房型、修改房型、确认订单（入住）以及结束订单（退房）

这些功能基本上可以总结为房型管理和订单管理。其操作流程是：在“分店管理”中添加分店，在“房间管理”中给指定分店添加房间。在“订单管理”中针对具体订单进行“入住”或者“退房”操作。也就是这些后台操作与小程序中的功能相映成趣，完成整个订单流程。

![image-20210511132407486](../picture/基于RocketMQ的互联网酒店预订系统项目/image-20210511132407486.png)





## 业务流程



### 用户登录



用户通过小程序进行登录，这里会通过**微信授权登录方法**来调用后端登录接口登录，同时也会有**手机号**的授权



如图1所示，在登录“酒店小程序”的时候，会请求“小程序后台”判断**是否“第一次登录？“：如果“是”，会给用户发放优惠券。同时用户会在”我的优惠券“中找到对应的优惠券信息。如果不是第一次登录，那么不会走“发放优惠券”的流程。**

无论是否“第一次登录”，都会在“小程序后台”通过**“验证登录信息”**，返回给客户登录的结果。客户在获得登录结果以后，登录系统或者由于后端服务异常，返回登陆失败（微信授权失败）。

<img src="../picture/基于RocketMQ的互联网酒店预订系统项目/image-20210511133731201.png" alt="image-20210511133731201" style="zoom:50%;" />





### 搜索房型以及下单



用户在登录以后会进行哪些业务流程呢？如图2所示，泳道的从左到右依次列出了**“酒店小程序”和“酒店管理后台”**，从上往下流出了“客户”和“酒店管理人员”。可以方便了解，在不同流程的参与者和操作的平台。

![image-20210511135648121](../picture/基于RocketMQ的互联网酒店预订系统项目/image-20210511135648121.png)



有部分配置信息是“酒店管理员”事先在“酒店管理后台”配置好的，这里用黄色表示，同时在执行流程过程中如果**需要调用这部分信息**时，会通过蓝色的箭头表示。

------

我们顺着箭头从上往下来看流程，先从“查询酒店”开始。客户登录“酒店小程序”以后，会通过时间范围等搜索条件**查询酒店信息**，在此基础上“**选择分店**”，**“分店信息”是由酒店管理者在“酒店管理后台”事先配置好的**。

针对每个分店有对应的房型信息**“选择房型”**，同样该信息也是配置好的，**直接获取**就好了。之后，通过**“预订”功能“填写入住信息”**，最后通过**“付款”**完成交易。

需要注意的是，付款的时候会**获取优惠券的信息**，还记得在登录时发放的优惠券吗？在这里就派上用场了。到这里客户在小程序上面操作的流程就完了。用户可以在**小程序的“我的”中实时查看订单的状态**。接下来就轮到**“酒店管理人员”上场了。**

客户付款以后会“记录预订”，以订单的形式，让“酒店管理人员”在**“查询订单信息”中找到对应的订单**。在根据对应的订单进行“确定入住”操作，也就是当客户到达酒店以后进行Check in的时候执行。

当然，客户离开酒店的时候，我们的管理人员也可以执行**“确定退房”的操作**。上述的这些操作都是在**酒店管理后台完成**。整个**订单的状态变更可以在小程序上看到。**





## 逻辑架构

有了业务的指导后再**根据业务进行系统的搭建**，逻辑架构图就是**根据业务流程和功能实现技术架构的基础**。逻辑架构就是对功能的技术抽象，告诉架构师为了实现这些业务功能需要用到哪些技术，例如：**客户端、网关、服务、存储等等。**

有了功能和流程形成了业务的骨架，这个骨架需要落地成为“活生生的人”就需要进行逻辑设计。



逻辑设计就是设计人的血肉，在原来的骨架上进行实体的填充，包括使用到的技术，功能模块的划分。在逻辑架构中**可以不提及具体的实现技术工具，但是要对技术要点进行分析**，也就是“造人”的可行性分析。这里一起来看看架构图，如图1 所示，我们将本系统分为四层，从上往下分别是“访问层”、“接入层”、“服务层”、“数据层”。

从上往下也是用户请求的方向，用户请求从“访问层”接触到系统，请求通过“接入层”访问到具体的业务服务。业务服务在**“服务层”中提供“访问层”需要的服务，并且对服务进行模块划分**。最后数据的持久化和缓存需要数据层提供。

![逻辑架构](../picture/基于RocketMQ的互联网酒店预订系统项目/klj2zuub0skn.png)



---

- 访问层

用户会通过这层提供的应用访问到整个系统，这里会继承系统中功能的访问接口。由于本系统有两类用户，分别是“客户”和“酒店管理人员”，因此这里提供了两个访问的入口，分别是**“预订客户端”（“酒店小程序”）和“酒店管理后台”。**

之所以这里不直接说小程序和Web管理网站，是因为在这一层需要做**抽象**。根据具体的使用场景不同，小程序可以被Android 、IOS所替代；管理后台也有可能需要接入其他系统或者使用其他技术。这也是**逻辑层存在的价值**，帮助大家使用**抽象思维架构系统**。



---

- 接入层

有了访问层，**请求会通过HTTP/HTTPS请求进入到系统的内部**。在进入之前会经过访问层，由于我们的系统有可能暴露在公网环境下，而内部的服务和公网用户访问需要有一个**缓冲或者保护机制**。

这里使用**反向代理**的方式，将**外部的请求代理到内部的服务中**，**对流量进行了管理、也隔离了网络**。同时使用网关的机制，也可**以对请求进行过滤和鉴权**。例如：用户**访问权限、重复发送请求、恶意请求、大流量控制**都可以在这一层完成。



----

- 服务层

这个是系统的大头，基本所有的服务都是在这里提供。从图中绿色的部分可以看到包括三个紫色的框图。将服务分为了三类：**基础信息管理、订单管理和系统管理。**

基础信息管理，主要包括**分店信息、房型信息、优惠券信息**这些信息属于系统基础信息，系统正常运行会依赖这部分的信息，在系统构建之后需要第一时间建立。

订单管理包括系统中的**主要业务信息**，包括：订单预订、订单付款、确认入住、确认退房，也是业务流程中的主要操作。

系统管理中的**用户登录和消息推送**，属于系统级别的功能和业务可以解耦。



---

- 数据层

为了实现数据的持久化，这一层通常用来存储业务数据。同时在必要的时候还会为系统提供缓存服务。







## 技术架构



![服务端技术架构](../picture/基于RocketMQ的互联网酒店预订系统项目/klj3zqpq0vha.png)



---

- 访问层

使用了微信小程序和H5的方式提供给“客户”和“酒店管理人员”进行操作。这里的技术选型和使用场景相关。

“客户”通常预订房间多是在旅途中，**使用移动终端的场景会多些**，由于微信的普及率较广，使用小程序作为入口推广成本较低。同样，“酒店管理人员”移动办公需求较少，同时还**需要处理各种表格数据，PC端使用H5的方式是不错的选择，符合使用场景匹配的原则。**



---

- 接入层

是连接客户和服务的纽带，这里使用了**Nginx 作为反向代理**，反向代理不仅使**外网和内网进行了有效的隔离**，同时进行了IP的转化。

Nginx 结合OpenResty的脚本功能还可以和缓存队列等技术有机结合，而且为以后的水平扩展也提供了可能性。启用“儒猿自研网关”**对请求进行过滤和鉴权**，针对**流量限制**，**用户权限控制**等功能可以放在这里完成。



---

- 服务层

服务层内容比较多，先从下面四个独立的模块开始**。Spring Boot 作为脚手架搭建基本服务架构**，理清组件包之间的依赖关系，快速搭建后台应用程序。**MyBatis 作为数据库访问**的利器，支持ORM和手动SQL的方式对数据库进行操作。

**Spring Web MVC 是提供RESTFUL API**的主力军，其网络请求的处理能力使它成为本架构的不二之选。

- 最后是**Elastic Search ，其将数据库中的信息转化为索引文件**，提供**高效的数据检索功能**，非常适合酒店搜索功能。在四个模块的右边有三个竖条分别是RocketMQ、Redis 和Nacos。

- RocketMQ提供了服务之间**消息流转的通道**，将服务之间关系进行**解耦**，让架构真正做到**高内聚、低耦合。**

- Redis 作为**进程外缓存**，提高请求的响应时间，增强用户体验。

- Nacos的加入作为**服务之间的注册中心**，不仅理清了服务之间的调用关系，也提供了分布式服务部署的可能性。

接着再看右边粉色的条形，slf4j （Simple Logging Facade for Java）作为日志的利器，**保存运行时的日志信息**，是debug的好帮手。

中间紫色区域的**“业务服务”包括“基础信息管理”、“订单管理”以及“系统管理”三大服务模块**，是系统需要实现的服务主体，作为业务描述放在这里与逻辑架构中的业务部分相对应。

最后是上方的Tomcat 应用服务器，作为应用服务的容器承载服务请求与响应的主要工作。



---

- 数据层

数据层的内容相对简单，MySQL老牌的关系型数据库负责存放客户、酒店、房型、配置等业务信息。**Redis的持久化功能**增加了Redis缓存的可靠性。





## 项目大图





### 修改房间场景





通过用户请求的过程将每个技术组件串联起来



![修改房间场景](../picture/基于RocketMQ的互联网酒店预订系统项目/klj4kcg40wty.png)



修改房间执行流程：

1. “酒店管理人员”通过“酒店管理后台”访问房间详情的配置信息。这里的入口是H5，也就是管理后台的具体实现。
2.  此时请求通过H5请求到系统的入口，作为**反向代理和负载均衡的Nginx**接受到请求会将其转交给“儒猿自研网关”。
3.  在“儒猿自研网关”可以进行**用户验证、流量限制**等工作，最终通过服务层请求对应的服务。
4. 由于**每个服务都是Host在Tomcat之上**的，这里会**通过Tomcat请求每个服务**。这里的Tomcat对应的是“酒店管理后台”，需要与后面提到的**小程序后台Tomcat**作区分。
5. 在请求服务之前会经过Spring Web MVC**对HTTP请求进行解析，转化为请求对象**。
6. 将解析请求以后会调用“基础信息管理“中的”房间详情“服务，并且根据请求内容进行房间信息的修改。
7.  修改信息通过MyBatis 更新到MySQL中完成数据的更新。
8. 之后再将更新的房间信息**发送给Redis缓存**起来，如果此时有请求访问时就不用从数据库中获取了。
9.  接下来就是通过**RocketMQ队列分发修改房间信息的消息**，并且**通知订阅该消息的其他服务。**
10. 订阅消息的服务获取**消息通知**以后，会**更新服务本身JVM的缓存**，同样也是为了提高用户的访问效率。
11. 最后，将这一连串的操作记录成日志存档。





### 查看房间场景



查询操作实际上与上面的修改操作形成对比，刚才保存的数据也就是为这里查询使用的。

如图2所示，系统架构和技术组件都没有改变，不同的是业务的流程和请求的路径发生变化。



![查看房间详情](../picture/基于RocketMQ的互联网酒店预订系统项目/klj4ng5q0w0f.png)

**查询房间执行流程 **

1. “客户”通过微信小程序访问酒店预订系统，此时小程序作为系统的入口。
2.  访问接入层的Nginx。
3.  通过“儒猿自研网关”的**过滤**。
4.  这里访问的是**小程序后台的Tomcat**，其**承载着小程序需要的服务API**。
5.  依旧是通过Spring Web MVC访问具体的服务。
6.  房间信息存放在基础信息管理服务中，从这里开始流程会有所不同。
7. 服务在获取用户请求的时候，会**先去检查本地JVM缓存中是否有房间详情的信息，如果有的话将会直接返回给客户。**
8. 如果在JVM缓存中不存在房间详情，**会从Redis 缓存中获取**。
9. 如果Redis缓存中依旧不存在该信息，才**回源到MySQL数据库**，同样也是使用MyBatis来访问。
10. 完成查询操作以后依旧会记录日志归档。







# 项目概述







## 基础服务

通过SpringCloud Alibaba 创建了项目

通用功能的使用，例如：数据库的访问、缓存使用和消息发送。这些功能作为课程的基础，辅助我们实现RocketMQ的应用场景，但比起RocketMQ而言并不是核心功能。

介绍下这几个组件：

-  little-project-common

主要封装了请求的响应体，以及枚举响应码的信息。在Controller中完成请求返回的时候经常被用到。

CommonResponse就是出自于“little-project-common”组件，作为标准的响应返回体。

- little-project-mysql-api

![image-20210512104809941](../picture/基于RocketMQ的互联网酒店预订系统项目/image-20210512104809941.png)

little-project-mysql-api”组件作为**访问MySQL数据库的API**，是一个**dubbo服务**，它在获取数据库信息的时候会被用到。

在需要访问数据库之前先**定义MysqlApi类型的变量**，使用MysqlApi 中的Query方法，传入查询的DTO对象，从而获取返回的结果。

- little-project-redis-api

![image-20210512104824180](../picture/基于RocketMQ的互联网酒店预订系统项目/image-20210512104824180.png)

little-project-redis-api”是用来访问Redis的组件，也是一个**dubbo服务**，用来对Redis缓存进行操作。

- little-project-message-api

little-project-message-api”组件是微信推送消息的API，也是**dubbo组件**，在**完成某些业务场景**的时候会调用其**进行微信消息的推送**。

通过调用WxSubscribeMessageApi 中的send方法**负责微信消息推送**，其输入参数是包含推送消息的DTO对象。

将订单的微信消息的推送封装到抽象类中，其中针对“WxSubscribeMessageApi”类声明变量，用来消息发送。





## 整体划分



### 后台系统的划分



整个系统由酒店小程序、酒店管理后台，这**两套系统分别对应两个后台的服务**，它们分别是：小程序后台API和酒店管理后台API。

如图所示，上方虚线框部分已经有儒猿团队提供给大家了，作为学员我们需要关注下方实线框的部分，**小程序后台API和酒店后台管理API是这次课程的主体**，我们的大部分代码都在这里完成。

后台的结构划分：

<img src="../picture/基于RocketMQ的互联网酒店预订系统项目/image-20210512105126951.png" alt="image-20210512105126951" style="zoom: 50%;" />

### 代码模块的划分



从业务出发我们需要将每个业务划分为不同的模块，每个模块对应一个或者多个服务从而支撑模块的运行

> 实际工作中这些服务有可能分开开发和部署，分别部署为多个微服务

我们在liitle-project-rocketmq项目中建立了admin和api两个包，**admin用来为酒店后台管理提供API服务**，而  **api 为酒店小程序提供API服务**。其中api包中分别包含如下几个包：

- coupon：包含优惠券相关的DTO、服务、Listener以及MQ消费者。
- hotel：包含房间相关的DTO、服务、Controller、Listener以及MQ消费者。
- login：包含登录相关的DTO、服务、Controller、Listener 、枚举状态、MQ生产者以及MQ消费者。
- message：包含消息推送相关的DTO、Listener 、消息命令以及MQ消费者。
- order：包含订单相关的DTO、服务、Controller、Listener 、枚举状态、MQ生产者以及MQ消费者。
- pay：包含消息支付相关的DTO、常量定义、Controller、服务。







# 部署服务到RocketMQ集群





## 打包服务



分配了对应的服务器，那么就可以讲我们前几节课中提到的应用服务部署上去了。

输入“mvn clean package” 的命令，这里表明通过Maven进行打包，打包的是生产环境（prod）。







## 上传服务



在上传服务（jar包）之前需要保证，在对应的ECS中建立服务运行的目录如下：

/home/admin/little-project-rocketmq/target





其中“/home/admin/little-project-rocketmq”是用来存放部署脚本的，注意我们这里使用的deploy.sh 脚本已经由儒猿团队上传了。

“/home/admin/little-project-rocketmq/target”目录是存放服务的jar包的，这个jar包是需要我们自己上传的。

在ECS上建立好目录结构以后，再回到本地的项目中，依旧是在IntelliJ IDEA的命令行中输入以下命令：

```
scp target/little-project-rocketmq.jar root@47.102.130.27:/home/admin/littl
e-project-rocketmq/target

命令的意思是通过scp命令将刚才打包的“little-project-rocketmq.jar”文件copy 到对应ECS服务器的“/home/admin/little-project-rocketmq/target”目录中。
```

这里需要特别说明一下，由于我在执行命令的根目录在“little-project-rocketmq”项目下面，如果你在其他的地方执行上述两条命令，需要指定好源文件的目录。

同时在使用scp命令以后会让大家输入服务器的密码，该密码可以从实战云平台上获取。

完成上面两个命令以后，服务就已经部署到ECS了。



## 启动服务



完成部署以后，需要到ECS 服务器上面启动部署的酒店管理后台的服务。还是登录ECS服务器，通过以下命令进入到“deploy.sh”文件所在的目录。

```
cd /home/admin/little-project-rocketmq
```



deploy.sh 文件是儒猿团队为大家生成的发布的脚本文件，通过Linux shell脚本完成发布的参数配置和启动命令。

包括启动应用等待的时间、应用端口号、健康检查的URL以及jar包的目录和日志信息。有兴趣的同学可以打开看看，这里就不做展开的介绍了。

保证当前目录下面存在“deploy.sh”文件，使用如下命令启动服务。

```
sh deploy.sh restart
```

命令使用了“restart”作用与“start”是一致的，用“restart”的目的以免在重复发布过程中，学员忘记是否启动过服务。因此使用“restart”，这样即便是已经启动过服务，也会重新加载服务。

另外，deploy.sh中已经加了自动化脚本，用来防止用户未指定成prod环境和更改rocketmq.namesrv.address地址的配置，默认了prod环境和当前ECS的地址:9876为namesrv的地址。这些大家只需要了解就可以了，不用做任何更改。

运行命令在看到所示的“success”字样的时候，就说明服务启动成功了。

![image-20210512134755192](../picture/基于RocketMQ的互联网酒店预订系统项目/image-20210512134755192.png)





## RocketMQ   Console的功能介绍



不过RocketMQ的服务是儒猿团队已经安装到ECS服务器上的，我们可以通过如下方式访问：

http://47.102.130.27:8080/



这里是RocketMQ的控制台，包括日常的OPS信息、集群（Cluster）、Topic、Consumer、Producer、Message的信息

![image-20210512135115116](../picture/基于RocketMQ的互联网酒店预订系统项目/image-20210512135115116.png)



# 数据的初始化





- 基础门店数据：

客户需要选择酒店以后在指定房间，随后才能完成预定的操作。其中酒店和房间的基本信息需要事先在“酒店管理后台”配置好，不过针对酒店的信息由于和主体的业务操作耦合没有那么紧密，已经由儒猿团队为大家配置好了。

如图1 所示，儒猿团队已经在后台配置好了三家酒店，分别是“儒猿酒店成都分店”、“儒猿酒店深圳分店”和“儒猿酒店北京分店”，其基本信息如图中所示。这部分的配置功能就不给大家开放了，也让大家把更多精力放在核心业务上面。

![image-20210512135838382](../picture/基于RocketMQ的互联网酒店预订系统项目/image-20210512135838382.png)



----



- 添加房间数据：

上面的酒店分店数据不用我们自己添加，儒猿平台已经帮我们加好了。房间的数据会作为我们一个实战的场景，所以对应的功能是对我们学员来开放的，这里我们先初始化一下房间数据，后面在进行详细的介绍。

如图2 所示，回到小实战平台，在“房间管理”中会显示所有添加房间的列表。点击右边的按钮，选择“新增房间”就可以初始化房间的数据了。

![image-20210512135912042](../picture/基于RocketMQ的互联网酒店预订系统项目/image-20210512135912042.png)

![image-20210512140017438](../picture/基于RocketMQ的互联网酒店预订系统项目/image-20210512140017438.png)







# 登录逻辑









## 登录逻辑的业务流程





登录酒店小程序的业务逻辑：



客户通过“酒店小程序”进行登录，此时会将请求提交给“小程序后台”，后台在判断是否是第一次登录以后，会出现两个流程分支。

如果是第一次登录：会**给对应的客户发放优惠券**，同时客户可以在小程序中**查询到优惠券**的信息。

如果不是第一次登录：跳过发放优惠券的流程进入下面**验证登录信息**的处理。

无论是否是第一次登录，最后流程都会**对用户进行登录验证的操作**，最后返回登录结果给客户。

![登录酒店小程序的业务流程](../picture/基于RocketMQ的互联网酒店预订系统项目/klkb56lk01mj.png)



有三件事是需要做的：

- 判断**是否第一次登录**：这个好理解，客户每次登录都**在数据库中记录一个状态**，下次登录的时候**与这个状态进行比较**就可以知道是否第一次登录。如果更加细致一点其实是两个操作：***检查登录状态和更新登录状态*** 。
- 发放优惠券：可以**单独作为一个功能存在**，就是向用户发放优惠券，这里使用的是**同步调用**。在这个场景中是第一次登录就发放。当然这个功能也可以用到其他场景中，例如：消费多少金额，发放优惠券。为了能够将此功能解耦，会单独将其提出来。
- 验证登录信息：由于大家是使用微信登录系统，因此会**封装微信登录的模块**。由于小程序的部分功能使用开源的PHP完成，因此这部分代码不需要大家做修改，已经由系统完成。我们可以在登录的时候打上日志，方便跟踪和理解这部分功能。





## **微信登录**





### 小程序登录授权



小程序登录授权的流程包括三个步骤，分别是   **登录小程序、获取手机号和解密手机号。**



登录的目的是为了获得登录的凭证，**保证登录小程序的合法性**。 接下来就是手机号授权，由于安全性问题小程序中是无法直接获取学员手机号的，因此需要得到使用者，也就是学员的授权。

授权通过以后，**手机号会通过加密的方式传到后端**，此时有一个解密的过程，由我们的PHP开源代码完成这个过程。并且**把手机号的明文传给服务器端**，服务器**通过手机号来识别对应学员申请的ECS 以及部署的应用**，从而继续后面的实验。 

<img src="../picture/基于RocketMQ的互联网酒店预订系统项目/image-20210512145715214.png" alt="image-20210512145715214" style="zoom:50%;" />



登录授权的目的是通过微信官方的登录能力获取微信的用户身份标识，快速建立小程序内的用户体系。



如图 3 所示，这里从开发者的角度描述了小程序授权的全过程。我们顺着箭头的方向从左往右看一下。在获取**用户授权**的时候，小程序会通过**wx.login（）方法获取一个code**，并且把它发往**开发者服务器（我们自己的后端）**，其通过已经申请好的**appid和appsecret** 加上这个code去**向微信接口服务获取对应的session_key和openid。**

同时将返回的信息与openid，session_key 进行管理，返回***自定义登录状态***。

**小程序**拿到自定义登录状态以后**保存在本地的storage**中，在每次发起服务端请求的时候，都会**携带上这个登录态**，开发者服务器也**通过这个登录态去查询用户的openid和session_key**，也就是最开始用户授权时生成的信息。在**通过验证以后返回业务数据**，整个过程保证用户数据传输的安全。

![image-20210512145812031](../picture/基于RocketMQ的互联网酒店预订系统项目/image-20210512145812031.png)



----

- 获取手机号

前面获取了用户的授权，但是不等于能够获得用户的手机号。微信小程序如果要获取用户的手机号，也需要**用户的授权（安全性要求）**。由于手机号会和学员的**ECS IP以及发布的服务相关，这里必须获取，于是就有了获取手机号的部分。**

如图4所示，在小程序授权之后，接着会弹出手机号授权的框，其目的就是为了获取用户的手机号。

<img src="../picture/基于RocketMQ的互联网酒店预订系统项目/image-20210512151302873.png" alt="image-20210512151302873" style="zoom:50%;" />



获取微信用户绑定的手机号，需先调用[wx.login](https://developers.weixin.qq.com/miniprogram/dev/api/open-api/login/wx.login.html)接口。

由于需要**用户主动触发**才能发起获取手机号接口，所以该功能不由 API 来调用，需用 [button](https://developers.weixin.qq.com/miniprogram/dev/component/button.html) 组件的点击来触发。

也就是在图4的“手机号”[button](https://developers.weixin.qq.com/miniprogram/dev/component/button.html) 组件的 open-type 的值设置为 `getPhoneNumber`，当用户点击并同意之后，可以通过 `bindgetphonenumber` 事件回调**获取到微信服务器返回的加密数据**， 然后在第三方服务端**结合 session_key 以及 app_id 进行解密获取手机号**。



---

- 解密手机号

由于获取的手机号为了安全性，本身是进行加密的这一点可以从获取手机号的返回参数看出。如图5所示，在返回参数中包括encryptedData的用户加密信息，以及iv的机密算法的初始向量，还有couldID敏感数据对应的云ID。

![image-20210512151524779](../picture/基于RocketMQ的互联网酒店预订系统项目/image-20210512151524779.png)

尽管返回的手机号是加过密的，但是我们不必担心，因为我们使用的开源PHP程序已经帮助我们处理这些机密信息了。在后台获得小程序请求的时候，我们将看到明文的手机号。





## MQ对登录系统核心流程异步化改造



### 传统登录流程



当用户登录的时候，会判断“是否第一次登录？”当用户满足第一次登录条件以后，后台会在数据库中记录登录的信息，然后发放优惠券。

<img src="../picture/基于RocketMQ的互联网酒店预订系统项目/image-20210512152433616.png" alt="image-20210512152433616" style="zoom:50%;" />

这里注意：**“记录第一次登录信息”和“发放优惠券”是有先后顺序的**，将两个功能放到一个模块里面进行。



> **为了服务的高内聚、低耦合，或者通过分布式提高服务的高并发能力，将模块或者服务切分，如：将登录模块和优惠券模块拆分成两个服务**

服务拆分以后，服务内部保证实现对应的功能，服务的执行先后顺序可以根据业务需求进行调整。例如：可以在“记录第一次登录信息”的同时就“发放优惠券”，**不用等待记录成功才进行优惠券的发放**。让系统效率最优，同时让更多的服务之间可以进行组合。





### 异步化登录流程



登录完毕更新状态再发送优惠券，同步的方式本身就带来性能问题。为了提升登录接口的性能，同时兼顾服务的高内聚、低耦合，分布式的要求，就有了异步化的登录流程。



我们将原来的一个小程序后台拆分成两个服务，分别是**登录服务和优惠券服务**，在图中用红色字体表现出来。注意：这里的服务分割是逻辑上的，为了整个项目方便操作和演示，我们还是将两个服务放到了一个项目中，只是通过包名来区分服务。



登录服务包含了`LoginService（登录服务）`，其主要负责**判断**第一次登录以及记录第一次登录的**信息**。在完成这些操作之后会通过`LoginProducer（登录消息生产者）`发送登录的消息。

登录消息会被发送到RocketMQ的消息队列，这里RocketMQ就充当了**服务解耦的中介者**。在`CouponCunsumer（优惠券消息消费者）`这边，会**订阅登录的消息**并且获取对应的消息，进行后续优惠券的发放。

> 这样在向消息队列发送了登录消息后，优惠券服务在收到订阅的消息后，异步发送优惠券，无需等待登录成功响应返回再去同步发出优惠券

![image-20210512154806916](../picture/基于RocketMQ的互联网酒店预订系统项目/image-20210512154806916.png)







## **基于MQ对登录系统核心流程进行异步化改造**



### 会员数据库表结构

首先要定义会员的数据库表结构，和其他的表定义一样，我们这里将定义展示如图1所示，这里不逐一给大家做介绍，因为这部分的表已经在数据库后台由儒猿团队为大家建立好了。

会员数据库表结构：

![table1](../picture/基于RocketMQ的互联网酒店预订系统项目/klkhchhu0qkt.png)



![table2](../picture/基于RocketMQ的互联网酒店预订系统项目/klkhchhr0jza.png)



<img src="../picture/基于RocketMQ的互联网酒店预订系统项目/image-20210512155745246.png" alt="image-20210512155745246" style="zoom:50%;" />



----



### 登录RocketMQ参数定义

我们要使用RocketMQ的功能发送消息，需要对其的基本参数进行定义，

这里**定义了RocketMQ topic、producer和consumer的group参数**，同时也定义了RocketMQ的namesrv的地址。

需要注意的是，这个服务器的地址不需要大家手动修改了，在部署项目的时候已经在deploy.sh 文件中自动为大家做修改了。这里配置出来是让大家知道，RocketMQ服务器的地址也是必填项。 

![image-20210512155851215](../picture/基于RocketMQ的互联网酒店预订系统项目/image-20210512155851215.png)



```properties
# 登录消息的topic
rocketmq.login.topic=login_notify_topic
rocketmq.login.producer.group=login_notify_producer_group
rocketmq.login.consumer.group=login_notify_consumer_group

# 第一次登陆下发的登录优惠券id
first.login.couponId=738
# 优惠券有效时间30天
first.login.coupon.day=30
# 当前部署的服务器的外网ip地址 每次重新部署时修改  这里在deploy.sh文件中已经自动做了修改，不需要手动配置
# rocket.namesrv.address=xxxxxx
```



----



### 登录消息生产者





```java
//用来启动loginMqProducer，也就是登陆消息的生产者，启动producer以后就能通过这个生产者在需要的时候发送MQ消息了
//这里面的loginMqProducer的MQ的地址和组 已经配置完成，使用的时候可以直接注入然后发送消息即可
@Configuration
public class LoginProducerConfiguration {

    //RocketMQ服务器的地址，会去读配置文件
    @Value("${rocket.namesrv.address}")
    private String namesrvAddress;

    //MQ生产者（Producer）的组（Group）
    @Value("${rocketmq.login.producer.group}")
    private String loginProducerGroup;

    //它被注册成为一个Java Bean
    @Bean(value = "loginMqProducer")
    public DefaultMQProducer loginMqProducer() throws MQClientException {
        //传入当前producer所属的group
        DefaultMQProducer producer = new DefaultMQProducer(loginProducerGroup);
        producer.setNamesrvAddr(namesrvAddress);
        producer.start();
        return producer;
    }
}
```





### 登录服务(LoginService)





```java
//登录接口service组件
public interface LoginService {

    /**
     * 第一次登录需要分发优惠券
     * @param loginRequestDTO 登录信息
     */
    void firstLoginDistributeCoupon(LoginRequestDTO loginRequestDTO);

    /**
     * 调用这个方法可以将用户的第一次登陆状态重置，主要是方便我们反复测试“第一次登录”的功能。
     * @param phoneNumber 手机号
     */
    void resetFirstLoginStatus(String phoneNumber);

}
```





登录核心业务：

```java
@Service
public class LoginServiceImpl implements LoginService {


    //日志组件
    private static final Logger LOGGER = LoggerFactory.getLogger(LoginServiceImpl.class);

    @Autowired
    @Qualifier(value = "loginMqProducer")
    private DefaultMQProducer loginMqProducer;

    @Value("${rocketmq.login.topic}")
    private String loginTopic;

    /**
     * mysql dubbo api接口
     */
    @Reference(version = "1.0.0",
            interfaceClass = MysqlApi.class,
            cluster = "failfast")
    private MysqlApi mysqlApi;

    /**
     * redis dubbo服务
     */
    @Reference(version = "1.0.0",
            interfaceClass = RedisApi.class,
            cluster = "failfast")
    private RedisApi redisApi;


    @Override
    public void firstLoginDistributeCoupon(LoginRequestDTO loginRequestDTO) {

        //不是第一次登录，返回
        if(!isFirstLogin(loginRequestDTO)){
            LOGGER.info("userid:{} not first login", loginRequestDTO.getUserId());
            return;
        }
        //是第一次登录：

        //更新第一次登录的标识位，根据手机号更新 登录状态为 "已登录过"
        this.updateFirstLoginStatus(loginRequestDTO.getPhoneNumber(), FirstLoginStatusEnum.NO);
        //发送第一次登录成功的消息
        this.sendFirstLoginMessage(loginRequestDTO);
    }

    /**
     * 校验是否是第一次登陆
     *
     * @param loginRequestDTO 登陆信息
     * @return
     */
    private boolean isFirstLogin(LoginRequestDTO loginRequestDTO) {
        MysqlRequestDTO mysqlRequestDTO = new MysqlRequestDTO();
        mysqlRequestDTO.setSql("select first_login_status from t_member where id = ? ");
        ArrayList<Object> params = new ArrayList<>();
        params.add(loginRequestDTO.getUserId());
        mysqlRequestDTO.setParams(params);
        mysqlRequestDTO.setPhoneNumber(loginRequestDTO.getPhoneNumber());
        mysqlRequestDTO.setProjectTypeEnum(LittleProjectTypeEnum.ROCKETMQ);

        LOGGER.info("start query first login status param:{}", JSON.toJSONString(mysqlRequestDTO));
        //远程调用mysqlapi查询结果
        CommonResponse<List<Map<String, Object>>> response = mysqlApi.query(mysqlRequestDTO);
        LOGGER.info("end query first login status param:{}, response:{}", JSON.toJSONString(mysqlRequestDTO), JSON.toJSONString(response));
        if (Objects.equals(response.getCode(), ErrorCodeEnum.SUCCESS.getCode())
                && !CollectionUtils.isEmpty(response.getData())) {
            Map<String, Object> map = response.getData().get(0);
            //判断登录状态是否是 "未登陆过"
            return Objects.equals(Integer.valueOf(String.valueOf(map.get("first_login_status"))),
                    FirstLoginStatusEnum.YES.getStatus());
        }
        return false;
    }

    /**
     * 更新第一次登陆的标志位
     *
     * @param phoneNumber          用户手机号
     * @param firstLoginStatusEnum 登录状态 {@link FirstLoginStatusEnum}
     */
    private void updateFirstLoginStatus(String phoneNumber, FirstLoginStatusEnum firstLoginStatusEnum) {
        MysqlRequestDTO mysqlRequestDTO = new MysqlRequestDTO();
        mysqlRequestDTO.setSql("update t_member set first_login_status = ? WHERE beid = 1563 and mobile = ?");
        ArrayList<Object> params = new ArrayList<>();
        params.add(firstLoginStatusEnum.getStatus());
        params.add(phoneNumber);
        mysqlRequestDTO.setParams(params);
        mysqlRequestDTO.setPhoneNumber(phoneNumber);
        mysqlRequestDTO.setProjectTypeEnum(LittleProjectTypeEnum.ROCKETMQ);

        LOGGER.info("start query first login status param:{}", JSON.toJSONString(mysqlRequestDTO));
        CommonResponse<Integer> response = mysqlApi.update(mysqlRequestDTO);
        LOGGER.info("end query first login status param:{}, response:{}", JSON.toJSONString(mysqlRequestDTO), JSON.toJSONString(response));
    }



    /**
     * 调用login的producer 往RocketMQ的队列中发送登录消息
     * @param loginRequestDTO 登录请求信息
     */
    private void sendFirstLoginMessage(LoginRequestDTO loginRequestDTO){

        //场景一：性能提升   异步发送一个登录成功的消息到mq中
        //定义了RocketMQ Message的实体，并且设置了对应的topic（loginTopic）
        Message message = new Message();
        message.setTopic(loginTopic);

        //消息内容用户id，需要发送字节流数组
        message.setBody(JSON.toJSONString(loginRequestDTO).getBytes(StandardCharsets.UTF_8));
        try {
            LOGGER.info("start send login success notify message");
            //使用生产者对象发送消息,等待下游服务中的consumer进行消费
            SendResult sendResult = loginMqProducer.send(message);
            LOGGER.info("end send login success notify message, sendResult:{}", JSON.toJSONString(sendResult));
        }catch (Exception e){
            LOGGER.error("send login success notify message fail, error message:{}", e);
        }

    }



    @Override
    public void resetFirstLoginStatus(String phoneNumber) {
        //重置登录状态为 “未登陆过”
        this.updateFirstLoginStatus(phoneNumber, FirstLoginStatusEnum.YES);
    }
}
```



```
mvn clean install
打包完成后发布到ECS服务器上
scp target/little-project-rocketmq.jar root@47.102.130.27:/home/admin/littl
e-project-rocketmq/target
```

部署到ecs服务器上，

这里注意需要加上这个属性：否则在编译时会报错。

![image-20210513104425245](../picture/基于RocketMQ的互联网酒店预订系统项目/image-20210513104425245.png)

![image-20210513104322730](../picture/基于RocketMQ的互联网酒店预订系统项目/image-20210513104322730.png)



<img src="../picture/基于RocketMQ的互联网酒店预订系统项目/image-20210513104336171.png" alt="image-20210513104336171" style="zoom:33%;" />

这里看到dubbo的地址，其实就是该ecs的内网地址，mysql，redis等dubbo服务是部署在分配的云服务器上的

![image-20210513105018550](../picture/基于RocketMQ的互联网酒店预订系统项目/image-20210513105018550.png)

![image-20210513105409864](../picture/基于RocketMQ的互联网酒店预订系统项目/image-20210513105409864.png)

向nacos中寻找所需服务：

![image-20210513105714073](../picture/基于RocketMQ的互联网酒店预订系统项目/image-20210513105714073.png)







# 优惠券服务





## 优惠券管理模块



### 发放优惠券的业务流程





在客户登录“微信小程序”以后，判断是否第一次登录，然后记录第一次登录的信息。随后，会通过LoginProducer发起一次消息，这个消息会通过消息队列RocketMQ发送到CouponConsumer中。

此时，**`CouponConsumer`**作为**优惠券消费者**接收到这个消息，会进行**“消费第一次登录消息”**的动作，从而调用**`CouponService`**去执行**“发放优惠券”**的动作。在“发放优惠券”以后，**客户就可以通过微信小程序看到对应的优惠券信息了。**

不过这部分的功能没有画在下图中，代码的实现已经由儒猿团队在PHP端实现了。大家可以理解为“发放优惠券”会**将优惠券保存到用户优惠券表中了**，PHP代码只是做了简单的**查询**动作。



![图片1.png](../picture/基于RocketMQ的互联网酒店预订系统项目/klkkyzbf0rwa.png)



### 优惠券初始化



针对初始化的操作不需要大家进行任何操作，已经有儒猿团队完成。在儒猿团队提供的后台中可以添加优惠券信息。

![图片2.png](../picture/基于RocketMQ的互联网酒店预订系统项目/klkkzsq907cg.png)



在点击“添加优惠券”之后会弹出如图3的内容，可以选择优惠券的种类、类别、类型、名称、参数设置、有效期、数量、状态、使用范围、优惠说明、排序等一些列信息。

![图片3.png](../picture/基于RocketMQ的互联网酒店预订系统项目/klkl0tso095m.png)



在完成添加以后，会通过图4中的内容，在优惠券的列表中会显示刚刚添加的优惠券。

![图片4.png](../picture/基于RocketMQ的互联网酒店预订系统项目/klkl19ow04cx.png)





## 给首次登录的用户发一张优惠券



- 数据库表设计与实体类



![图片1.png](../picture/基于RocketMQ的互联网酒店预订系统项目/klklmydz0l5a.png)

如果说coupon表是记录优惠券基本信息的话，那么coupon user 表就是**记录优惠券与用户关系**的。如图2 所示，"t_coupon_user“表的表结构中，记录 phone_number 和coupon_id 的信息，这张表就建立了用户和优惠券的关联。换句话说当把优惠券发放给某个人时，就会在这张表中进行记录。

![图片2.png](../picture/基于RocketMQ的互联网酒店预订系统项目/klklniqv0jyi.png)

在“/api/coupon/dto”下面定义了“FirstLoginMessageDTO”的实体类，从名字上看它是用来获取RocketMQ中关于第一次登录消息的。Message包含：userId（用户ID）、nickName（用户名称）、beid（小程序id）以及phoneNumber（用户手机号）。





- 优惠券消费者**CouponConsumer**



实体类定义的是第一次登录的队列消息（FirstLoginMessageDTO），为了接受这个订阅消息需要定义与之对应的CouponConsumer，也就是优惠券消费者。如图4所示，在”coupon/consumer/“中定义CouponConsumerConfiguration”，用配置的方式初始化一个“DefaultMQPushConsumer“。

在类中定义了一个loginConsumer方法，在@Bean的annotaion中可以得到是用来监听登录消息的。需要注意的是在@ Qualifier的annotation中定义了具体的监听器名称：“firstLoginMessageListener”。也就是说在CouponConsumer启动以后，是由这个监听器来处理具体RocketMQ的登录消息的。



```java
/**
 * 登录消息的consumer bean
 *
 * @return 登录消息的consumer bean
 */
@Bean(value = "loginConsumer")
public DefaultMQPushConsumer loginConsumer(@Qualifier(value = "firstLoginMessageListener") FirstLoginMessageListener firstLoginMessageListener) throws MQClientException {
    DefaultMQPushConsumer consumer = new DefaultMQPushConsumer(loginConsumerGroup);
    consumer.setNamesrvAddr(namesrvAddress);
    //订阅mq的login topic上的所有消息
    consumer.subscribe(loginTopic, "*");
    //通过“firstLoginMessageListener”的监听方法获取登录发过来的信息
    consumer.setMessageListener(firstLoginMessageListener);
    consumer.start();
    return consumer;
}
```







- 实施消息监听（**FirstLoginMessageListener）**

有了CouponConsumer 作为第一次登录消息的消费者，就要提供一个**处理消息的监听者FirstLoginMessageListener**，说白了**它需要对消息进行处理**。如图5所示，在api/coupon/listener下面定义了FirstLoginMessageListener类，其中有一个`consumerMessage`方法，就是用来对消息进行处理的。

在传入参数是一个Message的List，通过for函数**对这个List进行遍历**，**解析Message的内容**为FirstLoginMessageDTO，从而获取userId（用户id）、phoneNumber（手机号）等信息，通过调用couponService中的DistributeCoupon方法进行优惠券的分发。





- 分发优惠券（**DistributeCoupon）**





## 优惠券重复发放



但是如果过多发放优惠券，特别是一次登录重复发放多张优惠券就损失酒店的利益了



---

**生产者重复发送消息**



这里由于网络抖动，LoginProducer 就发送了两条“登录消息”，分别是“登录消息1”和“登录消息2”。因此，RocketMQ中也保存了上述的两条消息，在CouponConsumer这一端就会监听到两条信息，并且依次对两条消息进行处理。

处理的过程在前面也有讲过是记录用户与优惠券之间的关联，由于处理两次消息，自然就记录了两条关联记录。因此，最后的结果就是CouponService为“第一次登录”分配了两张优惠券。

![图片1.png](../picture/基于RocketMQ的互联网酒店预订系统项目/klku4xg30j8t.png)



---

**消费者重复处理消息**



CouponConsumer在消费完“第一次登录”的消息之后会**给RocketMQ的broker返回CONSUMER_SUCCESS**，即当前consumer消费了登录topic中的消息，成功提交消息offset；

但是当CouponService组件下发优惠券成功后，consumer端返回broker CONSUMER_SUCCESS时**网络抖动或者consumer消费消息后故障**，导致**broker没收到消费成功的响应**，即**消费者提交offset失败。**

当网络恢复或者CouponConsumer**重新启动**的时候，发现**队列指向的offset依旧指着刚才处理的那条“登录消息1”**，因此又将该消息处理了一次。因此就造成了在CouponService 中发放了两次优惠券的情况。

![图片2.png](../picture/基于RocketMQ的互联网酒店预订系统项目/klku6jq909xj.png)



---

**处理消息的幂等机制**



***无论消息被发送多少次，或者消息被反复处理多少次，最后只对同一条消息进行一次处理。***

换句话说**即便是RocketMQ的消息队列中积累了上百条消息，只要他们是相同的消息**，例如：针对**同一个账号的第一次登录**，优惠券只会发送一次，永远不会重复。







## 基于redis幂等机制



解决优惠券发放幂等性的问题需要回到消费者处理消息本身。作为优惠券消息的消费者CouponConsumer需要从RocketMQ的队列中获取消息，在上节课中介绍的两种情况中：

1. 因为网络抖动生产者（LoginProducer）产生了重复的消息，CouponConsumer会重复消费两条消息。
2.  消费者（CouponConsumer本身）由于下线没有通知broker CONSUMER_SUCCESS，导致重启后重复消费。

如图1所示，如果在消费者（CouponConsumer）每次处理消息的时候都将这个消息存放到Redis上进行缓存，**在每次发放优惠券之前都会询问“是否存在相同消息？”**，只有**在“否”的情况下才执行“发放优惠券”**。这里的“否”也就是**不存在相同优惠券**的意思，于是就避免了优惠券的重复发放。



![图片1.png](../picture/基于RocketMQ的互联网酒店预订系统项目/kllpbkh102fg.png)



通过redisApi.setnx方法（已经由儒猿团队进行封装）将消息信息保存到Redis中。设置了“第一次重复登录”的前缀+用户Id（userId）作为Key，后面的用户名（userId）、电话号码（phoneNumber）作为Value。



利用setnx的函数特性，当reponse返回成功“SUCCESS”并且data为”FALSE“说明Redis中对应的Key已经有Value 了，也就说明Redis中已经存在这条消息了，消费者已经处理过这条“第一次登录”的记录了。这种情况下，只记录一个日志，而不进行后续的操作。

如果返回的是“SUCCESS”并且data为“true”，说明Redis没有记录这条消息，也就是消费者第一次处理这个消息，那么就需要更新数据库中用户和优惠券的关系，从而完成给用户发放优惠券的操作。



```java
public ConsumeConcurrentlyStatus consumeMessage(List<MessageExt> msgs, ConsumeConcurrentlyContext context) {
    Integer userId = null;
    String phoneNumber = null;
    for (MessageExt msg : msgs) {
        String body = new String(msg.getBody(), StandardCharsets.UTF_8);
        try {
            LOGGER.info("received login success message:{}", body);
            // 第一次登陆消息内容
            FirstLoginMessageDTO firstLoginMessageDTO = JSON.parseObject(body, FirstLoginMessageDTO.class);
            // 用户id
            userId = firstLoginMessageDTO.getUserId();
            // 手机号
            phoneNumber = firstLoginMessageDTO.getPhoneNumber();

            // 通过redis保证幂等
            CommonResponse<Boolean> response = redisApi.setnx(RedisKeyConstant.FIRST_LOGIN_DUPLICATION_KEY_PREFIX + userId, //这是key
                                                              String.valueOf(userId),//userId和phone是value
                                                              phoneNumber,
                                                              LittleProjectTypeEnum.ROCKETMQ);
            //redis中已经有这个优惠券数据了，
            if (Objects.equals(response.getCode(), ErrorCodeEnum.FAIL.getCode())) {
                // 请求redis dubbo接口失败
                LOGGER.info("consumer first login message redis dubbo interface fail userId:{}", userId);
                return ConsumeConcurrentlyStatus.RECONSUME_LATER;
            }

            // redis操作成功
            if (Objects.equals(response.getCode(), ErrorCodeEnum.SUCCESS.getCode())
                    && Objects.equals(response.getData(), Boolean.FALSE)) {
                // 重复消费登录消息 返回
                LOGGER.info("duplicate consumer first login message userId:{}", userId);
            } else {
                // 未重复消费 调用service分发优惠券
                couponService.distributeCoupon(firstLoginMessageDTO.getBeid(),
                                               firstLoginMessageDTO.getUserId(),
                                               firstLoginCouponId,
                                               firstLoginCouponDay,
                                               0,
                                               phoneNumber);
                LOGGER.info("distribute userId:{} first login coupon end", userId);
            }
        } catch (Exception e) {
            // 消费失败，删除redis中幂等key
            if (userId != null) {
                redisApi.del(RedisKeyConstant.FIRST_LOGIN_DUPLICATION_KEY_PREFIX + userId,
                             phoneNumber,
                             LittleProjectTypeEnum.ROCKETMQ);
            }
            // 消费失败
            LOGGER.info("received login success message:{}, consumer fail", body);
            // Failure consumption,later try to consume 消费失败，以后尝试消费
            return ConsumeConcurrentlyStatus.RECONSUME_LATER;
        }
    }
    //消费成功，向broker返回成功消息，offset右移
    return ConsumeConcurrentlyStatus.CONSUME_SUCCESS;
}
```









## 测试



----

- 无redis版本

在登录小程序之前，通过Postman请求resetlogin的API，重置登录信息，保证此次是“第一次登录”。

```
http://139.224.52.148:8088/api/login/resetLoginStatus?phoneNumber=xxx
```

那么每次进行这个操作再重新进行登录都会得到一张优惠券，**这就是没有进行幂等性的结果**（这里只是模拟网络抖动等故障重复发送/消费消息的情况）

<img src="../picture/基于RocketMQ的互联网酒店预订系统项目/image-20210513140457897.png" alt="image-20210513140457897" style="zoom: 33%;" />





---

- **有redis版本**

无论重复第一次登录这个行为多少次都不会多发送优惠券



会出现重复消费消息的提示，因为**此时redis中已经保存了这个userId的《第一次登录优惠券》的信息，**如果处理过，就不用再次发放了，同时抛出上面这句话作为日志记录下来。

![image-20210513141036468](../picture/基于RocketMQ的互联网酒店预订系统项目/image-20210513141036468.png)





# 客房管理模块







## 酒店管理模块





存在**酒店后台管理**和**酒店小程序**两个操作入口，酒店后台管理员会通过酒店后台管理系统对酒店管理和房间管理模块进行操作。



管理员通常会将这两部分的数据进行配置，其中酒店管理的配置已经由儒猿团队实现初始化好了，不用学员进行额外配置。

从图中可以看出房间管理由一条红线连接到酒店管理，**说明房间管理依赖于酒店管理，学员在添加房间的时候会选择对应的酒店信息。**



在客户登录酒店小程序的时候，会通过查询酒店调用酒店管理模块获取酒店的基本信息，然后通过查询房间模块获取房间管理的信息。图中右下方的两个蓝色模块会对左上方两个橙色模块产生数据上的依赖

![图片1.png](../picture/基于RocketMQ的互联网酒店预订系统项目/kllxcd0202p2.png)



正如上面提到的酒店管理模块作为基础数据模块已经由儒猿团队完成初始化，如图2 所示，我们可以看到这里已经生成了三个基本的酒店信息，分别是儒猿酒店成都分店、深圳分店和北京分店。

![图片2.png](../picture/基于RocketMQ的互联网酒店预订系统项目/kllxe2tr0tu0.png)

这里是添加分店的页面，可以输入分店名称、排序信息、营业状态、分店头像、分店图片、服务标签、客服电话、详情介绍、分店地址、详细地址等信息。对分店进行描述，**这个界面从儒猿小实战平台是无法看到的，大家对此有一定了解就可以了**。在实际应用中，**这些基础配置界面都是需要开发的。**

![图片3.png](../picture/基于RocketMQ的互联网酒店预订系统项目/kllxeojd0qzr.png)



房间的列表信息，如图5所示，这里列出房间的基本信息：分店名称、房型名称、房型图片、房型状态、售价价格、库存，在最左边还提供了编辑和删除的功能。房间管理中的分店信息就是在图2的时候已经配置好的，这里直接使用了。同时如果需要添加房间可以点击右上角的按钮。



![image-20210513142223450](../picture/基于RocketMQ的互联网酒店预订系统项目/image-20210513142223450.png)



添加房间：在弹出的窗体中列出添加房间需要输入的基本信息。包括所属分店、房型名称、价格、每日库存、减库存方式、房型图片等。输入对应信息以后点击提交就可以添加房间了。

![图片6.png](../picture/基于RocketMQ的互联网酒店预订系统项目/kllxoge20ldb.png)







## 客房管理模块



在酒店小程序中会使用酒店管理后台系统中提供的分店信息和房间信息，其执行的流程也比较简单：首先根据条件查询酒店，在列出的分店列表中**选择分店**，然后在**房间列表**中选中对应的房间。下面就在小程序中把上面的流程走一遍。

如图1 所示，进入小程序以后登录授权和获取手机之后，会看到**酒店查询界面**。其中，可以通过**酒店名称或者关键字、附近信息以及入住和退房日期等查询信息来获取的酒店信息**。在选择好查询条件以后，点击“查询酒店”便进入酒店列表。

<img src="../picture/基于RocketMQ的互联网酒店预订系统项目/image-20210513142728952.png" alt="image-20210513142728952" style="zoom:33%;" />



点击“查询酒店”以后就进入了酒店列表页面，在这里列出了所有满足条件的酒店信息。如图2所示，下列的三个分店都是儒猿团队在后台初始化好的，每条酒店记录都由**酒店图片、酒店名称、酒店地址、距离以及房间起价**几个字段组成的。同时可以**根据价格和距离对其进行排序**。

<img src="../picture/基于RocketMQ的互联网酒店预订系统项目/klly2mbs0wzf.png" alt="图片2.png" style="zoom:50%;" />



在查询出来的酒店列表中选择一个酒店便可以看到其具体的房间信息，如图3所示，我们点击“儒猿酒店北京分店”，在酒店详细信息包括酒店名称、酒店地址和联系电话。同时将查询时的入住和退房时间也一并显示出来。

在“房型列表”中列出该酒店所包括的房型信息，这个信息是在酒店后台管理后台中由学员自行配置的，这里我们配置了一个房型记录为“test01”，**可以看到房型图片、房间面积、床的类型、以及早餐数量和价格信息等。**



通过上述的查询酒店->酒店列表->房型列表三个步骤的操作就能够查询到房间信息。***其中酒店信息和房间信息是已经在酒店管理后台中已经配置好的，这里只是显示对应的内容。***





## **客房详情数据在高并发查询下的热key问题**



- 传统获取房间信息的方法

在酒店小程序端向酒店程序后台API**发起获取房间信息的请求**，后台API通过`HotelRoomService`的服务通过数据库**返回对应的房间信息**。其过程如下：

1. 酒店小程序发起获取房间信息请求。

2. 酒店小程序后台API，通过服务HotelRoomService**查找房间信息**，从数据库中得到信息并且**返回给小程序**。

![图片1.png](../picture/基于RocketMQ的互联网酒店预订系统项目/klm0asg6010s.png)

这种方法直接从数据库中获取信息，**一旦请求并发亮上来了，数据库无疑就成为系统的瓶颈。**



---

- **使用redis缓存房间信息**

通过改造在酒店小程序后台API这一层加入Redis缓存，用来缓存酒店房间的信息。这样HotelRoomService**不用先请求数据库，而是就近请求Redis缓存**。由于Redis缓存**在内存中**，因此比磁盘上的数据库有**更高的响应能力**。

如图2 所示，房间信息请求顺序如下：

1. 酒店小程序通过酒店小程序后台API中的HotelRoomService获取房间信息。
2. HotelRoomService首先去询问Redis缓存中是否存在该房间信息，如果**存在房间信息就直接返回给小程序。**
3. 如果Redis中不包含房间信息，再请求数据库，并且返回信息。

![图片2.png](../picture/基于RocketMQ的互联网酒店预订系统项目/klm0dr5m0fwk.png)





---

- **使用JVM 进程内缓存房间信息**



众所周知数据存放在**离客户最近的地方**就能更快地响应客户的请求，上面我们讲数据从磁盘搬到了内存中，**`但毕竟HotelRoomService和Redis属于两个不同的进程`**。***HotelRoomService运行在JVM中，而Reids 有自己的进程，在跨进程的数据获取也会面临损耗***。

顺着这个思路，我们将缓存的数据从Redis搬到JVM的本地缓存中，于是就有了图3所示，这里将请求步骤分为了4步：

1. 酒店小程序通过**酒店小程序后台API**中的HotelRoomService获取房间信息。
2. 由于HotelRoomService运行在JVM中，因此会先请求JVM的本地缓存，是否存在酒店房间的信息，如果存在就将其返回给小程序。
3. 如果HotelRoomService所在JVM中不存在房间的缓存信息，再去询问Redis缓存中是否存在该房间信息，如果存在房间信息就直接返回给小程序。
4. 最后，Redis中也不包含房间信息，再请求数据库，并且返回信息。

![图片3.png](../picture/基于RocketMQ的互联网酒店预订系统项目/klm0f5gb0c9a.png)

最后加入了JVM 缓存信息以后让数据响应效率得到进一步的提升。不过JVM的缓存大小取决于JVM的大小，这里**推荐存放不是太大的数量，这些数据最好是热点数据。**，存的太多会导致应用OOM异常





## **多级缓存机制**







存放**分店信息**的表“t_shop_categroy”，如图1 所示，该表定义了name（分类名称）、thumb（分类图片）、detail_image（图片详情），area（地区）、phone（电话）等信息。

![图片1.png](../picture/基于RocketMQ的互联网酒店预订系统项目/klm11r5c08pt.png)



**房间信息表**”t_shop_goods”其中定义了，phone_number（手机号）、comment（详情图）、marketprice（市场价）、productprice（本店售价）等信息。

![图片2.png](../picture/基于RocketMQ的互联网酒店预订系统项目/klm15b7w01ae.png)





## 测试**热点查询问题**





第一次查询：

![image-20210513151504494](../picture/基于RocketMQ的互联网酒店预订系统项目/image-20210513151504494.png)

1. 先检查JVM缓存是否命中，发现JVM没有命中，于是查询Redis缓存
2. 查找Redis缓存发现没有命中，去查找数据库。
3. JVM和Redis都没有命中的情况下从数据库中获取数据。



第二次再查询：直接命中 jvm堆中的缓存：

![image-20210513151630161](../picture/基于RocketMQ的互联网酒店预订系统项目/image-20210513151630161.png)



重启服务： `sh deploy.sh restart`

jvm中的缓存由于重启失效了，但redis中的缓存还保留着：

![image-20210513151824089](../picture/基于RocketMQ的互联网酒店预订系统项目/image-20210513151824089.png)







## **如何保证多级缓存的数据一致性**





- 多级缓存的问题

为了提供用户更快的响应速度，我们会增加Redis和JVM本地缓存机制。***用户在请求数据按照 JVM本地缓存->Redis缓存->数据库的顺序进行***。这样的好处是**让缓存数据离用户足够近，响应足够迅速，特别是在高并发场景下，每多一次调用都会带来系统的损耗**。

上面描述的都是数据读取的场景，**缓存的数据是如何写入的**？在写入缓存数据的时候如何**保持Redis缓存和JVM本地缓存的一致性呢？**如果Redis缓存发生了改变，而**没有及时通知JVM本地缓存，导致其没有改变**，刚好**此时用户访问时根据房间ID命中了JVM缓存**，岂不是拿到的数据就是不正确的了。



为了解决这个问题，我们**使用了RocketMQ作为Redis与JVM缓存之间的“通信员”**，当修改Redis缓存的同时也**发送RocketMQ的广播消息，通知缓存信息修改了**，消息的消费者如果接到该信息以后**再去对JVM缓存进行修改，如此这般就可以保持数据的一致性了**。





---

- 多级缓存的数据一致性

对酒店项目的程序进行了升级

将缓存数据同步的部分画上了标号

1. 当酒店管理员通过酒店管理后台对房间信息进行修改或者添加的时候，会调用酒店后台API中的**房间管理服务**。

2. 房间管理服务除了对房间进行修改**保存到数据库之外**，还有一个任务就是***将修改后的房间信息保存到Redis缓存中***。

3. 在保存信息到Redis之后，房间管理服务还充当了**消息的生产者**，这里它会**发送房间更新的消息到RocketMQ的队列中**，以***供消费者消费该房间更新消息***。

4. 作为消费者的`HotelRoomUpdateMessageListener`来说，它位于酒店小程序后台的后台API的房间管理模块中，它会一直**监听房间更新的消息**。

5. 一旦监听到房间更新消息以后会**根据对应的房间ID从Redis中获取房间信息**。

6. 并**将房间信息保存到JVM本地的缓存中**。这样当客户通过酒店小程序调用JVM缓存获取房间信息时，就可以获取最新的房间信息了。**保证了JVM和Redis中数据的一致性**。

> 消息队列中的 <房间更新消息> 只是通知   酒店小程序的后台API去**找redis中的数据**，来进行**更新自身jvm缓存中的数据**
>
> 将管理后台中修改的房间信息**保存到Redis的同时**，发送消息给RocketMQ，从而通知小程序后台API**去更新JVM的缓存**。这样在用户请求小程序后台API获取房间信息的时候，就可以获取最新的缓存信息了。

![图片1.png](../picture/基于RocketMQ的互联网酒店预订系统项目/klmkmszo0csr.png)



























































































































































































































































































































































































































































































































































































































































































































































































































































































