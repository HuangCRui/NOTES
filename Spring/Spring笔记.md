

[TOC]

# Spring概述



Spring是分层的java SE/EE应用 full-stack 轻量级开源框架，以 **IoC（Inverse Of Control：反转控制）**和 **AOP（Aspect Oriented Programming：面向切面编程）**为内核。

提供了**展现层 SpringMVC和持久层 Spring JDBCTemplate**以及**业务层事务管理**等众多的企业级应用技术，还能整合开源世界众多著名的第三方框架和类库，逐渐成为使用最多的Java EE 企业应用开源框架





## Spring的优势



- 方便解耦，简化开发

  通过提供的IoC容器，将对象间的依赖关系交由Spring进行控制。

- AOP编程的支持

- 声明式事务的支持

- 方便程序的测试

- 方便集成各种优秀的框架

- 降低JavaEE API的使用难度，进行了封装

- Java源码经典学习范例



## Spring体系结构

![image-20210125162519777](../picture/Spring%E7%AC%94%E8%AE%B0/image-20210125162519777.png)



**核心容器**





# 快速入门



 ![image-20210125162902479](../picture/Spring%E7%AC%94%E8%AE%B0/image-20210125162902479.png)



要进行**解耦合**



通过配置文件



找框架要这个Dao对象

![image-20210125163811578](../picture/Spring%E7%AC%94%E8%AE%B0/image-20210125163811578.png)





①导入 Spring 开发的基本包坐标

②编写 **Dao 接口和实现类**

③创建 Spring 核心**配置文件**

④在 Spring 配置文件中**配置** UserDaoImpl

⑤使用 **Spring 的 API 获得 Bean 实例**



---



1. 添加springcontext依赖

   ```xml
   <dependency>
       <groupId>org.springframework</groupId>
       <artifactId>spring-context</artifactId>
       <version>5.1.2.RELEASE</version>
   </dependency>
   ```

2. 编写Dao接口和实现类

3. spring核心配置文件

   `在类路径下（resources）创建applicationContext.xml配置文件`，并配置UserDao的实现类UserDaoImpl

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    
    <bean id="userDao" class="dao.impl.UserDaoImpl"/>
</beans>
```

4. 使用Spring的API获得Bean实例

   ```java
   public static void main(String[] args) {
       ApplicationContext app = new ClassPathXmlApplicationContext("applicationContext.xml");
       UserDao userDao = (UserDao) app.getBean("userDao");
       userDao.save();//save running
   }
   ```







# Spring配置文件





## Bean标签基本配置

用于配置对象**交由Spring 来创建**。

默认情况下它调用的是类中的**无参构造函数**，如果**没有无参构造函数则不能创建成功。**



基本属性：

- id：**Bean实例**在Spring容器中的**唯一**标识

- class：Bean的全限定名称





## Bean标签的范围配置

scope：指对象的作用范围



| 取值范围         | 说明                                                         |
| ---------------- | ------------------------------------------------------------ |
| singleton        | **默认**值，单例的                                           |
| prototype        | 多例的（**多个对象**）                                       |
| request          | WEB   项目中，Spring   创建一个   Bean   的对象，将对象存入到   request   **域**中 |
| session          | WEB   项目中，Spring   创建一个   Bean   的对象，将对象存入到   session   **域**中 |
| global   session | WEB   项目中，应用在   Portlet   环境，如果没有   Portlet   环境那么globalSession   相当于   session |

```java
    @Test
    //测试scope属性
    public void test1(){
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
        UserDao userDao1 = (UserDao) applicationContext.getBean("userDao");
        UserDao userDao2 = (UserDao) applicationContext.getBean("userDao");
        System.out.println(userDao1);
        System.out.println(userDao2);
        /*
        singleton单例:
        dao.impl.UserDao@5d908d47
        dao.impl.UserDao@5d908d47
        prototype多例:
        dao.impl.UserDao@508dec2b
        dao.impl.UserDao@1e4f4a5c
        */
    }
