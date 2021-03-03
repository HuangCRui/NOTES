# 分布式技术-Zookeeper



# 1.Zookeeper概述



## 1.1概述



- 美团、饿了么、淘宝--->  服务  商家、用户,**服务型的应用平台**
- Zookeeper是一个开源的分布式（**多台服务器干一件事**）的，为分布式应用***提供协调服务***  的Apache项目
- 在大数据技术生态圈中，Zookeeper（管理这些动物...），Hadoop，Hive







## 1.2工作机制



- Zookeeper从设计模式角度来理解：是一个基于 **观察者模式（一个人干活，有人盯着他）**，设计的分布式服务管理框架

- 它负责 存储 和 管理 大家都关心的数据

  - 然后接受观察者的**注册**，一旦这些**数据发生变化**

  - Zookeeper就将负责**通知**已经注册的那些**观察者**做出相应的反应

  - 从而实现集群中类似**Master/Slave**管理模式

    

    > **Zookeeper  =  文件系统 + 通知机制**



![image-20210228161346297](../picture/Zookeeper%E8%AF%A6%E8%A7%A3/image-20210228161346297.png)

栗子：

1. 商家营业并入驻
2. （消费者）获取到当前营业的饭点列表
3. 服务器节点下线（商家打烊）
4. 服务器节点上下线事件通知  **监听+通知**
5. 重新再去获取服务器列表，并**注册监听->美团对饭店的监听**







## 1.3特点



> 分布式和集群的区别？
>
> **无论分布式还是集群，都是很多人在做事情**。具体区别如下：
>
> 有一个饭店，越来越火爆，需要多招聘一些工作人员：
>
> - 分布式：招聘1个厨师，1个服务员，1个前台，三个人**负责的工作不一样**，但是最终目的都是为饭店工作，**一起合作才能完成这件事**
> - 集群：招聘3个服务员，3个人的**工作一样**



![image-20210228162045999](../picture/Zookeeper%E8%AF%A6%E8%A7%A3/image-20210228162045999.png)



1. 是**一个leader和多个follower**来组成的 **集群**，（只有一台是leader）
2. 集群中只要有**半数以上的节点存活**，Zookeeper就能正常工作（5台服务器挂两台，没问题。4台服务器挂2台，就停止）-> **奇数台服务器**
3. **全局数据一致性**，**每台服务器都保存一份相同的数据副本**，无论client连接哪台server，数据都是一致的
4. **数据更新原子性**，一次数据要么所有服务器更新数据全成功，要么全都更新数据失败。
5. **实时性**，在一定时间范围内，client能读取到**最新数据**
6. 更新的请求**按照顺序执行**，会按照发送过来的顺序，**逐一执行**







## 1.4数据结构



![image-20210228170905111](../picture/Zookeeper%E8%AF%A6%E8%A7%A3/image-20210228170905111.png)



- Zookeeper数据模型的结构和linux文件系统很类似，整体上可以看作是一棵树，每个节点乘坐一个ZNode(ZookeeperNode)
- 每一个ZNode默认能够存储1MB的数据（元数据），每个ZNode的路径都是唯一的
  - 元数据Metadata，又称中介数据、中继数据，为描述数据的数据(data about data)，主要是描述数据属性proeprty的信息，用来支持如**指示存储位置，历史数据、资源查找、文件记录等功能**，不保存数据本身。







## 1.5应用场景

提供的服务包括：统一命名服务、统一配置管理、统一集群管理、服务器节点动态上下线、软负载均衡





### 1.5.1统一命名服务



- 在分布式环境下，通常需要对应用或服务进行统一的命名

- 服务器的IP地址不容易记，但域名更好记



一个域名后面有很多台服务器，**统一把这些服务器的ip命名为`www.xxx.com`域名地址**





### 1.5.2统一配置管理

- 分布式环境下，配置文件做同步是必经之路
- 1000台服务器，如果**配置文件作出修改**，那一台一台的改，工作量太大。如何做到**修改一处就快速同步到每台服务器上**



![image-20210228172304844](../picture/Zookeeper%E8%AF%A6%E8%A7%A3/image-20210228172304844.png)



将配置管理交给Zookeeper

1. 将配置信息写入到Zookeeper的某个节点上
2. 每个客户端都 **监听这个节点**
3. 一旦节点中的**数据文件被修改**，Zookeeper这个“话匣子”就会**通知**每台客户端**服务器**，每台服务器自动获得这个修改后的数据，并更改自身的配置





### 1.5.3服务器节点动态上下线



- 客户端能实时获取服务器上下线的变化
- 栗子：在美团APP上能实时看到商家是否在营业或打烊







### 1.5.4软负载均衡



- Zookeeper会记录每台服务器的**访问数**，让**访问数量最少的服务器**去处理最新的客户请求（**雨露均沾**）









## 1.6下载地址



镜像库地址：http://archive.apache.org/dist/zookeeper





![image-20210228173524564](../picture/Zookeeper%E8%AF%A6%E8%A7%A3/image-20210228173524564.png)



`apache-zookeeper-3.6.2.tar.gz`需要安装maven，一个命令下载安装很多jar包

