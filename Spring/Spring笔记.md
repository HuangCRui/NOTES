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
| @ComponentScan  | 用于指定 Spring   在初始化容器时**要扫描的包**。   作用和在 Spring   的 xml 配置文件中的   <context:component-scan   base-package="com.itheima"/>一样 |
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











