```



1）当scope的取值为singleton时

​      Bean的实例化个数：**1个**

​      Bean的实例化时机：当Spring核心文件**被加载时，实例化配置的Bean实例**

​      Bean的生命周期：

对象创建：当应用**加载**，创建容器时，对象就被创建了

对象运行：**只要容器在，对象一直活着**

对象销毁：当应用卸载，销毁容器时，对象就被销毁了



```java
//在单例模式下,配置文件被加载的时候，UserDao就被创建了
//并且就只被创建一次
ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
```





2）当scope的取值为prototype时

​      Bean的实例化个数：**多个**

​      Bean的实例化时机：当**调用getBean()方法时实例化Bean**

对象创建：当**使用对象时**，创建新的对象实例

对象运行：只要对象在使用中，就一直活着

对象销毁：当对象长时间不用时，被 **Java 的垃圾回收器**回收了

```java
//多例模式下，加载配置文件时，对象没有被创建
ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
//这一步才开始创建，每次创建一个不同的对象   getBean()时才创建
UserDao userDao1 = (UserDao) applicationContext.getBean("userDao");
UserDao userDao2 = (UserDao) applicationContext.getBean("userDao");
```



## Bean生命周期配置



init-method：指定类中的初始化方法名称

destroy-method：指定类中销毁方法名称



**对象先创建，再执行init**

销毁方法？手动关闭

```java
ClassPathXmlApplicationContext classPathXmlApplicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
UserDao userDao1 = (UserDao) classPathXmlApplicationContext.getBean("userDao");
//手动关闭
classPathXmlApplicationContext.close();
```



## Bean实例化的三种方式



1. 使用无参构造方法实例化

    它会根据默认无参构造方法来创建类对象，如果bean中没有默认无参构造函数，将会创建失败

```xml
<bean id="userDao" class="dao.impl.UserDaoImpl" scope="singleton" init-method="init" destroy-method="destroy"/>
```





2. 工厂**静态方法**实例化

工厂的静态方法返回Bean实例

```java
public class StaticFactory {
    public static UserDao getUserDao(){
        System.out.println("factory!");
        return new UserDaoImpl();
    }
}
```

```xml
<bean id="userDao" class="factory.StaticFactory" factory-method="getUserDao"/>
```

不需要**工厂对象**，可以直接调用静态方法







3. 工厂**实例方法**实例化



```java
public class DynamicFactory {
    public UserDao getUD(){
        System.out.println("dynamic factory!");
        return new UserDaoImpl();
    }
}
```



```xml
<bean id="dynfac" class="factory.DynamicFactory"/>
<bean id="userDao" factory-bean="dynfac" factory-method="getUD"/>
```





并不是都使用**无参构造**



## Bean的依赖注入入门





业务层-----web层



```java
public class UserServiceImpl implements UserService {
    @Override
    public void save() {
        System.out.println("userservice");
        //获取dao对象
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
        UserDao userDao = (UserDao) applicationContext.getBean("userDao");
        userDao.save();
    }
}
```

```xml
<bean id="userServ" class="service.impl.UserServiceImpl"/>
```

```java
public class UserController {
    public static void main(String[] args) {
        //配到容器中？让spring来产生
//        UserService userService = new UserServiceImpl();
//        userService.save();
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
        UserService userService = (UserService)applicationContext.getBean("userServ");
        userService.save();
    }
}
```

**问题在哪？？**





## Bean的依赖注入概念



业务层中用到userdao

web层只操作service

![image-20210125210203124](../picture/Spring%E7%AC%94%E8%AE%B0/image-20210125210203124.png)

实际上web层用的是service

**不管dao是怎么获取的**

**可以在Spring容器中，将UserDao设置到UserService内部 **



![image-20210125210400403](../picture/Spring%E7%AC%94%E8%AE%B0/image-20210125210400403.png)



---



**依赖注入（Dependency Injection）**：它是 Spring 框架核心 **IOC 的具体实现**。

**Service需要Dao的依赖注入**

在编写程序时，通过控制反转，把**对象的创建交给了 Spring**，但是代码中不可能出现没有**依赖**的情况。



IOC 解耦只是**降低他们的依赖关系**，但不会消除。例如：业务层仍会调用持久层的方法。

**解决方法：配到容器中**

那这种业务层和持久层的依赖关系，在使用 Spring 之后，就让 **Spring 来维护**了。

简单的说，就是坐等框架***把持久层对象传入业务层*** ，而**不用我们自己去获取**





---



如何在容器内部将Dao给Service？？？

将一个对象设置到另一个对象**内部**



- set
- 有参构造



```java
public class UserServiceImpl implements UserService {
    @Override
    public void save() {
        //都不需要了
        System.out.println("userservice");
//        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
//        UserDao userDao = (UserDao) applicationContext.getBean("userDao");
        userDao.save();
    }
    //告诉spring
    private UserDao userDao;
    public void setUserDao(UserDao userDao) {
        this.userDao = userDao;
    }
}
```

property name  使用 **set方法set后的名字，首字母小写**

```xml
<bean id="userDao" class="dao.impl.UserDaoImpl"/>
<bean id="userServ" class="service.impl.UserServiceImpl">
    <property name="userDao" ref="userDao"></property>
</bean>
```

```java
public static void main(String[] args) {
//        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
//        UserService userService = (UserService)applicationContext.getBean("userServ");
//        userService.save();

        //空指针 userDao空-- 这个service不是从容器中拿的，容器中的service才注入了dao，而自己new的就没有dao对象
        UserService userService = new UserServiceImpl();
        userService.save();//java.lang.NullPointerException
    }
```

---



set方法：P命名空间注入



P命名空间注入本质也是set方法注入，但比起上述的set方法注入更加方便，主要体现在配置文件中，如下：



首先，需要引入P命名空间：

```xml
xmlns:p="http://www.springframework.org/schema/p"    

<bean id="userDao" class="dao.impl.UserDaoImpl"/>
<!--    <bean id="userServ" class="service.impl.UserServiceImpl">-->
<!--        <property name="userDao" ref="userDao"></property>-->
<!--    </bean>-->
    <bean id="userServ" class="service.impl.UserServiceImpl" p:userDao-ref="userDao"/>
```



---



有参构造注入



```xml
<bean id="userDao" class="dao.impl.UserDaoImpl"/>
    <bean id="userServ" class="service.impl.UserServiceImpl">
<!--        什么都不写，那么就是默认无参构造，没有进行dao对象赋值-->
<!--        这里name是 构造方法函数的 参数名   ref是引用容器中的参数名-->
        <constructor-arg ref="userDao" name="userDao"/>
    </bean>
```



```java
//告诉spring用什么方法来注入的
private UserDao userDao;
public UserServiceImpl(UserDao userDao) {
    this.userDao = userDao;
}
public UserServiceImpl() {
}
```

![image-20210126023614808](../picture/Spring%E7%AC%94%E8%AE%B0/image-20210126023614808.png)







## Bean的依赖注入的数据类型



上面的操作，都是注入的**引用Bean**，除了**对象的引用**可以注入，**普通数据类型，集合**等都可以在容器中进行注入。

注入数据的三种数据类型 

- 普通数据类型

- 引用数据类型

- 集合数据类型





其中引用数据类型，此处就不再赘述了，之前的操作都是对UserDao对象的引用进行注入的，下面将以set方法注入为例，演示普通数据类型和集合数据类型的注入。



```java
<bean id="userDao" class="dao.impl.UserDaoImpl">
    <property name="age" value="21"/>
    <property name="username" value="阿克曼"/>
</bean>
```



```java
private String username;
private int age;

public void setUsername(String username) {
    this.username = username;
}

public void setAge(int age) {
    this.age = age;
}
```



---



集合类型注入

> spring底层使用集合接口的实现类来对其赋值：
>
> java.util.ArrayList
> java.util.LinkedHashMap
>
> ...

```xml
 <bean id="userDao" class="dao.impl.UserDaoImpl">
            <property name="strlist" >
                <list>