`apache-zookeeper-3.6.2-bin.tar.gz`自带所需的jar包







# 2.zookeeper本地模式安装





## 2.1本地模式安装



1. 安装jdk
2. 将安装包拷贝到opt目录
3. 解压安装包





### 2.1.2配置修改



在opt/zookeeper这个目录上创建zkData和zkLog目录





cp一份新的配置文件`zoo.cfg`

![image-20210228174559676](../picture/Zookeeper%E8%AF%A6%E8%A7%A3/image-20210228174559676.png)



修改Data和log目录

![image-20210228174813596](../picture/Zookeeper%E8%AF%A6%E8%A7%A3/image-20210228174813596.png)



### 2.1.3操作Zookeeper



1. 启动zookeeper

   进入bin目录：

   ```bash
   [root@iZ2zeddfx87fw5aiee280uZ bin]# ./zkServer.sh start
   /usr/bin/java
   ZooKeeper JMX enabled by default
   Using config: /opt/zookeeper/bin/../conf/zoo.cfg
   Starting zookeeper ... STARTED
   
   ```



2. 查看状态：

![image-20210228175221715](../picture/Zookeeper%E8%AF%A6%E8%A7%A3/image-20210228175221715.png)

standalone：孤独运行。。



3. 查看进程是否启动

```bash
[root@iZ2zeddfx87fw5aiee280uZ bin]# jps
2432 QuorumPeerMain
3487 Jps
```

`QuorumPeerMain`:是Zookeeper集群的启动入口类，是用来加载配置启动QuorumPeer程的





4. 启动客户端

进入zookeeper客户端

```
./zkCli.sh
```









## 2.2配置参数解读



Zookeeper中的配置文件zoo.cfg中参数含义如下：



- **tickTime=2000**：通信心跳数，Zookeeper服务器与客户端心跳时间，单位毫秒
  - Zookeeper使用的基本时间，**服务器之间**或**客户端与服务器之间** ->**维持心跳时间的时间间隔**，也就是**每个tickTime时间就会发送一个心跳**，时间单位为毫秒
  - 证明还活着....
- **initLimit=10**：LF初始通信时限
  - 集群中的**Followers跟随者服务器与Leader领导者服务器**，启动时能**容忍**的最多心跳数
  - 10*2000（10个心跳时间）如果**领导和跟随者**没有发出心跳通信，就视为失效的连接，**领导和跟随者彻底断开**
  - 表示失去联系，不是死亡，只是表示失效连接
- **syncLimit=5**：LF同步通信时限
  - 集群启动后，Leader与Follower之间的**最大响应时间单位**，假如响应超时suncLimit*TickTime->10s，Leader就认为Follower已经死掉，会将Follower从服务器列表中删除
  - 如果Follower在10s内没有响应Leader的请求，认为死了
- **DataDir**：数据文件目录+数据持久化路径
  - 用于保存Zookeeper中的数据
- **dataLogDir**：日志文件目录
- **clientPort=2181**：客户端连接端口
  - 监听客户端连接的端口



> - **initLimit：LF初始通信时限**
>
> ​    集群中的follower服务器(F)与leader服务器(L)之间 **初始连接** 时能容忍的最多心跳数（tickTime的数量）。
>
>    此配置表示，允许 follower （相对于 leader 而言的“客户端”）连接 并同步到 leader 的**初始化连接时间**，它以 tickTime 的倍数来表示。当超过设置倍数的 tickTime 时间，则连接失败。
>
>    如果在设定的时间段内，**半数以上的跟随者未能完成同步**，领导者便会宣布放弃领导地位，**进行另一次的领导选举**。如果zk集群环境数量确实很大，同步数据的时间会变长，因此这种情况下可以适当调大该参数。默认为10。
>
>  
>
> - **syncLimit：LF同步通信时限**
>
>    集群中的follower服务器(F)与leader服务器(L)之间 请求和应答 之间能容忍的最多心跳数（tickTime的数量）。
>
>    此配置表示， leader 与 follower 之间发送消息，**请求 和 应答 时间长度**。如果 follower 在设置的时间内不能与leader 进行通信，那么此 follower 将被丢弃。所有关联到这个跟随者的客户端将连接到另外一个跟随着。









# 3.Zookeeper内部原理





## 3.1 选举机制（重点）



- **半数机制：及群众半数以上机器存活，集群可用，所以Zookeeper适合安装技术台数服务器**
- 虽然在配置文件中并没有指定Master和Slave，但是，Zookeeper工作时，室友一个节点为Leader，其他则为Follower，Leader是通过内部的选举机制临时产生的



![image-20210228181716797](../picture/Zookeeper%E8%AF%A6%E8%A7%A3/image-20210228181716797.png)

选举Leader：**5台Server，必须半数以上：3票**

1. Server1先投票，**投给自己**，自己为1票，没有超过半数，根本无法成为leader，顺水推舟将票数投给id比自己大的Server2
2. Server2也把自己的票数投给了自己，再加上Server1给的票数，总票数为2，没有超过半数，也无法成为leader，也学习Server1，顺水推舟，将自己所有的票数给了id比自己大的Server3
3. Server3得到了Server1和Server2的两票，再加上自己投给自己的一票，3票超过半数，顺利成为Leader
4. Server4和Server5都投给自己，但是无法改变Server3的票数，只好听天由命，承认Server3是leader