<!--                    普通字符串 基本类型-->
                    <value>阿克曼</value>
                    <value>大司人</value>
                    <value>tonyMa</value>
                </list>
            </property>
            <property name="userMap">
                <map>
<!--                    User类型，引用，也需要spring注入，配置文件-->
                    <entry key="阿克曼" value-ref="user1"/>
                    <entry key="天津人" value-ref="user2"/>
                </map>
            </property>
            <property name="properties">
                <props>
                    <prop key="p1">v1</prop>
                    <prop key="p2">v2</prop>
                    <prop key="p3">v3</prop>
                </props>
            </property>
        </bean>

<!--    User也有属性，set，注入。。。-->
        <bean id="user1" class="domain.User">
            <property name="addr" value="芜湖"/>
            <property name="name" value="马老c"/>
        </bean>
        <bean id="user2" class="domain.User">
            <property name="addr" value="天津"/>
            <property name="name" value="结界"/>
        </bean>
```



**一样：需要set方法和属性**

```java
private List<String> strlist;
private Map<String, User> userMap;
private Properties properties;

public void setStrlist(List<String> strlist) {
    this.strlist = strlist;
}

public void setUserMap(Map<String, User> userMap) {
    this.userMap = userMap;
}

public void setProperties(Properties properties) {
    this.properties = properties;
}
```



---



集合数据类型（`List<User>`）的注入

```java
public class UserDaoImpl implements UserDao {
	private List<User> userList;
	public void setUserList(List<User> userList) {
	this.userList = userList;  
 }
public void save() {
	System.out.println(userList);
	System.out.println("UserDao save method running....");
	}
}
```

```xml
<bean id="u1" class="com.itheima.domain.User"/>
<bean id="u2" class="com.itheima.domain.User"/>
<!-- 这里没在User中添加属性（set方法/构造）-->
<bean id="userDao" class="dao.impl.UserDaoImpl">
    <property name="userList">
        <list>
            <bean class="domain.User"/><!--可能是临时创造的？-->
            <bean class="domain.User"/>
            <ref bean="u1"/><!-- 使用spring容器注入的User对象-->
            <ref bean="u2"/>       
        </list>
    </property>
</bean>
```







## 引入其他配置文件



**分模块开发，进行拆解  （用户模块？商业？）**



实际开发中，Spring的配置内容非常多，这就导致Spring配置很繁杂且体积很大，所以，可以将部分配置拆解到其他配置文件中，而在Spring主配置文件通过import标签进行加载

```xml
<import resource="applicationContext-User.xml"/>
```





![image-20210126032638832](../picture/Spring%E7%AC%94%E8%AE%B0/image-20210126032638832.png)





# Spring相关API



ApplicationContext  ：**接口**类型，代表**应用上下文**，可以**通过其实例获得 Spring 容器中的 Bean 对象**



```java
ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
UserDao userDao = (UserDao) applicationContext.getBean("userDao");
userDao.save();
```

![image-20210126032839498](../picture/Spring%E7%AC%94%E8%AE%B0/image-20210126032839498.png)





## ApplicationContext的实现类

1）**ClassPathXml**ApplicationContext 

​      它是从**类的根路径**下加载配置文件 推荐使用这种

> org.springframework.beans.factory.xml.XmlBeanDefinitionReader - Loaded 2 bean definitions **from class path resource** [applicationContext-User.xml]

![image-20210126033554550](../picture/Spring%E7%AC%94%E8%AE%B0/image-20210126033554550.png)



2）**FileSystemXml**ApplicationContext 

​      它是从**磁盘路径**上加载配置文件，配置文件可以在磁盘的任意位置。

```java
FileSystemXmlApplicationContext fileSystemXmlApplicationContext =
        new FileSystemXmlApplicationContext("D:\\desktop\\Code\\springlearning1\\src\\main\\resources\\applicationContext.xml");
UserDao userDao = (UserDao) fileSystemXmlApplicationContext.getBean("userDao");
```



3）AnnotationConfigApplicationContext

​      当**使用注解**配置容器对象时，需要使用此类来创建 spring 容器。它用来读取注解。



## getBean方法使用



```java
//传入bean的id
public Object getBean(String name) throws BeansException {
    this.assertBeanFactoryActive();
    return this.getBeanFactory().getBean(name);
}

//传入字节码对象类型  按照类型从容器中
public <T> T getBean(Class<T> requiredType) throws BeansException {
    this.assertBeanFactoryActive();
    return this.getBeanFactory().getBean(requiredType);
}
```

```java
UserDao userDao = applicationContext.getBean(UserDao.class);//不用强转，已经告诉什么类型了
```



使用id可以唯一区分

容器中存在**多个**某类型的对象，没法使用传入Class对象来获取对象，必须使用唯一的id获取







# Spring配置数据源



bean都是自己配置好的，使用容器产生非自定义的一些对象



## 数据源（连接池）的作用



数据源(连接池)是提高**程序性能**出现的

- 事先**实例化数据源**，**初始化**部分连接资源

- 使用连接资源时从数据源中获取

- 使用完毕后将连接资源归还给数据源



常见的数据源(连接池)：DBCP、C3P0、BoneCP、Druid等



**开发步骤**

①导入数据源的坐标和数据库驱动坐标

②创建数据源对象

③设置数据源的基本连接数据

④使用数据源获取连接资源和归还连接资源







## 数据源的手动创建



添加依赖包

```xml
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>8.0.13</version>
</dependency>
<dependency>
    <groupId>c3p0</groupId>
    <artifactId>c3p0</artifactId>
    <version>RELEASE</version>
</dependency>
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid</artifactId>
    <version>RELEASE</version>
</dependency>
```



```java
@Test
//手动创建c3p0数据源
public void test1() throws PropertyVetoException, SQLException {
    ComboPooledDataSource dataSource = new ComboPooledDataSource();
    dataSource.setDriverClass("com.mysql.cj.jdbc.Driver");
    dataSource.setJdbcUrl("jdbc:mysql://localhost:3306/classes?useSSL=false&serverTimezone=GMT%2B8");
    dataSource.setUser("root");
    dataSource.setPassword("huangchenrui20");
    //拿到资源并使用
    Connection connection = dataSource.getConnection();
    System.out.println(connection);//com.mchange.v2.c3p0.impl.NewProxyConnection@5876a9af 成功
    connection.close();
}
```





```java
@Test
//druid数据源
public void test2() throws SQLException {
    DruidDataSource druidDataSource = new DruidDataSource();
    druidDataSource.setDriverClassName("com.mysql.cj.jdbc.Driver");
    druidDataSource.setUrl("jdbc:mysql://localhost:3306/classes?useSSL=false&serverTimezone=GMT%2B8");
    druidDataSource.setUsername("root");//不是setname....
    druidDataSource.setPassword("huangchenrui20");
    Connection connection = druidDataSource.getConnection();
    System.out.println(connection);
    connection.close();
}
```



-----



很多参数设置，更换数据库？需要找到源码来修改。**耦合**



**抽取到properties配置文件**

```properties
jdbc.driver=com.mysql.cj.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/classes?useSSL=false&serverTimezone=GMT%2B8
jdbc.username=root
jdbc.password=huangchenrui20
```



```java
@Test
//手动创建c3p0数据源
public void test1() throws PropertyVetoException, SQLException {
    //读取配置文件
    ResourceBundle resourceBundle = ResourceBundle.getBundle("jdbc");//相对于类加载路径  只要基本名称就行
    String driver = resourceBundle.getString("jdbc.driver");
    String url = resourceBundle.getString("jdbc.url");
    String username = resourceBundle.getString("jdbc.username");
    String password = resourceBundle.getString("jdbc.password");
    //创建数据源对象 设置连接参数
    ComboPooledDataSource dataSource = new ComboPooledDataSource();

    dataSource.setDriverClass(driver);
    dataSource.setJdbcUrl(url);
    dataSource.setUser(username);
    dataSource.setPassword(password);
    //拿到资源并使用
    Connection connection = dataSource.getConnection();
    System.out.println(connection);//com.mchange.v2.c3p0.impl.NewProxyConnection@5876a9af 成功
    connection.close();
}
```





## Spring配置数据源



第三方的bean也可以使用spring来配置



**数据源才创建时也有无参构造**

```java
//创建数据源对象 设置连接参数
ComboPooledDataSource dataSource = new ComboPooledDataSource();
```

**可以通过set方法注入**

```java
dataSource.setDriverClass(driver);
dataSource.setJdbcUrl(url);
dataSource.setUser(username);
dataSource.setPassword(password);
```





```xml
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-context</artifactId>
    <version>5.3.1</version>
</dependency>
```



不用再去写dao接口了



```xml
    <bean id="datasource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
<!--        这里的name是set方法后面的名字-->
        <property name="driverClass" value="com.mysql.cj.jdbc.Driver"/>
        <property name="jdbcUrl" value="jdbc:mysql://localhost:3306/classes?useSSL=false&amp;serverTimezone=GMT%2B8"/>
        <property name="user" value="root"/>
        <property name="password" value="huangchenrui20"/>
    </bean>
```



```java
@Test
public void test3() throws SQLException {
    ClassPathXmlApplicationContext classPathXmlApplicationContext = new ClassPathXmlApplicationContext("ApplicationContext.xml");
    ComboPooledDataSource bean = classPathXmlApplicationContext.getBean(ComboPooledDataSource.class);//容器中只有一个该类对象
    //ComboPooledDataSource comboPooledDataSource = (ComboPooledDataSource) classPathXmlApplicationContext.getBean("datasource");
    Connection connection = bean.getConnection();
    System.out.println(connection);
    connection.close();
}
```



## 抽取jdbc配置文件

**在spring配置文件中，也是写死的具体的参数**

但在xml配置文件中，也是解耦的。不需要修改源码，只用修改xml配置文件中的bean。

**数据库配置信息往往需要单独提出来一个配置文件。每个配置文件专注于自己的范围**

---



applicationContext.xml加载jdbc.properties配置文件获得连接信息。

首先，需要引入context命名空间和约束路径：

命名空间：xmlns:context="http://www.springframework.org/schema/context"

约束路径：http://www.springframework.org/schema/context

​                   http://www.springframework.org/schema/context/spring-context.xsd





```xml
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">

<!--    加载外部的properties配置文件-->
    <context:property-placeholder location="classpath:jdbc.properties"/>

    <bean id="datasource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
<!--        这里的name是set方法后面的名字-->
        <property name="driverClass" value="${jdbc.driver}"/>
        <property name="jdbcUrl" value="${jdbc.url}"/>
        <property name="user" value="${jdbc.username}"/>
        <property name="password" value="${jdbc.password}"/>
    </bean>