## 3.2 节点类型





- 持久型（persistent）：
  - **持久化目录节点**，客户端与Zookeeper断开连接后，该节点依旧存在
  - **持久化顺序编号目录节点（persistent_sequential）**，客户端与Zookeeper断开连接后，该节点依旧存在，创建znode时**设置顺序标识**，znode名称后会**附加一个值**，顺序号是一个**单调递增的计数器**，由**父节点维护**，例如：Znode**001**，Znode**002**......   ，名字是一样的都是Znode，但后面的顺序标识不一样



- 短暂型（ephemeral）：
  - **临时目录节点**，客户端和服务器断开连接后，创建的节点自动删除
  - **临时顺序编号目录节点**，客户端与Zookeeper断开连接后，该节点被删除，创建znode时设置顺序标识，znode名称后会附加一个值，顺序号是一个单调递增的计数器，由父节点维护，Znode**001**，Znode**002**...... 



> 注意：序号相当于i++，和数据库中的自增长类似









## 3.3 监听器原理（重点）



![image-20210228183001078](../picture/Zookeeper%E8%AF%A6%E8%A7%A3/image-20210228183001078.png)



1. 在main方法中创建Zookeeper客户端 （进程）的同时就会创建**两个线程**，**一个负责网络连接通信，一个负责监听listener**

2. **监听事件**就会通过网络通信发送给Zookeeper，Zookeeper得到消息：**需要监听敌人这个节点，**
3. Zookeeper获得注册的监听事件后，将这个监听事件放入监听的列表中
4. Zookeeper监听到  **数据变化  或  路径变化**，就会将这个消息发送给监听线程
   1. 常见的监听
      1. 监听**节点数据**的变化：get path[watch]
      2. 监听**子节点增减**的变化：Is path [watch]，这个节点(敌人)创建了子节点(创建了骑兵？)
5. 监听线程就会在内部调用**process方法**（需要**我们实现**process方法内容），针对监听的线程发生变化后，需要**执行的业务逻辑**





## 3.4 写数据流程





![image-20210228183855304](../picture/Zookeeper%E8%AF%A6%E8%A7%A3/image-20210228183855304.png)



1. Client想向Zookeeper的Server1上写数据，必须得先发送一个写的请求
2. 如果Server1**不是Leader**，那么Server1会把接收到的请求进一步**转发给Leader**
3. 这个Leader会将**写请求广播给各个Server**，各个Server写成功后就会通知Leader
4. 当Leader收到**半数以上的Server数据写成功了**，那么就说明数据写成功了
5. 随后，Leader会告诉Server1数据写成功了
6. Server1会**反馈通知client**数据写成功了，整个流程结束









# 4. Zookeeper实战（重点）







## 4.1 分布式安装部署



### 配置集群属性

集群思路：先搞定一台服务器，再克隆出两台，形成集群



1. 安装Zookeeper

2. 配置服务器编号

   在opt/zookeeper/zkData下创建myid文件，在文件中添加与Server对应的编号： `1`，表示是一号服务器，其余两台服务器分别对应2和3

3. 配置zoo.cfg文件：

![image-20210301011436738](../picture/Zookeeper%E8%AF%A6%E8%A7%A3/image-20210301011436738.png)



配置参数解读server.A=B:C:D

- A: 一个数字，表示**第几号服务器**。集群模式下配置的opt/zookeeper/zkData/muid文件里的数据就是A的值
- B：服务器的**ip**地址
- C：与集群中**Leader服务器交换信息**的端口
- D：**选举时专用端口**，万一集群中的Leader服务器挂了，需要一个端口来重新进行选举，选出一个新的Leader，而这个端口就是用来执行选举时服务器相互通信的端口



----



### 配置其余两台服务器



1. 在虚拟机数据目录下，创建zk2
2. 将本台服务器数据目录下的**.vmx文件和所有的.vmdk文件**分别拷贝到zk02下
3. 虚拟机--文件--打开（选择zk02下的.vmx文件）

4. 开启此虚拟机，选择：“我已复制该虚拟机”
5. 进入系统后，修改linux中的ip，修改opt/zookeeper/zkData/myid中的数值为2/3

![image-20210301014142285](../picture/Zookeeper%E8%AF%A6%E8%A7%A3/image-20210301014142285.png)





### 集群操作



1. 每台服务器的防火墙必须关闭

   ```
   firewall-cmd --state
   systemctl stop firewalld.service
   systemctl disable firewalld.service 
   ```

   

2. 启动第一台

   ```bash
   cd /opt/zookeeper/bin
   ./zkServer.sh start
   ```

   

若是只启动其中一台：

![image-20210301021032969](../picture/Zookeeper%E8%AF%A6%E8%A7%A3/image-20210301021032969.png)



**因为没有半数服务器启动，所以会失败**



再启动一台：

![image-20210301021138523](../picture/Zookeeper%E8%AF%A6%E8%A7%A3/image-20210301021138523.png)