```

**类加载路径target/classes**

![image-20210126174028608](../picture/Spring%E7%AC%94%E8%AE%B0/image-20210126174028608.png)







---



Spring容器加载properties文件

```xml
<context:property-placeholder location="xx.properties"/>
<property name="" value="${key}"/>
```







# Spring注解开发



Spring是**轻代码而重配置**的框架，**配置比较繁重**，影响开发效率，所以**注解开发**是一种趋势，注解代替xml配置文件可以简化配置，提高开发效率。 





## Spring原始注解



**Spring原始注解主要是替代<Bean>的配置**

| 注解           | 说明                                                         |
| -------------- | ------------------------------------------------------------ |
| @Component     | 使用在类上用于实例化Bean（替代bean标签）在类上加个注解就行   |
| @Controller    | 使用在**web层**类上用于实例化Bean                            |
| @Service       | 使用在**service层**类上用于实例化Bean                        |
| @Repository    | 使用在**dao层**类上用于实例化Bean       **语义化功能  和component一样** |
| **@Autowired** | 使用在字段上用于根据类型**依赖注入**                         |
| @Qualifier     | 结合@Autowired一起使用用于根据名称进行依赖注入               |
| **@Resource**  | 相当于@Autowired+@Qualifier，**按照名称**进行注入   **↑这三个用于注入   这三个标签用来替代ref  引用注入** |
| @Value         | 注入普通属性                                                 |
| @Scope         | 标注Bean的作用范围                                           |
| @PostConstruct | 使用在方法上标注该方法是Bean的**初始化方法**init method      |
| @PreDestroy    | 使用在方法上标注该方法是Bean的**销毁方法**destroy method     |







```java
//    <bean id="userDao" class="dao.impl.UserDaoImpl"/>
@Component("userDao")
public class UserDaoImpl  implements UserDao {
```



**set方法可以删掉**



`@Autowired`：按照数据类型**从spring容器中进行匹配**的（**从容器中找UserDao类型的bean**），找到后，直接注入进去

**如果容器中有多个同类型的bean**，就不知道去注入哪个



`@Qualifier("userDao")` **按照id值  从容器中进行匹配的，但是此处的@Qualifier结合@Autowired一起使用**



> 如果只要按照**类型**注入，只写`@Autowired`就行
>
> 如果想按照名称注入，还要再 写一个`@Qualifier`

```java
//    <bean id="userService" class="service.impl.UserServiceImpl">
@Component("userService")
public class UserServiceImpl  implements UserService {

    //<property name="userDao" ref="userDao"/>
    @Autowired
    @Qualifier("userDao")
    private UserDao userDao;

    
    @Override
    public void save() {
        userDao.save();
    }
    public void setUserDao(UserDao userDao) {
        this.userDao = userDao;
    }
}
```





![image-20210126180716861](../picture/Spring%E7%AC%94%E8%AE%B0/image-20210126180716861.png)



**需要告诉spring哪里有注解？？**

> 注意：
>
> 使用注解进行开发时，需要在applicationContext.xml中配置组件扫描，作用是指定哪个包及其子包下的Bean需要进行扫描以便识别使用注解配置的类、字段和方法。

```xml
<!--    配置组件扫描-->
<!--    扫dao 和 service包中所有的bean-->
    <context:component-scan base-package="dao"/>
    <context:component-scan base-package="service"/>
```





---





`@Resource`相当于@Autowired+ @Qualifier

```java
//    <bean id="userService" class="service.impl.UserServiceImpl">
//@Component("userService")
@Service("userService")
public class UserServiceImpl  implements UserService {

    //<property name="userDao" ref="userDao"/>
//    @Autowired
//    @Qualifier("userDao")
    @Resource(name = "userDao")
    private UserDao userDao;
    
    
    
      
//    <bean id="userDao" class="dao.impl.UserDaoImpl"/>
//@Component("userDao")
@Repository("userDao")
public class UserDaoImpl  implements UserDao {
```



---



注入普通数据类型

```java
//@Value("dsm")没啥用
@Value("${jdbc.username}")//已经加载到了容器内
private String username;

@PostConstruct
public void init(){
    System.out.println("service init");
}
@PreDestroy
public void destroy(){
    System.out.println("service destroy");
}
```







## Spring新注解



 有一些配置，按照原始注解是无法替代的

现在只是代替了一些自定义的Bean，加上注解

对于非自定义的bean：dataSource？

 

---

使用上面的注解还不能全部替代xml配置文件，还需要使用注解替代的配置如下：

**非自定义的**Bean的配置：`<bean>`

加载**properties文件**的配置：`<context:property-placeholder>`

**组件扫描**的配置：`<context:component-scan>`

**引入**其他文件：`<import>`





| 注解            | 说明                                                         |
| --------------- | ------------------------------------------------------------ |
| @Configuration  | 用于指定当前类是一个 **Spring  配置类**，当**创建容器**时会**从该类上加载注解** |
| @ComponentScan  | 用于指定 Spring   在初始化容器时**要扫描的包**。   作用和在 Spring   的 xml 配置文件中 的   <context:component-scan   base-package="com.itheima"/>一样 |
| @Bean           | **使用在*方法*上 **，标注将该方法的**返回值**存储到   Spring   容器中 |
| @PropertySource | 用于加载**.properties 文件**中的配置                         |
| @Import         | 用于导入其他配置类                                           |



---



核心配置类



```java
package config;

//标志该类是Spring的核心配置类   类代替文件  注解代替标签
//<!--    配置组件扫描-->
//<!--    扫dao 和 service包中所有的bean-->
//    <context:component-scan base-package="dao"/>
//    <context:component-scan base-package="service"/>
@Configuration
@ComponentScan({"dao", "service"})
//<!--    加载外部的properties配置文件-->
//    <context:property-placeholder location="classpath:jdbc.properties"/>
@PropertySource("classpath:jdbc.properties")
public class SpringConfiguration {

    /*
        <bean id="datasource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
<!--        这里的name是set方法后面的名字-->
        <property name="driverClass" value="${jdbc.driver}"/>
        <property name="jdbcUrl" value="${jdbc.url}"/>
        <property name="user" value="${jdbc.username}"/>
        <property name="password" value="${jdbc.password}"/> 
    </bean>
     */

    //jdbc.properties此时在容器中，可以注入
    @Value("${jdbc.driver}")
    private String driver;
    @Value("${jdbc.url}")
    private String url;
    @Value("${jdbc.username}")
    private String userName;
    @Value("${jdbc.password}")
    private String passWord;

    @Bean("dataSource")//spring会将当前方法的返回值以指定名称存储到spring容器中
    //但不能直接写入参数，需要解耦合，加载配置文件
    public DataSource getDataSource() throws PropertyVetoException {
        ComboPooledDataSource dataSource = new ComboPooledDataSource();
        dataSource.setDriverClass(driver);
        dataSource.setJdbcUrl(url);
        dataSource.setUser(userName);
        dataSource.setPassword(passWord);
        return dataSource;
    }

}
```





```java
public static void main(String[] args) throws SQLException {
    //ApplicationContext applicationContext = new ClassPathXmlApplicationContext("ApplicationContext.xml");
    
    //专门加载核心配置类
    AnnotationConfigApplicationContext annotationConfigApplicationContext =
            new AnnotationConfigApplicationContext(SpringConfiguration.class);
    UserService userService = (UserService) annotationConfigApplicationContext.getBean("userService");
    userService.save();
    DataSource dataSource = (DataSource) annotationConfigApplicationContext.getBean("dataSource");
    System.out.println(dataSource.getConnection());
}
```







# Spring整合Junit



在测试类中，没法测试方法都有以下两行代码：

```java
 ApplicationContext ac = new ClassPathXmlApplicationContext("bean.xml");
 IAccountService as = ac.getBean("accountService",IAccountService.class);
```

这两行代码的作用是获取容器，如果不写的话，直接会提示空指针异常。所以又不能轻易删掉。

![image-20210126204954146](../picture/Spring%E7%AC%94%E8%AE%B0/image-20210126204954146.png)

每个测试都有这两句





---

解决思路：



让**SpringJunit负责创建Spring容器**，但是需要将配置文件的名称告诉它

将需要进行测试Bean直接**在测试类中进行注入**



## Spring集成Junit步骤与实现



①导入spring集成Junit的坐标



```xml
<dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.12</version>
    <scope>test</scope>
</dependency>
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-test</artifactId>
    <version>5.3.1</version>
</dependency>
```



②使用@Runwith注解替换原来的运行期

③使用@ContextConfiguration指定配置文件或配置类

④使用@Autowired注入需要测试的对象

⑤创建测试方法进行测试



```java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration("classpath:ApplicationContext.xml")
public class SpringJunitTest {

    @Autowired
    private UserService userService;

    @Autowired
    private DataSource dataSource;

    @Test
    public void test1() throws SQLException {
        userService.save();
        System.out.println(dataSource.getConnection());
    }
}
```



配置类的字节码

```java
@RunWith(SpringJUnit4ClassRunner.class)
//@ContextConfiguration("classpath:ApplicationContext.xml")
@ContextConfiguration(classes = SpringConfiguration.class)
public class SpringJunitTest {
```









# Spring AOP



AOP 为 Aspect Oriented Programming 的缩写，意思为**面向切面编程**，是通**过预编译方式**和**运行期动态代理（不修改源码，完成程序功能之间的松耦合）**实现程序功能的统一维护的一种技术。

AOP 是 OOP 的延续，是软件开发中的一个热点，也是Spring框架中的一个重要内容，是函数式编程的一种衍生范型。利用AOP可以**对业务逻辑的各个部分进行隔离**，从而使得业务逻辑各部分之间的**耦合度降低**，提高程序的**可重用性**，同时提高了开发的效率。



## AOP的作用及其优势

**作用：在程序运行期间，在不修改源码的情况下对方法进行功能增强**，都配置上某一个功能

​		**抽取代码**

优势：减少重复代码，提高开发效率，并且便于维护



> B、C、D功能都需要A功能，本来是需要引那个功能，AOP配置使得B、C、D都具备A功能

![image-20210127152617133](../picture/Spring%E7%AC%94%E8%AE%B0/image-20210127152617133.png)

**都需要改，维护困难**

抽取出来？都需要引用日志控制方法

![image-20210127152712118](../picture/Spring%E7%AC%94%E8%AE%B0/image-20210127152712118.png)

**耦合死了！**，从代码角度在一起了

![image-20210127152943062](../picture/Spring%E7%AC%94%E8%AE%B0/image-20210127152943062.png)

这时候，这两段代码就没任何关系了



**通过配置文件，告诉他们相结合**

解耦---



> 简要理解：切面：目标方法+功能增强



**AOP的底层实现**

实际上，AOP 的底层是通过 Spring 提供的的**动态代理技术**实现的。在运行期间，Spring通过动态代理技术动态地生成代理对象，代理对象方法执行时进行增强功能的介入，在去调用目标对象的方法，从而完成功能的增强。







## AOP的动态代理技术

常用的动态代理技术

JDK 代理 : 基于**接口**的动态代理技术，**基于接口**动态生成代理对象，保证代理对象和目标对象有相同的方法

cglib 代理：基于**父类**的动态代理技术，没有接口，cglib为目标对象 **动态生成子对象，子对象功能进行增强**，*不是继承*

![image-20210127151855139](../picture/Spring%E7%AC%94%E8%AE%B0/image-20210127151855139.png)



### JDK的动态代理



**底层实现**：

```java
public static void main(String[] args) {

    //创建目标对象
    Target target = new Target();
    //获得增强对象
    Advice advice = new Advice();

    //返回值  就是动态生成的代理对象
    //代理对象和目标对象是兄弟关系，用接口来接受
    TargetInterface proxy = (TargetInterface) Proxy.newProxyInstance(
            target.getClass().getClassLoader(),//目标对象类加载器
            target.getClass().getInterfaces(),//目标对象相同的接口字节码对象数组
            new InvocationHandler() {
                @Override
                public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                    advice.before();//前置增强
                    Object invoke = method.invoke(target, args);//执行目标方法
                    advice.after();//后置增强
                    return invoke;
                }
                //调用代理对象的任何方法，实质执行的都是invoke方法
            }
    );