这时就是leader，另外的是follower









## 4.2 客户端命令行操作

- 启动客户端

  ```
  ./zkCli.sh
  ```

  

- `help`：显示所有操作命令



- `ls /`：查询根目录下的节点，后面跟着path



### 节点详细数据



- `ls -s /`：查看当前节点详细数据

  ![image-20210301022152700](../picture/Zookeeper%E8%AF%A6%E8%A7%A3/image-20210301022152700.png)

  - cZxid：创建节点的**事务**
    - 每次修改Zookeeper状态都会受到一个zxid形成的**时间戳**，也就是Zookeeper事务ID
    - 事务ID是Zookeeper中**所有修改总的次数**
    - 每个**修改都有唯一的zxid**，如果zxid1小于zxid2，那么zxid1一定在zxid2**之前发生**
  - clime：被创建的毫秒数--从1970年开始
  - mZxid：最后更新的事务ID
  - mtime：最后修改的毫秒数
  - pZxid：最后更新的子结点zxid
  - cversion：创建版本号，子结点修改次数
  - dataVersion：数据变化版本号
  - adVersion：权限版本号
  - ephemeralOwner：如果是临时节点，这个是znode拥有者的session id，如果不是临时节点则是0
  - dataLength：数据长度
  - numChildren：子节点数[当前只有zookeeper节点]



- 创建普通节点

  - 在根目录下，创建两个节点：

    ```
    create /china
    create /usa
    ```

    ![image-20210301022958356](../picture/Zookeeper%E8%AF%A6%E8%A7%A3/image-20210301022958356.png)

  - ```
    # 创建目录并保存数据
    create /russia "pujing"
    ```

    **得到数据：**

  - ```
    get /russia
    ```

  - ![image-20210301023413771](../picture/Zookeeper%E8%AF%A6%E8%A7%A3/image-20210301023413771.png)

    **必须先创建父目录才能再在其中创建子目录**

    ![image-20210301023532285](../picture/Zookeeper%E8%AF%A6%E8%A7%A3/image-20210301023532285.png)

  - **默认创建的节点都是持久的**



- 创建**临时节点**

  ```
  create -e /uk
  ```

  **断开与Zookeeper的连接，推出客户端，重新连接，再进行查询，就发现这个临时节点消失**



- 创建**带序号**的节点  **s：sequential**，（默认为持久的，-cs：短暂带序号）

  ```
  create -s /russia/city
  ```

  ![image-20210301023934086](../picture/Zookeeper%E8%AF%A6%E8%A7%A3/image-20210301023934086.png)





- **修改**节点数据值

  ```
  set /jp/Tokyo "too hot"
  ```

  



- **监听** **节点的值变化**  或  **子节点变化(路径变化)**

  - 在server3主机上注册监听/usa节点的数据变化

    ```
    addWatch /usa
    ```

  - 在server1上修改/usa的数据

    ```
    set /usa "Trump"
    ```

  - server3会立刻响应：

    ![image-20210301024633713](../picture/Zookeeper%E8%AF%A6%E8%A7%A3/image-20210301024633713.png)

    **/usa节点的数据被改变了**

  - 如果在server1的/usa下面创建子节点NewYork

    ```
    create /usa/NewYork
    ```

  - server3会立刻响应：

    ![image-20210301024721220](../picture/Zookeeper%E8%AF%A6%E8%A7%A3/image-20210301024721220.png)

    创建了新的节点

  - 改变NewYork的数据，也会得到响应：

    ![image-20210301024830459](../picture/Zookeeper%E8%AF%A6%E8%A7%A3/image-20210301024830459.png)

  - 再给NewYork节点创建子节点：

    ![image-20210301024917284](../picture/Zookeeper%E8%AF%A6%E8%A7%A3/image-20210301024917284.png)

**只要/usa节点下的所有子节点/子子节点....有任何变化，都会被监听到**





- 删除

  ```
  delete /usa/NewYork/ny
  ```

  ![image-20210301025056251](../picture/Zookeeper%E8%AF%A6%E8%A7%A3/image-20210301025056251.png)

  **监听到删除**

  

  - **递归删除**, 同时删除父目录和子目录

  ```
  deleteall /russia
  ```

  



## 4.3 API应用



### 4.3.1 IDEA环境搭建





依赖：

```xml
    <dependencies>
        <dependency>
            <groupId>org.apache.logging.log4j</groupId>
            <artifactId>log4j-core</artifactId>
            <version>RELEASE</version>
        </dependency>
        <dependency>
            <groupId>org.apache.zookeeper</groupId>
            <artifactId>zookeeper</artifactId>
<!--            Zookeeper版本要和linux中装的版本一致-->
            <version>3.6.2</version>
        </dependency>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
        </dependency>
    </dependencies>
```



log4j.properties日志文件：