    //调用代理对象的方法
    proxy.save();//save running
}
```





### cglib的动态代理



spring将cglib集成到spring-core里了

![image-20210127164451110](../picture/Spring%E7%AC%94%E8%AE%B0/image-20210127164451110.png)





```java
public static void main(String[] args) {

    //创建目标对象
    Target target = new Target();
    //获得增强对象
    Advice advice = new Advice();

    //返回值  就是动态生成的代理对象  基于cglib
    //1.创建增强器
    Enhancer enhancer = new Enhancer();
    //2.设置父类（目标）
    enhancer.setSuperclass(Target.class);
    //3.设置回调
    enhancer.setCallback(new MethodInterceptor() {
        @Override
        public Object intercept(Object o, Method method, Object[] objects, MethodProxy methodProxy) throws Throwable {
            advice.before();//执行前置
            method.invoke(target, objects);//执行目标
            advice.after(); //执行后置
            return null;
        }
    });
    //3.创建代理对象  可以使用Target接，生成的是子类
    Target proxy = (Target) enhancer.create();

    //调用代理对象的方法
    proxy.save();//save running
}
```





## AOP相关概念



Spring 的 AOP 实现底层就是对上面的动态代理的代码进行了**封装**，封装后我们只需要对需要关注的部分进行代码编写，并通过配置的方式完成指定目标的方法增强。

在正式讲解 AOP 的操作之前，我们必须理解 AOP 的相关术语，常用的术语如下：

- Target（目标对象）：代理的目标对象（**要被增强的对象**）

- Proxy （代理）：一个类被 AOP 织入增强后，就产生一个**结果代理类**

- Joinpoint（连接点）：所谓连接点是指那些被**拦截到的点**。在spring中,这些点指的是**方法**，因为spring只支持**方法类型**的连接点（拦截到，才能呢进行增强）----***可以*  被增强的方法**

- Pointcut（切入点）：所谓切入点是指我们要对哪些 Joinpoint 进行拦截的定义（**要对哪些连接点进行配置，真正被增强的**）

- Advice（通知/ 增强）：所谓通知是指拦截到 Joinpoint 之后所要做的事情就是通知

- Aspect（切面）：是**切入点和通知（引介）的结合**

  Spring AOP就是负责实施切面的框架, ***它将切面所定义的横切逻辑织入到切面所指定的连接点中***. AOP的工作重心在于如何***将增强织入目标对象的连接点上***, 这里包含两个工作:

  1. 如何通过 pointcut 和 advice 定位到特定的 joinpoint 上
  2. 如何在 advice 中编写切面代码.

  **可以简单地认为, 使用 @Aspect 注解的类就是切面.**

- Weaving（织入）**动词**：是指**把增强应用到目标对象**来**创建新的代理对象**的过程。spring采用动态代理织入，而AspectJ采用编译期织入和类装载期织入







## AOP开发明确的事项



1. 需要编写的内容

   - 编写核心业务代码（**目标类的目标方法**）
   - 编写**切面类**，切面类中有**通知(增强功能方法)**
   - 在配置文件中，**配置织入关系**，即将哪些通知与哪些连接点进行**结合**

2. AOP技术实现的内容

   Spring 框架**监控切入点方法的执行**。一旦监控到切入点方法被运行，**使用代理机制**，**动态创建切点所在目标对象的  *代理对象***，根据通知**类别（前置/后置）**，在代理对象的**对应位置**，将通知对应的功能**织入**，完成完整的代码逻辑运行。

3. AOP 底层使用哪种代理方式

   在 spring 中，框架会**根据目标类是否实现了接口**来决定采用哪种动态代理的方式。



## 知识要点



- aop：面向切面编程

- aop底层实现：基于JDK的动态代理 和 基于Cglib的动态代理

- aop的重点概念：

  > Pointcut（切入点）：被增强的方法
  >
  > Advice（通知/ 增强）：封装增强业务逻辑的方法
  >
  > Aspect（切面）：切点+通知
  >
  > Weaving（织入）：将切点与通知结合的过程

- 开发明确事项：

  > 谁是**切点**（切点表达式配置）
  >
  > 谁是**通知**（切面类中的增强方法）
  >
  > 将切点和通知进行**织入配置**







## 基于XML的AOP开发





①导入 AOP 相关坐标

![image-20210127174518028](../picture/Spring%E7%AC%94%E8%AE%B0/image-20210127174518028.png)

```xml
<dependency>
    <groupId>org.aspectj</groupId>
    <artifactId>aspectjweaver</artifactId>
    <version>1.9.2</version>
</dependency>
```





②创建目标接口和目标类（内部有切点）

```java
public class Target  implements TargetInterface {
    @Override
    public void save() {
        System.out.println("save running");
    }
}
```



③创建切面类（内部有增强方法）

```java
public class MyAspect {

    public void before(){
        System.out.println("前置增强");
    }
}
```



④将目标类和切面类的对象创建权交给 spring  **bean标签**

⑤在 applicationContext.xml 中配置织入关系



导入aop命名空间

```xml
xmlns:aop="http://www.springframework.org/schema/aop"

http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop.xsd
```





```xml
<!--    配置目标对象-->
    <bean id="target" class="com.crhuang.aop.Target"/>

<!--    切面对象-->
    <bean id="myAspect" class="com.crhuang.aop.MyAspect"/>

<!--    告诉spring框架  哪些方法(切点)需要被进行哪些增强？-->
<!--    配置织入-->
    <aop:config >
<!--        告诉spring 这是个切面类-->
        <aop:aspect ref="myAspect">
<!--            切面：切点+通知-->
<!--            哪些方法是前置增强   切点表达式  方法全限定名-->
            <aop:before method="before" pointcut="execution(public void com.crhuang.aop.Target.save())"/>
        </aop:aspect>
    </aop:config>
```

- `<aop:aspect ref="myAspect">` 指定切面的**引用**：ref   才代表MyAspect是一个切面类

- `before` 增强类型

- `method="before"` 切面对象内的before()方法  **只是个方法名**

- `pointcut="execution(public void com.crhuang.aop.Target.save())"` 切点表达式









⑥测试代码

```java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration("classpath:applicationContext.xml")
public class AopTest {

    @Autowired
    //注意：这里要  使用接口来接，代理对象并不是Target类的子类或实现类
    private TargetInterface target;