```properties
### direct log messages to stdout ###
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.Target=System.out
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%d{ABSOLUTE} %5p %c{1}:%L - %m%n

### direct messages to file mylog.log ###
log4j.appender.file=org.apache.log4j.FileAppender
log4j.appender.file.File=c:/mylog.log
log4j.appender.file.layout=org.apache.log4j.PatternLayout
log4j.appender.file.layout.ConversionPattern=%d{ABSOLUTE} %5p %c{1}:%L - %m%n

### set log levels - for more verbose logging change 'info' to 'debug' ###

log4j.rootLogger=debug, stdout
```







### 4.3.2 创建Zookeeper客户端



```java
public class TestZk {

    //Zookeeper集群的IP和端口
    private String connStr = "192.168.150.128:2181,192.168.150.129:2181,192.168.150.130:2181";

    //session超时的时间  时间不宜设置太小，因为Zookeeper和加载集群环境会因为性能等原因而延迟略高
    //如果时间太少，还没有创建好客户端，就开始操作节点，会报错
    private int sessionTimeOut = 60*1000;

    private ZooKeeper zkClient;

    @Test
    public void init() throws IOException {
        //创建Zookeeper客户端   新建一个监听器，其中有需要编写的监听器process方法
        zkClient = new ZooKeeper(connStr, sessionTimeOut, new Watcher() {
            @Override
            public void process(WatchedEvent watchedEvent) {
                System.out.println("得到监听反馈，进行业务处理！");
            }
        });
    }
}
```







### 4.3.3 创建节点



#### ACL

一个**ACL对象**就是一个**id和permission 对**

- 表示哪个/那些范围的id(Who)在通过了怎样的鉴权(How)之后，就允许进行哪些操作(What)：Who How What
- permission(What)就是一个**int表示的位码**，每一位代表一个对应**操作的允许状态**
- 类似unix的文件权限，不同的是共有5种操作：**create、read、Write、delete、admin**(对应更改ACL的权限)
  - OPEN_ACL_UNSAFE：创建**开放**节点，允许任意操作（**用的最多**，其余的权限用的很少）
  - READ_ACL_UNSAGE：创建**只读**节点
  - CREATOR_ALL_ACL：**创建者才有全部权限，独立拥有**





```java
@Test
public void createNode() throws KeeperException, InterruptedException {
    //节点路径，值，节点权限，节点类型(持久/临时)
    String s = zkClient.create("/china/tianjin", "hcr".getBytes(), ZooDefs.Ids.OPEN_ACL_UNSAFE, CreateMode.PERSISTENT);
    System.out.println(s);//返回已创建节点的名字：/china/tianjin
}
```



创建节点成功：

![image-20210301034411770](../picture/Zookeeper%E8%AF%A6%E8%A7%A3/image-20210301034411770.png)







### 4.3.4 查询绩点的值



```java
//获取节点上的值
@Test
public void getNodeData() throws KeeperException, InterruptedException {
    byte[] data = zkClient.getData("/china/tianjin", false, new Stat());
    String s = new String(data);
    //  /china/tinajin节点的数据：hcr
    System.out.println("/china/tinajin节点的数据：" + s);
}
```







### 4.3.5 修改节点的值



```java
@Test
public void UpdateNodeData() throws KeeperException, InterruptedException {
    Stat stat = zkClient.setData("/china/tianjin", "hcr1".getBytes(), 0);
    System.out.println(stat);
    //4294967327,4294967332,1614541401877,1614541974459,1,0,0,0,4,0,4294967327
    //这就是节点的详细信息的一个对象
}
```



修改一次，事务id

`mZxid：最后更新的事务ID`

![image-20210301035719301](../picture/Zookeeper%E8%AF%A6%E8%A7%A3/image-20210301035719301.png)









### 4.3.6 删除节点



需要有正确的版本：**dataVersion**

![image-20210301035935963](../picture/Zookeeper%E8%AF%A6%E8%A7%A3/image-20210301035935963.png)



```java
@Test
public void deleteNodeData() throws KeeperException, InterruptedException {
    zkClient.delete("/china/tianjin", 2);
}
```









### 4.3.7 获取子节点



```java
@Test
public void getChildren() throws KeeperException, InterruptedException {
    List<String> children = zkClient.getChildren("/jp", false);
    System.out.println("/jp下的子节点："+ children);//  /jp下的子节点：[fushi, Tokyo]
}
```





### 4.3.8 监听子节点的变化



**getChildren 只有在该节点下有新建子节点的时候才会监听到，修改数据无法监听**

```java
//监听根节点下面的变化
//让它监听着
@Test
public void WatchNode() throws KeeperException, InterruptedException {
    List<String> children = zkClient.getChildren("/", true);
    System.out.println("/下的子节点" + children);
    //让线程无限等待下去
    Thread.sleep(Long.MAX_VALUE);
   //System.in.read();
}
```





![image-20210301041121152](../picture/Zookeeper%E8%AF%A6%E8%A7%A3/image-20210301041121152.png)



```java
public void process(WatchedEvent watchedEvent) {
    System.out.println("得到监听反馈，进行业务处理！");
    System.out.println(watchedEvent.toString());
}
```

![image-20210301041521494](../picture/Zookeeper%E8%AF%A6%E8%A7%A3/image-20210301041521494.png)



---







**监听子节点的数据变化：**