    @Test
    //另外：Test要使用junit里的，别选成junit.jupiter了
    public void test1(){
        target.save();
    }
}
```



**BeanNotOfRequiredTypeException 说明使用的类型不对，不能使用Target类接这个代理对象**

![image-20210127182322464](../picture/Spring%E7%AC%94%E8%AE%B0/image-20210127182322464.png)



![image-20210127182242259](../picture/Spring%E7%AC%94%E8%AE%B0/image-20210127182242259.png)







### xml配置AOP详解



### 切点表达式的写法



表达式语法：

`execution([修饰符] 返回值类型 包名.类名.方法名(参数))`



- 访问修饰符可以省略

- 返回值类型、包名、类名、方法名可以使用星号*  代表任意

- 包名与类名之间一个点 . 代表当前包下的类，**两个点 .. 表示当前包及其子包下的类**

- 参数列表可以使用两个点 **.. 表示任意个数，任意类型的参数列表**



> method方法，并且没有返回值，没有参数
> `execution(public void com.itheima.aop.Target.method())`	
>
> **Target类的所有方法任意参数，就是所有的方法都会被增强，但要求是void**
>
> `execution(void com.itheima.aop.Target.*(..))`
>
> **返回值任意，包下的任意类的任意方法**
>
> `execution(* com.itheima.aop.*.*(..))`
>
> **aop包及子包下(如：aop.aop1.Target)的任意类的任意方法**
>
> `execution(* com.itheima.aop..*.*(..))`
>
> 无意义。。。
>
> `execution(* *..*.*(..))`



### 通知的类型



```xml
<aop:通知类型 method=“切面类中方法名” pointcut=“切点表达式"></aop:通知类型>
```

![image-20210127190033064](../picture/Spring%E7%AC%94%E8%AE%B0/image-20210127190033064.png)



```xml
<!--    告诉spring框架  哪些方法(切点)需要被进行哪些增强？-->
<!--    配置织入-->
    <aop:config >
<!--        告诉spring 这是个切面类-->
        <aop:aspect ref="myAspect">
<!--            切面：切点+通知-->
<!--            哪些方法是前置增强   切点表达式  方法全限定名-->
<!--            <aop:before method="before" pointcut="execution(* com.crhuang.aop.*.*(..))"/>-->
<!--            <aop:after-returning method="afterReturning" pointcut="execution(* com.crhuang.aop.*.*(..))"/>-->
            <aop:around method="around" pointcut="execution(* com.crhuang.aop.*.*(..))"/>
            <aop:after-throwing method="afterThrowing" pointcut="execution(* com.crhuang.aop.*.*(..))"/>
            <aop:after method="after" pointcut="execution(* com.crhuang.aop.*.*(..))"/>
        </aop:aspect>
    </aop:config>
```



```java
//正在执行的 连接点   ---  就是切点
public Object around(ProceedingJoinPoint proceedingJoinPoint) throws Throwable {
    System.out.println("环绕前");
    //切点方法
    Object proceed = proceedingJoinPoint.proceed();
    System.out.println("环绕后");
    return proceed;
}

public void afterThrowing(){
    System.out.println("异常抛出增强");
}

public void after(){
    System.out.println("最终增强.......");
}
```





### 切点表达式的抽取



抽取完好维护



当多个增强的切点表达式相同时，可以将切点表达式进行抽取，在增强中使用 pointcut-ref 属性代替 pointcut 属性来引用抽取后的切点表达式。



```xml
<aop:pointcut id="myPointcut" expression="execution(* com.crhuang.aop.*.*(..))"/>

<aop:around method="around" pointcut-ref="myPointcut"/>
```



## 基于注解的AOP开发



开发步骤：

①创建目标接口和目标类（内部有切点）

```java
@Component("target")
public class Target  implements TargetInterface {
    @Override
    public void save() {
        System.out.println("save running");
    }

    @Override
    public void save2() {
        System.out.println("save2 running");
    }
}
```





②创建切面类（内部有增强方法）

③将目标类和切面类的对象创建权交给 spring

④在切面类中使用注解配置织入关系

```java
@Component("myAspect")
@Aspect//标注当前这个MyAspect是一个切面类
public class MyAspect {

    //不用告诉是前置了，直接在这配置  需要指定切点表达式
    @Before(value = "execution(* com.crhuang.anno.*.*(..))")
    public void before(){
        System.out.println("前置增强");
    }
```





⑤在配置文件中开启组件扫描和 AOP 的自动代理



```xml
    <context:component-scan base-package="com.crhuang.anno"/>

<!--    aop自动代理-->
    <aop:aspectj-autoproxy/>
```

⑥测试

```java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration("classpath:applicationContext.xml")
public class AnnoTest {

    @Autowired
    //别忘了开启组件扫描
    private TargetInterface target;

    @Test
    public void test1(){
        target.save();
    }
}
```



### 注解配置aop详解



通知的配置语法：@通知注解(“切点表达式")



![image-20210127214004770](../picture/Spring%E7%AC%94%E8%AE%B0/image-20210127214004770.png)





```java
@Around(value = "execution(* com.crhuang.anno.*.*(..))")
//正在执行的 连接点   ---  就是切点
public Object around(ProceedingJoinPoint proceedingJoinPoint) throws Throwable {
    System.out.println("环绕前");
    //切点方法
    Object proceed = proceedingJoinPoint.proceed();
    System.out.println("环绕后");
    return proceed;
}
```



切点表达式的抽取：



```java
@Around("MyAspect.pointCut()")
//正在执行的 连接点   ---  就是切点
public Object around(ProceedingJoinPoint proceedingJoinPoint) throws Throwable {
    System.out.println("环绕前");
    //切点方法
    Object proceed = proceedingJoinPoint.proceed();
    System.out.println("环绕后");
    return proceed;
}

//定义一个切点表达式  注解需要一个宿主
@Pointcut("execution(* com.crhuang.anno.*.*(..))")
public void pointCut(){}
```



---



注解aop开发步骤:



①使用@Aspect**标注切面类**

②使用@通知注解**标注通知方法**

③在配置文件中配置aop自动代理`<aop:aspectj-autoproxy/>`







 









































