```
得到监听反馈，进行业务处理！
WatchedEvent state:SyncConnected type:NodeDataChanged path:/china
```



```java
//获取节点上的值
@Test
public void getNodeData() throws KeeperException, InterruptedException {
    byte[] data = zkClient.getData("/china", true, new Stat());
    String s = new String(data);
    //  /china/tinajin节点的数据：hcr
    System.out.println("/china/tinajin节点的数据：" + s);
    Thread.sleep(Long.MAX_VALUE);
}
```



> **和zkCli.sh操作不同，貌似只能监听一次**















### 4.3.9 判断Znode是否存在



```java
@Test
public void exists() throws KeeperException, InterruptedException {
    Stat exists = zkClient.exists("/china", false);
    if(exists == null){
        System.out.println("不存在");
    }else{
        System.out.println("存在："+exists);//输出这个节点的对象表示
    }
}
```

















## 4.4 模拟美团商家上下线





需求：

- 模拟美团服务平台，商家营业通知，商家打烊通知
- 在根节点下，创建好/meituan节点





### ShopService

```java
public class ShopService {

    private String connectString = "192.168.150.128:2181,192.168.150.129:2181,192.168.150.130:2181";
    private int sessionTimeOut = 60*1000;
    private ZooKeeper zkClient;
    //创建客户端，连接到Zookeeper
    public void connect() throws IOException {
        zkClient = new ZooKeeper(connectString, sessionTimeOut, new Watcher() {
            @Override
            public void process(WatchedEvent watchedEvent) {

            }
        });
    }

    //注册到Zookeeper
    public void register(String shopname) throws KeeperException, InterruptedException {
        //一定要创建临时有序的节点（营业）
        //因为：可以自动编号；断开时节点自动删除(打烊)
        String s = zkClient.create("/meituan/"+shopname, shopname.getBytes(), ZooDefs.Ids.OPEN_ACL_UNSAFE, CreateMode.EPHEMERAL_SEQUENTIAL);
        System.out.println(shopname + " 开始营业了！");
    }

    public static void main(String[] args) throws IOException, KeeperException, InterruptedException {
        //1.开一个饭店
        ShopService shop = new ShopService();


        //2.连接Zookeeper集群(和美团取得联系)
        shop.connect();

        //3.将服务节点注册到Zookeeper(入驻美团)
        //需要传入参数
        shop.register(args[0]);

        //4.业务逻辑代码处理（做生意）
        shop.business(args[0]);

    }

    //做生意
    private void business(String shopName) throws IOException {
        System.out.println(shopName + " 火爆营业中！");
        //一直监听
        System.in.read();
    }
}
```





### Customers

```java
//消费者
public class Customers {

    public static void main(String[] args) throws IOException, KeeperException, InterruptedException {
        //1.获得Zookeeper的连接（用户打开美团app）
        Customers client = new Customers();
        client.connect();

        //2.获取/meituan下的所有子节点(获取商家列表)
        client.getShopList();

        //3.业务逻辑处理(对比商家，下单点餐)
        client.business();
    }

    private void business() throws IOException {
        System.out.println("用户正在浏览商家。。。");
        //这里是希望这个线程一直运行，等待服务器上节点变化，来得到监听器的反馈
        //如果不等待，这个类只运行一次
        //一旦监听到服务器节点变化，Zookeeper客户端的监听器就会执行process方法
        System.in.read();
    }

    //获取商家列表
    private void getShopList() throws KeeperException, InterruptedException {
        //1.获得服务器的子节点信息并且对父节点进行监听，
        List<String> shops = zkClient.getChildren("/meituan", true);
        //2.声明储存服务器信息的集合
        ArrayList<String> shopList = new ArrayList<>();
        for(String shop : shops){
            //节点上的数据
            //如果节点上没有存入数据，会出现nullpointerexception
            byte[] data = zkClient.getData("/meituan/" + shop, false, new Stat());
            shopList.add(new String(data));
        }

        System.out.println("目前正在营业的商家："+shopList);
    }

    private String connectString = "192.168.150.128:2181,192.168.150.129:2181,192.168.150.130:2181";
    private int sessionTimeOut = 60*1000;
    private ZooKeeper zkClient;
    private void connect() throws IOException {
        zkClient = new ZooKeeper(connectString, sessionTimeOut, new Watcher() {
            @Override
            public void process(WatchedEvent watchedEvent) {
                //重新再次查询商家列表，因为发生了变化
                //商家做出信息更改不会进行监听，但如果有商家上下线，会进行监听
                try {
                    getShopList();
                } catch (KeeperException | InterruptedException e) {
                    e.printStackTrace();
                }
            }
        });
    }
}
```









### 测试商家上下线







**如何及时知道有没有商家上下线？**



在Watch监听类的process方法中做下一步处理！**连接Zookeeper时就传入的监听器**

**重新查询所有商家**















以main方法带参数的形式运行：



![image-20210301155030605](../picture/Zookeeper%E8%AF%A6%E8%A7%A3/image-20210301155030605.png)







同时：在customers中监听到了变化，新上线了luckin和KFC：

![image-20210301155004695](../picture/Zookeeper%E8%AF%A6%E8%A7%A3/image-20210301155004695.png)



如果这个时候shop窗口终止了：

![image-20210301155254376](../picture/Zookeeper%E8%AF%A6%E8%A7%A3/image-20210301155254376.png)

**创建的临时节点就没了**

相当于下线了

![image-20210301155347521](../picture/Zookeeper%E8%AF%A6%E8%A7%A3/image-20210301155347521.png)



运行多个shop实例：

![image-20210301155518441](../picture/Zookeeper%E8%AF%A6%E8%A7%A3/image-20210301155518441.png)



新加两个饭店：

![image-20210301155948788](../picture/Zookeeper%E8%AF%A6%E8%A7%A3/image-20210301155948788.png)

![image-20210301155942751](../picture/Zookeeper%E8%AF%A6%E8%A7%A3/image-20210301155942751.png)



关闭FKC2实例：**断开连接该客户端，创建的KFC2节点就删去了**

![image-20210301160125162](../picture/Zookeeper%E8%AF%A6%E8%A7%A3/image-20210301160125162.png)







## 4.5 案例-分布式锁-商品秒杀





- 锁：作用就是让当前的资源不会被其他线程访问！

  - 打开锁，被锁住的资源才能被另一个人访问

- 在Zookeeper中使用传统的锁引发的 **“羊群效应”**：1000个人创建节点，**只有1个人能成功**，999人需要**等待**！

- > 羊群是一种很散乱的组织，平时在一起也是盲目地左冲右撞，但一旦有一只头羊动起来，其他的羊也会不假思索地一哄而上，全然不顾旁边可能有的狼和不远处更好的草。羊群效应比喻人都有一种从众心理，从众心理很容易导致盲从，而盲从往往会陷入骗局遭到失败

Redis：性能高，Zookeeper：可靠性高

![image-20210301160704233](../picture/Zookeeper%E8%AF%A6%E8%A7%A3/image-20210301160704233.png)



**999个人都在抢，没有秩序，一哄而上**



---

- 避免“羊群效应”，Zookeeper采用**分布式锁**

  ![image-20210301173347713](../picture/Zookeeper%E8%AF%A6%E8%A7%A3/image-20210301173347713.png)



1. **所有请求进来**，在/lock下创建 **临时顺序节点**，Zookeeper会**帮你编号排序**
2. 判断自己是不是/lock下 **最小的节点**
   - 是：获得锁---创建节点
   - 否：对前面小我一级的节点进行监听
3. 获得锁请求，处理完业务逻辑，释放锁（删除节点），后一个节点得到通知（比你小的节点死了，你成为最小的了）
4. 重复步骤2



**保证占有锁的就是当前编号最小的节点**





### 实现步骤





### 创建数据库

```mysql
# 商品表
CREATE table product(
	id int primary key auto_increment, -- 商品编号
	product_name varchar(20) not null, -- 商品名称
	stock int not null, -- 库存-----
	version int not null -- 版本
)

insert into product (product_name,stock,version) values("锦鲤-清空购物车-大奖",5,0)

# 订单表  order是个关键字
CREATE TABLE `order`  (
  `id` varchar(100),
  `pid` int NOT NULL, -- 商品编号
  `userid` int NOT NULL,
  PRIMARY KEY (`id`) USING BTREE
) 

```







### 搭建工程

使用springboot：



项目结构：

![image-20210302020759474](../picture/Zookeeper%E8%AF%A6%E8%A7%A3/image-20210302020759474.png)





Mybatis-Mapper接口

```java
@Mapper
public interface OrderMapper {
    //生成订单
    @Insert("insert into `order` values(#{id},#{pid},#{userid})")
    int insert(Order order);
}

@Mapper
public interface ProductMapper {
    //查询指定id的商品（目的是查询库存）
    @Select("select * from product where id=#{id}")
    Product getProduct(int id);

    //减库存
    @Update("update product set stock = stock-1 where id=#{id}")
    int reduceStock(int id);
}
```





service层：

```java
@Service
public class ProductServiceImpl implements ProductService {
    @Autowired
    ProductMapper productMapper;
    @Autowired
    OrderMapper orderMapper;

    @Override
    public void reduceStock(int id) {
        //1.获取库存,根据商品id查询商品
        Product product = productMapper.getProduct(id);

        if(product.getStock() <= 0){
            throw new RuntimeException("已抢光！");
        }

        //2.减库存
        int i = productMapper.reduceStock(id);
        if(i == 1){
            //减库存成功--生成订单
            Order order = new Order();
            order.setId(UUID.randomUUID().toString());//使用uuid工具帮我们生成一个订单号
            order.setPid(id);
            order.setUserid(101);
            orderMapper.insert(order);
        }else{
            //减库存失败
            throw new RuntimeException("减库存失败！");
        }
    }
}
```



controller层：（加了Zookeeper分布式锁后）

```java
@Controller
public class ProductAction {

    @Autowired
    ProductService productService;

    @ResponseBody
    @GetMapping("/product/reduce")
    public Object reduce(@RequestParam("id") int id) throws Exception {
        String connectString = "192.168.150.128:2181,192.168.150.129:2181,192.168.150.130:2181";

        //1.创建curator工具对象
        //重试策略（1000ms 试一次，最多试三次）
        RetryPolicy retryPolicy = new ExponentialBackoffRetry(1000, 3);
        CuratorFramework client = CuratorFrameworkFactory.newClient(connectString, retryPolicy);
        client.start();

        //2.根据客户端工具对象创建"内部互斥锁"
        //内部创建   临时有序节点
        InterProcessMutex lock = new InterProcessMutex(client, "/product_"+id);

        //3.加锁
        try{
            lock.acquire();
            //减去库存
            productService.reduceStock(id);
        } catch (Exception e) {
            if(e instanceof RuntimeException){
                throw e;
            }
        } finally {
            //4.释放锁
            lock.release();
        }
        return "ok";
    }
}
```

























### 启动测试

1. 启动两次工程，端口号分别为8080 、 8088



![image-20210301232350513](../picture/Zookeeper%E8%AF%A6%E8%A7%A3/image-20210301232350513.png)





![image-20210301232406771](../picture/Zookeeper%E8%AF%A6%E8%A7%A3/image-20210301232406771.png)











1. 使用nginx做负载均衡





![image-20210301221825718](../picture/Zookeeper%E8%AF%A6%E8%A7%A3/image-20210301221825718.png)







使用jmeter

一秒发十个请求

![image-20210301230535221](../picture/Zookeeper%E8%AF%A6%E8%A7%A3/image-20210301230535221.png)



请求信息：

![image-20210301230838559](../picture/Zookeeper%E8%AF%A6%E8%A7%A3/image-20210301230838559.png)



监听：

![image-20210301230854279](../picture/Zookeeper%E8%AF%A6%E8%A7%A3/image-20210301230854279.png)





![image-20210301230926987](../picture/Zookeeper%E8%AF%A6%E8%A7%A3/image-20210301230926987.png)









![image-20210301232436175](../picture/Zookeeper%E8%AF%A6%E8%A7%A3/image-20210301232436175.png)





---



结果：

![image-20210301232636318](../picture/Zookeeper%E8%AF%A6%E8%A7%A3/image-20210301232636318.png)



七个请求成功了：

![image-20210302010510315](../picture/Zookeeper%E8%AF%A6%E8%A7%A3/image-20210302010510315.png)



...透支了。因为没有加锁





甚至可能10个请求全部成功：

![image-20210302011127035](../picture/Zookeeper%E8%AF%A6%E8%A7%A3/image-20210302011127035.png)



![image-20210302011142489](../picture/Zookeeper%E8%AF%A6%E8%A7%A3/image-20210302011142489.png)









### apache提供的Zookeeper客户端



基于Zookeeper原生态的客户端类实现分布式是非常麻烦的，我们使用apache提供的一个Zookeeper客户端来实现



http://curator.apache.org



```xml
<dependency>
    <groupId>org.apache.curator</groupId>
    <artifactId>curator-recipes</artifactId>
    <version>4.2.0</version>
</dependency>
```



recipes是curator族谱大全，里面包含Zookeeper和framework







### 使用配置对象的方式创建curator客户端工具



在controller**执行业务代码之前加入分布式锁的逻辑代码：**

```java
@ResponseBody
@GetMapping("/product/reduce")
public Object reduce(@RequestParam("id") int id) throws Exception {
    String connectString = "192.168.150.128:2181,192.168.150.129:2181,192.168.150.130:2181";

    //1.创建curator工具对象
    //重试策略（1000ms 试一次，最多试三次）
    RetryPolicy retryPolicy = new ExponentialBackoffRetry(1000, 3);
    CuratorFramework client = CuratorFrameworkFactory.newClient(connectString, retryPolicy);
    client.start();
    
    //2.根据客户端工具对象创建"内部互斥锁"
    //内部创建   临时有序节点
    InterProcessMutex lock = new InterProcessMutex(client, "/product_"+id);

    //3.加锁
    try{
        lock.acquire();
        //减去库存
        productService.reduceStock(id);
    } catch (Exception e) {
        if(e instanceof RuntimeException){
            throw e;
        }
    } finally {
        //4.释放锁
        lock.release();
    }
    return "ok";
}
```





### 使用分布式锁后再次测试：







![image-20210302015818026](../picture/Zookeeper%E8%AF%A6%E8%A7%A3/image-20210302015818026.png)



每次加一，后面的监听前面的

![image-20210302020213495](../picture/Zookeeper%E8%AF%A6%E8%A7%A3/image-20210302020213495.png)



可以在Zookeeper客户端中监听到：

![image-20210302021215225](../picture/Zookeeper%E8%AF%A6%E8%A7%A3/image-20210302021215225.png)



**在Zookeeper`/product_1`节点下不断有新的有序节点创建并销毁**











































# tips





## 使用springboot启动两个不同端口的程序



通过新增profile，增加一个配置文件，在执行时选择要执行的配置文件

https://blog.csdn.net/m0_37564404/article/details/81512650



![image-20210301232605202](../picture/Zookeeper%E8%AF%A6%E8%A7%A3/image-20210301232605202.png)









