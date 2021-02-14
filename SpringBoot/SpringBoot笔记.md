 













![image-20210211025342522](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210211025342522.png)





![image-20210211025355505](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210211025355505.png)





# Spring与SpringBoot



## Spring的生态



![image-20210211025550760](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210211025550760.png)

- 微服务，功能太多，将模块拆分成微服务
- 响应式编程
- 分布式云开发
- web开发
- 无服务开发
- 事件驱动
- 批处理
- ....





![image-20210211031406195](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210211031406195.png)



----

快速将Spring生态圈整合起来--->**SpringBoot**





## Spring5  响应式编程



![image-20210211031724163](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210211031724163.png)



两套方案

响应式开发

---



java8：

**接口的默认实现（适配器Adapter）**  

重新设计源码架构。





## 为咩使用SpringBoot





> Spring Boot makes it easy to create stand-alone, production-grade Spring based Applications that you can "**just run**".
>
> 
>
> 能快速创建出生产级别的Spring应用







### SpringBoot优缺点



- Create stand-alone Spring applications

- - 创建独立**Spring应用**

- Embed Tomcat, Jetty or Undertow directly (no need to deploy WAR files)

- - 内嵌web**服务器** 		（war包打包到tomcat中发布）

- Provide opinionated 'starter' dependencies to simplify your build configuration

- - **自动starter依赖**，简化构建配置		（jar包及其版本，web：web-starter, **控制好了其中的版本**）

- Automatically configure Spring and 3rd party libraries whenever possible

- - 自动配置Spring以及第三方功能		（全部自动配置，只需要提供参数）

- Provide production-ready features such as metrics, health checks, and externalized configuration

- - 提供生产级别的监控、健康检查及外部化配置	

- Absolutely no code generation and no requirement for XML configuration

- - 无代码生成、无需编写XML  （**底层装配，自动注入**）



> SpringBoot是**整合Spring技术栈**的一站式框架
>
> SpringBoot是**简化**Spring技术栈的快速开发脚手架



----



- 迭代快
- 封装太深，内部原理复杂



## 微服务



微服务完整概念 中文版：https://mp.weixin.qq.com/s?__biz=MjM5MjEwNTEzOQ==&mid=401500724&idx=1&sn=4e42fa2ffcd5732ae044fe6a387a1cc3#rd

> In short, the **microservice architectural style** is an approach to developing a single application as a **suite of small services**, each **running in its own process** and communicating with **lightweight** mechanisms, often an **HTTP** resource API. These services are **built around business capabilities** and **independently deployable** by fully **automated deployment** machinery. There is a **bare minimum of centralized management** of these services, which may be **written in different programming languages** and use different data storage technologies.-- [James Lewis and Martin Fowler (2014)](https://martinfowler.com/articles/microservices.html)

- 微服务是一种**架构风格**
- 一个应用**拆分**为**一组小型服务**
- 每个服务运行在**自己的进程**内，也就是可独立部署和升级（**每一个小模块都可以独立部署在单独的服务器上，独立迭代升级**）
- 服务之间使用**轻量级**HTTP交互
- 服务围绕**业务功能**拆分
- 可以由**全自动部署**机制独立部署
- **去中心化**，服务**自治**。服务可以使用不同的语言、不同的存储技术









## 分布式



![image-20210211033525097](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210211033525097.png)





---

**分布式的困难**：



- 远程调用（互相调用）

- 服务发现

- 负载均衡

- 服务容错

- 配置管理（配置中心）

- 服务监控

- 链路追踪

- 日志管理

- 任务调度

- ......





---



**分布式的解决**

SpringBoot + SpringCloud



![image-20210211034008076](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210211034008076.png)





## 云与安生



原生应用如何上云。 Cloud Native



上云的困难：

- 服务自愈
- 弹性伸缩
- 服务隔离
- 自动化部署
- 灰度发布
- 流量治理
- ......



---



上云的解决：



![image-20210211034453095](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210211034453095.png)





## 官方文档





![image-20210211034929490](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210211034929490.png)





https://docs.spring.io/spring-boot/docs/current/reference/html/



查看版本新特性：

https://github.com/spring-projects/spring-boot/wiki/Spring-Boot-2.4-Release-Notes





# SpringBoot2入门





配置

![image-20210211043256941](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210211043256941.png)





maven配置：



```xml
<!-- 配置中央仓库的镜像（改用：阿里云中央仓库镜像）-->
<mirror>        
  <id>alimaven</id>
  <name>aliyun-maven</name>
  <mirrorOf>central</mirrorOf>
  <url>http://maven.aliyun.com/nexus/content/groups/public</url>
</mirror>


<profile>
	<id>jdk-11</id>
	<activation>
		<activeByDefault>true</activeByDefault>
		<jdk>11</jdk>
	</activation>
	<properties>
		<maven.compiler.source>11</maven.compiler.source>
		<maven.compiler.target>11</maven.compiler.target>
		<maven.compiler.compilerVersion>11</maven.compiler.compilerVersion>
	</properties>
</profile>
```



导入依赖

```xml
<!--    父工程-->
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.4.2</version>
    </parent>

    <dependencies>
<!--        导入依赖-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
    </dependencies>
```



```java
//这是一个springboot应用
//主程序类
@SpringBootApplication
public class MainApplication {

    public static void main(String[] args) {
        SpringApplication.run(MainApplication.class, args);
    }
}
```



`@RestController`  包括了 `@Controller`和 `@ResponseBody`

```java
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Controller
@ResponseBody
public @interface RestController {
    @AliasFor(
        annotation = Controller.class
    )
    String value() default "";
}
```



```java
@RestController
public class HelloController {
    @RequestMapping("/hello")
    public String handle01(){
        return "hello SpringBoot 2";
    }
}
```



![image-20210211162107886](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210211162107886.png)







配置文件 application.properties

```properties
server.port=8888
```



---

**配置文件能写什么：**

https://docs.spring.io/spring-boot/docs/current/reference/html/appendix-application-properties.html#common-application-properties





---

## 简化部署





默认是打包成jar包



![image-20210211162722090](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210211162722090.png)



导入打包所需依赖

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
        </plugin>
    </plugins>
</build>
```



打包

![image-20210211163628504](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210211163628504.png)



得到jar包

![image-20210211163840632](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210211163840632.png)



执行jar包

![image-20210211163943446](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210211163943446.png)





jar包  包含了所有的环境

![image-20210211164613249](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210211164613249.png)







# SpringBoot特点



## 依赖管理





```xml
<!--    父项目来做依赖管理-->
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.4.2</version>
    </parent>

它的父项目
  <parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-dependencies</artifactId>
    <version>2.4.2</version>
  </parent>
```



`spring-boot-dependencies` **这里面声明了近乎所有jar包的依赖**



![image-20210211164914466](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210211164914466.png)







**不用写明版本号**，父项目已经将版本都控制好了

```xml
    <dependencies>
<!--        导入依赖-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
        </dependency>
    </dependencies>
```



-----

- 可以修改版本号

查看 `spring-boot-dependencies`里面规定当前依赖的版本      

自定义修改版本：

![image-20210211165803364](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210211165803364.png)







---



**导入starter场景启动器**

spring-boo-starter-*,   *代表某种场景



![image-20210211170030104](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210211170030104.png)



只要引入starter，这个场景的所有常规需要的依赖

也可以自定义starter：见到的  `*-spring-boot-starter`： 第三方为我们提供的简化开发的场景启动器。

各种**场景**：https://docs.spring.io/spring-boot/docs/current/reference/html/using-spring-boot.html#using-boot-starter

![image-20210211170156511](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210211170156511.png)







所有场景启动器最底层的依赖：

**springboot自动配置核心依赖**

```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter</artifactId>
  <version>2.3.4.RELEASE</version>
  <scope>compile</scope>
</dependency>
```



依赖树：

![image-20210211170524370](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210211170524370.png)



---



无需关注版本号，自动版本仲裁



> 1、引入依赖默认都可以不写版本
> 2、引入**非版本仲裁的jar**，要写版本号。





## 自动配置



- 自动配好Tomcat

- - 引入**Tomcat依赖**。
  - **配置**Tomcat

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-tomcat</artifactId>
    <version>2.4.2</version>
    <scope>compile</scope>
</dependency>
```

- 自动配好SpringMVC

- - 引入SpringMVC全套组件
  - 自动配好SpringMVC**常用组件**（常用功能）

- 自动配好Web常见功能，如：字符编码问题

- - SpringBoot帮我们**配置好了所有web开发的常见场景**

  - ```java
    //这是一个springboot应用
    //主程序类
    @SpringBootApplication
    public class MainApplication {
        public static void main(String[] args) {
            //1、返回Ioc容器
            ConfigurableApplicationContext run = SpringAp plication.run(MainApplication.class, args);
    
            //2、查看容器里面的组件
            String[] definitionNames = run.getBeanDefinitionNames();
            for (String name : definitionNames){
                System.out.println(name);
            }
        }
    }
    ```

    **都在容器中：**

    `DispatcherServlet`

    ![image-20210211171503895](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210211171503895.png)

    乱码过滤器

    ![image-20210211171617361](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210211171617361.png)

    视图解析器 、文件上传解析器........都有

    ![image-20210211171706970](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210211171706970.png)

- 默认的**包结构**

- - **主程序**所在包及其下面的**所有子包**里面的**组件都会被默认扫描进来**
  - **无需以前的包扫描配置**
  - 想要改变扫描路径，`@SpringBootApplication(scanBasePackages="com.demo1")`

- - - 或者@ComponentScan 指定扫描路径

> @SpringBootApplication(scanBasePackages="com.demo1")
>
> 等同于
> @SpringBootConfiguration
> @EnableAutoConfiguration
> @ComponentScan("com.demo1")  包扫描



- **各种配置拥有默认值**
  - ![image-20210211172648608](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210211172648608.png)

- - 默认配置最终都是**映射到某个类上**，如：MultipartProperties---并且也在容器中
  - 配置文件的值最终会**绑定该类上**，这个类会**在容器中创建对象**

- **按需加载**所有自动配置项

- - 非常多的starter

  - **引入**了哪些场景这个场景的自动配置才会开启

  - SpringBoot所有的**自动配置功能**都在 `spring-boot-autoconfigure` 包里面

  - **引入对应场景后，自动配置文件生效，不引入则找不到这些类**

    ![image-20210211173059350](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210211173059350.png)

    ![image-20210211173139239](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210211173139239.png)

    

    



# 容器功能





##  组件添加





### @Configuration



- **Full模式与Lite模式**
  - 最佳实战
    - 配置 类组件之间无依赖关系用Lite模式**加速容器启动过程**，减少判断
    - 配置类组件之间有依赖关系，方法会被调用得到之前**单实例组件**，用Full模式

```java
/*
配置类里面使用@Bean标注在方法上给容器注册组件，默认也是单实例的
配置类本身也是一个组件
proxyBeanMethods：代理bean的方法
Full(proxyBeanMethods = true) 每次都要检查是容器不是有这个组件
Lite(proxyBeanMethods = false)每次调用都会产生一个新的对象  跳过检查组件是否在容器中
组件依赖必须使用Full模式（默认）
如果不依赖，就使用false
 */
@Configuration(proxyBeanMethods=false)//告诉springboot这是一个配置类  ==  配置文件
public class MyConfig {

    //外部无论对配置类中的这个组件注册 方法 调用多少次，获取的都是之前注册到容器中的单实例对象
    @Bean //给容器中添加组件。以方法名作为组件的id，返回类型是组件类型。方法返回的对象就是组件在容器中的实例
    public User user01(){
        User user = new User("zhangsan", 18);
        //user组件依赖了pet组件
        user.setPet(tomcatPet());//如果是proxyBeanMethods=true，就可以这么调用  用的pet对象就和容器中的一模一样，依赖了pet组件
        return user;
    }

    @Bean("tom")
    public Pet tomcatPet(){
        return new Pet("tom");
    }
}
```





```java
//这是一个springboot应用
//主程序类
@SpringBootApplication
public class MainApplication {
    public static void main(String[] args) {
        //1、返回Ioc容器
        ConfigurableApplicationContext run = SpringApplication.run(MainApplication.class, args);

        //2、查看容器里面的组件
        String[] definitionNames = run.getBeanDefinitionNames();
        for (String name : definitionNames){
            System.out.println(name);
        }

        //3、从容器中获取组件

        //com.demo1.config.MyConfig$$EnhancerBySpringCGLIB$$bc5c9479@654c7d2d  代理对象
        //如果@Configuration(proxyBeanMethods=false)  com.demo1.config.MyConfig@21bd20ee
        MyConfig bean = run.getBean(MyConfig.class);
        System.out.println(bean);
        //如果@Configuration(proxyBeanMethods=true) 代理对象调用方法 springboot总会检查这个组件是否在容器中有
        //保持组件单实例
        
        User user = bean.user01();
        User user1 = bean.user01();
        System.out.println(user == user1); 

        User user01 = run.getBean("user01", User.class);
        Pet tom = run.getBean("tom", Pet.class);
        System.out.println(user01.getPet() == tom);
    }
}
```





![image-20210211220654607](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210211220654607.png)







> @Bean、@Component、@Controller、@Service、@Repository



### @ComponentScan、@Import



```java
/*
import:给容器中自动创建出这两个类型的组件,默认组件的名字是全类名
 */
@Import({User.class, DBHelper.class})
```



### @Conditional



条件装配：满足COnditional指定的条件，则进行组件注入



![image-20210211231539972](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210211231539972.png)





```java
@ConditionalOnBean(name = "tom")//配置在类上或方法上，只要条件中的组件在容器中，才进行下面的操作
```







## 原生配置文件引入



### @ImportResource



在xml配置文件的基础上，在配置类上添加注解

`@ImportResource("classpath:beans.xml")`

来导入配置文件







## 配置绑定



如何使用Java**读取到properties文件中的内容**，并且把它**封装到JavaBean中**，以供随时使用



```java
public class getProperties {
     public static void main(String[] args) throws FileNotFoundException, IOException {
         Properties pps = new Properties();
         pps.load(new FileInputStream("a.properties"));
         Enumeration enum1 = pps.propertyNames();//得到配置属性的名字
         while(enum1.hasMoreElements()) {
             String strKey = (String) enum1.nextElement();
             String strValue = pps.getProperty(strKey);
             System.out.println(strKey + "=" + strValue);
             //封装到JavaBean。
         }
     }
 }
```



### @ConfigurationProperties



```properties
mycar.brand=Porsche
mycar.price=1000000
```



```java
//容器中的组件才能拥有springboot提供的各种功能
@Component
@ConfigurationProperties(prefix = "mycar") //和配置文件中哪个前缀匹配
public class Car {
    private String brand;
    private Integer price;
}
```









### @EnableConfigurationProperties + @ConfigurationProperties





```java
//@Component
@ConfigurationProperties(prefix = "mycar") //和配置文件中哪个前缀匹配
public class Car {}
```



```java
@EnableConfigurationProperties(Car.class)//开启属性配置功能
//1. 开启Car属性配置绑定功能
//2. 把Car这个组件自动注册到容器中
//可以就不用写@Component了（引用的第三方包无法配置@Controller注解）
public class MyConfig {
```



**两种办法，要么加载容器中，要么使用开启功能的方式**



> properties中自定义的属性，想绑给哪个javaBean都可以







# 自动配置原理入门



## 引导加载自动配置类



```java
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan(excludeFilters = { @Filter(type = FilterType.CUSTOM, classes = TypeExcludeFilter.class),
      @Filter(type = FilterType.CUSTOM, classes = AutoConfigurationExcludeFilter.class) })
public @interface SpringBootApplication {}
```



- `@SpringBootConfiguration`，代表当前是一个配置类
- `@ComponentScan`，指定扫描哪些spring注解





### @EnableAutoConfiguration

```java
@AutoConfigurationPackage
@Import(AutoConfigurationImportSelector.class)
public @interface EnableAutoConfiguration {}
```



---



`@AutoConfigurationPackage`，

> 自动配置包？指定了默认的包规则
>
> **即 `MainApplication` 所在的这个包**



```java
//给容器中导入一个组件
@Import(AutoConfigurationPackages.Registrar.class) 
public @interface AutoConfigurationPackage {}

//利用Registrar给容器中导入一系列组件
//将指定的一个包下的所有组件导入进来 ——> MainApplication 所在包下(com.demo1)。
```



![image-20210212045836216](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210212045836216.png)



![image-20210212045236399](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210212045236399.png)





得到的包名    **此时注解标注在主类上，主类在com.demo1包中**

![image-20210212045401125](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210212045401125.png)



**Registrar将main程序所在包下的所有组件，批量注册**

![image-20210212045534613](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210212045534613.png)







---



`@Import(AutoConfigurationImportSelector.class)`



`org.springframework.boot.autoconfigure.AutoConfigurationImportSelector#selectImports`

![image-20210212050549253](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210212050549253.png)



利用 `getAutoConfigurationEntry`给容器中批量导入一些组件



这里面的组件都默认导入容器中

![image-20210212050833413](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210212050833413.png)





然后：调用 `List<String> configurations = getCandidateConfigurations(annotationMetadata, attributes);`**获取到所有需要导入到容器中的配置类**



利用工厂：

```java
List<String> configurations = SpringFactoriesLoader.loadFactoryNames(getSpringFactoriesLoaderFactoryClass(),
      getBeanClassLoader());
```

加载：

`Map<String, List<String>> loadSpringFactories(ClassLoader classLoader)` **得到所有组件**



从 `"META-INF/spring.factories"` 位置来**加载一个文件**

默认扫描当前系统里面所有 `"META-INF/spring.factories"`位置的文件



![image-20210212051628965](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210212051628965.png)



![image-20210212051802361](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210212051802361.png)



`spring-boot-autoconfigure-2.4.2.jar`包里面也有META-INF/spring.factories文件



文件里面写死了springboot一启动就要给容器中加载的所有配置类

![image-20210212052225767](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210212052225767.png)





## 按需开启自动配置项



虽然130个场景的所有自动配置启动的时候默认全部加载，

最终按需配置



![image-20210212052539240](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210212052539240.png)

![image-20210212052503210](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210212052503210.png)

只有导入包/依赖，才会生效



---



![image-20210212052623528](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210212052623528.png)

![image-20210212052642472](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210212052642472.png)





**都不能生效~~**



得益于springboot按照条件装配规则@Controller，按需配置







## 定制化修改自动配置



![image-20210212152904729](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210212152904729.png)



---





![image-20210212153347703](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210212153347703.png)



![image-20210212153401860](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210212153401860.png)







![image-20210212153712628](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210212153712628.png)





给容器中阿计入了文件上传解析器：当容器中有这个**类型**的组件，但是容器中没有这个**名字**的组件

给@Bean标注的方法传入了对象参数，这个参数的值就会从 **容器中找(这个类型)**

**规范化，防止有些用户配置的文件上传解析器不符合规范**

![image-20210212153900891](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210212153900891.png)





![image-20210212183107647](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210212183107647.png)



SpringBoot默认会在底层配好所有的组件。但是如果用户自己配置了，以用户的优先

—>`@ConditionalOnMissingBean`



---



**总结：**

- springboot先加载所有的自动配置类   `xxxxAutoConfiguration`
- 每个自动配置类**按照条件进行生效**,   ***默认都会   绑定配置文件 `xxxxProperties.class` 中  指定的值***  并且`xxxxProperties.class` 和配置文件进行了绑定
- 生效的**配置类**  就会给容器中装配很多**组件**
- 只要容器中有这些组件，相当于这些**功能就有了**
- 只要用户有自己配置的，就以用户的优先
- 定制化配置
  - 用户直接自己@Bean替换底层组件
  - 用户去看这个组件是获取的配置文件什么值就去修改 `server.servlet.encoding.charset=utf-8`



`xxxxAutoConfiguration` ---->  组件    ---->   `xxxxProperties.class` 里面拿值   ---->   `application.properties`



```java
//绑定了配置文件中属性前缀
@ConfigurationProperties(prefix = "server", ignoreUnknownFields = true)
public class ServerProperties {}
```

![image-20210212184317251](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210212184317251.png)



![image-20210212184455258](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210212184455258.png)







## 最佳实践



- 引入场景依赖

  - https://docs.spring.io/spring-boot/docs/current/reference/html/using-spring-boot.html#using-boot-starter

- 查看自动配置了哪些

  - 根据导入的场景starter的自动配置来分析，一般都生效了

  - 配置文件application.properties  中开启 `debug=true`  **自动配置报告**   negative不生效   positive生效的配置

    - ![image-20210212191744981](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210212191744981.png)

    - ![image-20210212191823270](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210212191823270.png)

      ![image-20210212191934241](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210212191934241.png)



- 是否需要修改
  - 参照文档修改配置项
    - https://docs.spring.io/spring-boot/docs/current/reference/html/appendix-application-properties.html#common-application-properties
    - 分析  xxxxProperties都绑定了配合文件的哪些属性
      - `spring.banner.location=classpath:noBug.txt` 修改banner
  - 自定义加入或者替换组件
    - @Bean，  @Componnet ......
  - **自定义器xxxCustomizer** 











# 配置文件yaml





properties

---







YAML 是 "YAML Ain't Markup Language"（YAML 不是一种标记语言）的递归缩写。在开发的这种语言时，YAML 的意思其实是："Yet Another Markup Language"（仍是一种标记语言）。 



非常适合用来做**以数据为中心**的配置文件





## 基本语法



`key: value`；kv之间有空格

大小写敏感

使用**缩进**表示层级关系

缩进不允许使用tab，**只允许空格**

缩进的空格数不重要，只要相同层级的元素**左对齐**即可

'#'表示注释

字符串无需加引号，如果要加，''与""表示字符串内容 会被 转义/不转义





## 数据类型



- 字面量：单个的、不可再分的值。date、boolean、string、number、null

```
k: v
```

- 对象：键值对的集合。map、hash、set、object 

```
行内写法：  k: {k1:v1,k2:v2,k3:v3}
#或
k: 
  k1: v1
  k2: v2
  k3: v3
```



- 数组：一组按次序排列的值。array、list、queue

```
行内写法：  k: [v1,v2,v3]
#或者
k:
 - v1
 - v2
 - v3
```





## 示例



```java
@Data
@ToString
@Component
@ConfigurationProperties(prefix = "person")
public class Person {
    private String userName;
    private Boolean boss;
    private Date birth;
    private Integer age;
    private Pet pet;
    private String[] interests;
    private List<String> animal;
    private Map<String, Object> score;
    private Set<Double> salarys;
    private Map<String, List<Pet>> allPets;
}
```



```yaml
person:
  userName: zhangsan
  boss: true
  birth: 2021/2/12
  age: 21
  #insterests: [羽毛球,跑步]
  insterests:
    - 羽毛球
    - coding
  animals: [猫猫, 狗狗]
#  score:
#    english: 80
#    math: 90
  score: {english: 80, math: 90}
  salarys:
    - 9999.99
    - 9999.95
  pet:
    name: 布偶
    weight: 250.5
  allPets:
    sick:
      - {name: 大橘, weight: 99.99}
      - name: 布偶
        weight: 88.88
      - name: 美短
        weight: 50.00
    health:
      - {name: 萨摩耶, weight: 250.00}
      - {name: 二哈, weight: 250.10}
```

![image-20210213011139459](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210213011139459.png)



根据层级关系对应模块



> properties优先于yaml



**写字符串的时候无需使用引号**

双引号`“zhangsan \n lisi”`，会输出 一个 **换行符**

单引号：`'zhangsan \n lisi'`，输出`zhangsan \n lisi`字符串



> 双引号不会转义，单引号会转义
>
> **\n**的**本身意思就是换行** ，双引号没改变它本身意思，就是换行
>
> 单引号会改变它的本身意思，将\  转义为 `\\`



----



怎么没有提示呢？？？？？？？？



![image-20210213021302921](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210213021302921.png)

原来是你。。



```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-configuration-processor</artifactId>
    <optional>true</optional>
</dependency>
```

加上这个依赖并重启应用，yaml中就可以显示提示了





















# Web开发



![image-20210213150730459](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210213150730459.png)





## springmvc自动配置概览



**选自官方文档**

Spring Boot provides auto-configuration for Spring MVC that **works well with most applications.(大多场景我们都无需自定义配置)**



The auto-configuration adds the following features on top of Spring’s defaults:

- Inclusion of `ContentNegotiatingViewResolver` and `BeanNameViewResolver` beans.

- - 内容协商视图解析器和BeanName视图解析器

- Support for serving static resources, including support for WebJars (covered [later in this document](https://docs.spring.io/spring-boot/docs/current/reference/html/spring-boot-features.html#boot-features-spring-mvc-static-content))).

- - 静态资源（包括webjars）

- Automatic registration of `Converter`, `GenericConverter`, and `Formatter` beans.

- - 自动注册 `Converter，GenericConverter，Formatter `

- Support for `HttpMessageConverters` (covered [later in this document](https://docs.spring.io/spring-boot/docs/current/reference/html/spring-boot-features.html#boot-features-spring-mvc-message-converters)).

- - 支持 `HttpMessageConverters` （后来我们配合内容协商理解原理）

- Automatic registration of `MessageCodesResolver` (covered [later in this document](https://docs.spring.io/spring-boot/docs/current/reference/html/spring-boot-features.html#boot-features-spring-message-codes)).

- - 自动注册 `MessageCodesResolver` （国际化用）

- Static `index.html` support.

- - 静态index.html 页支持

- Custom `Favicon` support (covered [later in this document](https://docs.spring.io/spring-boot/docs/current/reference/html/spring-boot-features.html#boot-features-spring-mvc-favicon)).

- - 自定义 `Favicon`  网站小图标

- Automatic use of a `ConfigurableWebBindingInitializer` bean (covered [later in this document](https://docs.spring.io/spring-boot/docs/current/reference/html/spring-boot-features.html#boot-features-spring-mvc-web-binding-initializer)).

- - 自动使用 `ConfigurableWebBindingInitializer` ，（DataBinder负责将请求数据绑定到JavaBean上）

> If you want to keep those Spring Boot MVC customizations and make more [MVC customizations](https://docs.spring.io/spring/docs/5.2.9.RELEASE/spring-framework-reference/web.html#mvc) (interceptors, formatters, view controllers, and other features), you can add your own `@Configuration` class of type `WebMvcConfigurer` but **without** `@EnableWebMvc`.
>
> **不用@EnableWebMvc注解。使用** **`@Configuration`** **+** **`WebMvcConfigurer（组件名称）`** **自定义规则**



> If you want to provide custom instances of `RequestMappingHandlerMapping`, `RequestMappingHandlerAdapter`, or `ExceptionHandlerExceptionResolver`, and still keep the Spring Boot MVC customizations, you can declare a bean of type `WebMvcRegistrations` and use it to provide custom instances of those components.
>
> **声明** **`WebMvcRegistrations`** **改变默认底层组件**



> If you want to take complete control of Spring MVC, you can add your own `@Configuration` annotated with `@EnableWebMvc`, or alternatively add your own `@Configuration`-annotated `DelegatingWebMvcConfiguration` as described in the Javadoc of `@EnableWebMvc`.
>
> **使用** **`@EnableWebMvc+@Configuration+DelegatingWebMvcConfiguration 全面接管SpringMVC`**



## 简单功能分析





### 静态资源访问



#### 静态资源目录



只要静态资源放在**类路径下**： called `/static`  or `/public` or `/resources` or `/META-INF/resources`

访问 ： 当前项目根路径/ + **直接加静态资源名** ，就可以访问到静态资源



```java
@RestController
public class HelloController {

    @RequestMapping("/haocun1.png")
    public String hello(){
        return "aaaa";  // aaaa
    }
}
```



原理： 静态映射`/**`。

![image-20210213161744812](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210213161744812.png)

请求进来，**先去找Controller看能不能处理,是否有mapping**。不能处理的所有请求又都交给**静态资源处理器**。静态资源也找不到则响应404页面



![image-20210213161809474](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210213161809474.png)

改变默认的静态资源路径

![image-20210213164853543](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210213164853543.png)

```yaml
spring:
  mvc:
    static-path-pattern: /res/**

  resources:
    static-locations: classpath:/haha/
```





#### 静态资源访问前缀



默认无前缀



```yaml
spring:
  mvc:
    static-path-pattern: /res/**
```



当前项目根路径 + `static-path-pattern` + 静态资源名 = 静态资源文件夹下找



`http://localhost:8080/res/haocun1.png`





#### webjar



自动映射 /[webjars](http://localhost:8080/webjars/jquery/3.5.1/jquery.js)/**

https://www.webjars.org/

```xml
        <dependency>
            <groupId>org.webjars</groupId>
            <artifactId>jquery</artifactId>
            <version>3.5.1</version>
        </dependency>
```

访问地址：[http://localhost:8080/webjars/jquery/3.5.1/jquery.js](http://localhost:8080/webjars/jquery/3.5.1/jquery.js)  后面地址要按照依赖里面的包路径



![image-20210213170053622](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210213170053622.png)





### 欢迎页支持



- 静态资源路径下  index.html

- - 可以配置**静态资源路径**
  - 但是**不可以配置静态资源的访问前缀**。否则导致 index.html**不能被默认访问**

```yaml
#  mvc:
#    static-path-pattern: /res/**   会导致welcome page失效
  web:
    resources:
      static-locations: classpath:/haha
```

- controller能处理`/index`





### 自定义Favicon

将图标名称设置为：favicon.ico

不显示和缓存有关

![image-20210213174203633](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210213174203633.png)

```
spring:
#  mvc:
#    static-path-pattern: /res/**   这个会导致 Favicon 功能失效。。。。。。
```









### 静态资源配置原理



- SpringBoot启动默认加载  **xxxAutoConfiguration 类**（**自动配置类**）

- **SpringMVC功能**的自动配置类 `WebMvcAutoConfiguration`，生效

```java
@Configuration(proxyBeanMethods = false)
@ConditionalOnWebApplication(type = Type.SERVLET)
@ConditionalOnClass({ Servlet.class, DispatcherServlet.class, WebMvcConfigurer.class })
//全面接管springmvc
@ConditionalOnMissingBean(WebMvcConfigurationSupport.class)
@AutoConfigureOrder(Ordered.HIGHEST_PRECEDENCE + 10)
@AutoConfigureAfter({ DispatcherServletAutoConfiguration.class, TaskExecutionAutoConfiguration.class,
      ValidationAutoConfiguration.class })
public class WebMvcAutoConfiguration {}
```

- 给容器中配了什么

  ```java
  @Configuration(proxyBeanMethods = false)
  @Import(EnableWebMvcConfiguration.class)//将EnableWebMvcConfiguration类添加到容器中
  @EnableConfigurationProperties({ WebMvcProperties.class,
        org.springframework.boot.autoconfigure.web.ResourceProperties.class, WebProperties.class })
  @Order(0)
  public static class WebMvcAutoConfigurationAdapter implements WebMvcConfigurer {}
  ```

- 配置文件的相关属性和 ？xxx 进行了绑定

  - ```java
    @ConfigurationProperties(prefix = "spring.mvc")
    public class WebMvcProperties {}
    ```

  - ```java
    @Deprecated  //这个属性类已经废弃  改用WebProperties
    @ConfigurationProperties(prefix = "spring.resources", ignoreUnknownFields = false)
    public class ResourceProperties extends Resources {}
    ```

    ![image-20210213181718013](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210213181718013.png)

  - ```java
    @ConfigurationProperties("spring.web")//原先resources中的属性都改为web.resources
    public class WebProperties {}
    ```

![image-20210213181700778](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210213181700778.png)

```java
/*
webProperties   获取和spring.web绑定的所有的值的对象
mvcProperties	 获取和spring.mvc绑定的所有的值的对象
beanFactory	  	 Spring的beanFactory
HttpMessageConverters   找到所有的HttpMessageConverters
ResourceHandlerRegistrationCustomizer		============找到资源处理器的自定义器=============
dispatcherServletPath
ServletRegistrationBean		给应用注册servlet、filter....
*/
public WebMvcAutoConfigurationAdapter(WebProperties webProperties, WebMvcProperties mvcProperties,
      ListableBeanFactory beanFactory, ObjectProvider<HttpMessageConverters> messageConvertersProvider,
      ObjectProvider<ResourceHandlerRegistrationCustomizer> resourceHandlerRegistrationCustomizerProvider,
      ObjectProvider<DispatcherServletPath> dispatcherServletPath,
      ObjectProvider<ServletRegistrationBean<?>> servletRegistrations) {
   this.mvcProperties = mvcProperties;
   this.beanFactory = beanFactory;
   this.messageConvertersProvider = messageConvertersProvider;
   this.resourceHandlerRegistrationCustomizer = resourceHandlerRegistrationCustomizerProvider.getIfAvailable();
   this.dispatcherServletPath = dispatcherServletPath;
   this.servletRegistrations = servletRegistrations;
   this.mvcProperties.checkConfiguration();
}
```



----



**资源处理的默认规则**



```java
//ResourceProperties： WebProperties
private final Resources resourceProperties;


@Override
protected void addResourceHandlers(ResourceHandlerRegistry registry) {
    super.addResourceHandlers(registry);
    if (!this.resourceProperties.isAddMappings()) {
        logger.debug("Default resource handling disabled");
        return;
    }
    ServletContext servletContext = getServletContext();
    //webjars的访问规则
    addResourceHandler(registry, "/webjars/**", "classpath:/META-INF/resources/webjars/");
    
    //静态资源路径前缀的配置规则      				/**	 ↓
    addResourceHandler(registry, this.mvcProperties.getStaticPathPattern(), (registration) -> {
        registration.addResourceLocations(this.resourceProperties.getStaticLocations());
        if (servletContext != null) {
            registration.addResourceLocations(new ServletContextResource(servletContext, SERVLET_LOCATION));
        }
    });
}

private void addResourceHandler(ResourceHandlerRegistry registry, String pattern, String... locations) {
    //都找location这个路径，即：classpath:/META-INF/resources/webjars/
    addResourceHandler(registry, pattern, (registration) -> registration.addResourceLocations(locations));
}

private void addResourceHandler(ResourceHandlerRegistry registry, String pattern,
                                Consumer<ResourceHandlerRegistration> customizer) {
    if (registry.hasMappingForPattern(pattern)) {
        return;
    }
    ResourceHandlerRegistration registration = registry.addResourceHandler(pattern);
    customizer.accept(registration);
    //并且这个资源还会缓存一段时间（配置文件中）
    registration.setCachePeriod(getSeconds(this.resourceProperties.getCache().getPeriod()));
    registration.setCacheControl(this.resourceProperties.getCache().getCachecontrol().toHttpCacheControl());
    customizeResourceHandlerRegistration(registration);
}
```



![image-20210213190050644](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210213190050644.png)

![image-20210213190427875](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210213190427875.png)



默认值

```java
public static class Resources {

   private static final String[] CLASSPATH_RESOURCE_LOCATIONS = { "classpath:/META-INF/resources/",
         "classpath:/resources/", "classpath:/static/", "classpath:/public/" };

   /**
    * Locations of static resources. Defaults to classpath:[/META-INF/resources/,
    * /resources/, /static/, /public/].
    */
   private String[] staticLocations = CLASSPATH_RESOURCE_LOCATIONS;
```





```yaml
spring:
  web:
   resources:
     add-mappings: false  所有静态资源都无法访问
```





---





欢迎页的处理规则

**HandlerMapper: 处理器映射。保存了每一个Handler能处理哪些请求**



```java
@Bean
public WelcomePageHandlerMapping welcomePageHandlerMapping(ApplicationContext applicationContext,
      FormattingConversionService mvcConversionService, ResourceUrlProvider mvcResourceUrlProvider) {
   WelcomePageHandlerMapping welcomePageHandlerMapping = new WelcomePageHandlerMapping(
         new TemplateAvailabilityProviders(applicationContext), applicationContext, getWelcomePage(),
       		//得到默认/配置的静态资源前缀 
         this.mvcProperties.getStaticPathPattern());
   welcomePageHandlerMapping.setInterceptors(getInterceptors(mvcConversionService, mvcResourceUrlProvider));
   welcomePageHandlerMapping.setCorsConfigurations(getCorsConfigurations());
   return welcomePageHandlerMapping;
}


	WelcomePageHandlerMapping(TemplateAvailabilityProviders templateAvailabilityProviders,
			ApplicationContext applicationContext, Resource welcomePage, String staticPathPattern) {
        //若配置了自定义访问路径前缀，就进不到if中，即找不到欢迎页
        //===========  解释了：前面配置了staticPathPattern就找不到欢迎页，  无法跳转到欢迎页
        //要使用欢迎页功能，就必须使用 /**
		if (welcomePage != null && "/**".equals(staticPathPattern)) {
			logger.info("Adding welcome page: " + welcomePage);
			setRootViewName("forward:index.html");
		}
        //调用controller   /index  查看谁能处理该请求
		else if (welcomeTemplateExists(templateAvailabilityProviders, applicationContext)) {
			logger.info("Adding welcome page template: index");
			setRootViewName("index");
		}
	}
```





![image-20210213215121881](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210213215121881.png)





![image-20210213215333064](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210213215333064.png)



修改staticPathPattern：则无法进入

![image-20210213215644226](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210213215644226.png)





----



还有个favicon：/favicon.ico，当前项目下的这个文件名，一旦配置了静态资源访问前缀，自然就找不到了







## 请求参数处理



### 请求映射



#### Rest使用与原理





- xxxMapping
- Rest风格支持（用HTTP请求方式动词来表示对资源的操作）
  - 以前： /getUser 获取用户    /deleteUser 删除用户	  /editUser  修改用户   /saveUser  保存用户
  - 现在：**几个方法的请求路径都是/user**   GET-获取用户 	DELETE-删除用户   PUT-修改用户     POST-保存用户
  - 核心FIlter：HiddenHttpMethodFilter
    - 用法：表单method=post，隐藏域 _method=put





```html
<form action="/user" method="get">
    <input type="submit" value="REST-GET">
</form>
<form action="/user" method="post">
    <input type="submit" value="REST-POST">
</form>
<!--默认当成get方式GET-张三-->
<form action="/user" method="delete">
    <input type="submit" value="REST-DELETE">
</form>
<form action="/user" method="put">
    <input type="submit" value="REST-PUT">
</form>
```











```java
//WebMvcAutoConfiguration
@Bean
//两个条件
@ConditionalOnMissingBean(HiddenHttpMethodFilter.class)
//配置文件中的属性，默认是不开启，需要修改配置文件来开启
@ConditionalOnProperty(prefix = "spring.mvc.hiddenmethod.filter", name = "enabled", matchIfMissing = false)
public OrderedHiddenHttpMethodFilter hiddenHttpMethodFilter() {
   return new OrderedHiddenHttpMethodFilter();
}


public class HiddenHttpMethodFilter extends OncePerRequestFilter {
    private static final List<String> ALLOWED_METHODS;
    public static final String DEFAULT_METHOD_PARAM = "_method";
    private String methodParam = "_method";
  
```

**_method属性作为真正的请求方式**



只有表单是POST方式，才会去寻找`methodParam`参数对应的值

```java
protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response, FilterChain filterChain) throws ServletException, IOException {
    HttpServletRequest requestToUse = request;
    if ("POST".equals(request.getMethod()) && request.getAttribute("javax.servlet.error.exception") == null) {
        String paramValue = request.getParameter(this.methodParam);//methodParam = "_method"
        if (StringUtils.hasLength(paramValue)) {
            String method = paramValue.toUpperCase(Locale.ENGLISH);
            if (ALLOWED_METHODS.contains(method)) {
                requestToUse = new HiddenHttpMethodFilter.HttpMethodRequestWrapper(request, method);
            }
        }
    }

    filterChain.doFilter((ServletRequest)requestToUse, response);
}
```



故修改表单：

```html
<form action="/user" method="get">
    <input type="submit" value="REST-GET">
</form>
<form action="/user" method="post">
    <input type="submit" value="REST-POST">
</form>
<!--默认当成get方式GET-张三-->
<form action="/user" method="post">
    <input name="_method" type="hidden" value="DELETE">
    <input type="submit" value="REST-DELETE">
</form>
<form action="/user" method="post">
    <input name="_method" type="hidden" value="PUT">
    <input type="submit" value="REST-PUT">
</form>
```



```yaml
spring:
  banner:
    location: classpath:noBug.txt
  web:
    resources:
      add-mappings: true
      cache:
        period: 1100
      static-locations: classpath:/hh
  mvc:
    hiddenmethod:
      filter:
        enabled: true
```



----





Rest原理：（表单提交要使用REST的时候）

- 表单提交会带上_method=PUT

- 请求过来会被HiddenHttpMethodFilter拦截

  - 获取到_method的值：`String paramValue = request.getParameter(this.methodParam);`

    ![image-20210214161616486](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210214161616486.png)

  - 转成大写 `String method = paramValue.toUpperCase(Locale.ENGLISH);

  - 允许的请求方式：

    ```java
    static {
        ALLOWED_METHODS = Collections.unmodifiableList(Arrays.asList(HttpMethod.PUT.name(), HttpMethod.DELETE.name(), HttpMethod.PATCH.name()));
    }
    ```

    - 兼容 PUT DELETE PATCH请求

  - 原生request（post），包装模式requestWrapper重写了getMethod方法，返回的是 **传入的值(_method的值)**

    ![image-20210214182037119](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210214182037119.png)

  - 过滤器链方形的时候用wrapper，以后的方法调用getMethod是调用Wrapper的method

    ```java
    @RequestMapping(value = "/user",method = RequestMethod.PUT)
    public String putUser(HttpServletRequest request){
        return "PUT-张三" + request.getMethod();//PUT
    }
    ```



Rest使用客户端工具

- 使用POSTMAN可以直接发请求，从http层面：发过来的就是Delete请求，不需要进行转换，直接跳过这一步。**无需Filter**

  ![image-20210214182519019](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210214182519019.png)



-----



使用：

```java
@PutMapping("/user")
@DeleteMapping("/user")
@GetMapping("/user")
@PostMapping("/user")
```



```java
@RequestMapping(
    method = {RequestMethod.GET}
)
public @interface GetMapping {
```





----



**扩展：如何将_method变成其他名字**



```java
@Bean
//自己放一个HiddenHttpMethodFilter
@ConditionalOnMissingBean(HiddenHttpMethodFilter.class)
@ConditionalOnProperty(prefix = "spring.mvc.hiddenmethod.filter", name = "enabled", matchIfMissing = false)
public OrderedHiddenHttpMethodFilter hiddenHttpMethodFilter() {
   return new OrderedHiddenHttpMethodFilter();
}
```



```java
@Configuration(proxyBeanMethods = false)
public class WebConfig {

    @Bean
    public HiddenHttpMethodFilter hiddenHttpMethodFilter(){
        HiddenHttpMethodFilter hiddenHttpMethodFilter = new HiddenHttpMethodFilter();
        //private String methodParam = "_method";可以改变
        hiddenHttpMethodFilter.setMethodParam("_m");
        return hiddenHttpMethodFilter;
    }
}
```

![image-20210214183812109](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210214183812109.png)



修改为： 

```html
<form action="/user" method="post">
    <input name="_m" type="hidden" value="PUT">
    <input type="submit" value="REST-PUT">
</form>
```

成功！







#### 请求映射原理



![image-20210214203500127](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210214203500127.png)



所有对springmvc功能分析，都从`DispatcherServlet` -> `doDispatch()`方法开始





![image-20210214204157035](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210214204157035.png)



**如何从 /user请求找到调用deleteUser()方法来处理的？**



![image-20210214204410802](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210214204410802.png)





```java
//找到当前请求使用哪个Handler（Controller中的方法）来处理
mappedHandler = this.getHandler(processedRequest);
if (mappedHandler == null) {
    this.noHandlerFound(processedRequest, response);
    return;
}
```



-----



**HandlerMapping：处理器映射**





![image-20210214204746597](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210214204746597.png)





RequestMappingHandlerMapping：保存了所有@RequestMapping和handler的映射规则。

扫描所有的controller，保存到handlerMapping中



![image-20210214211132503](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210214211132503.png)





![image-20210214211349711](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210214211349711.png)





![image-20210214213213836](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210214213213836.png)



要求：同样的一个请求方式，只能有一个handler方法处理



所有的请求映射都在HandlerMapping中。

- Springboot自动配置 欢迎页的HandlerMapping。访问/能访问到index.html。**此时requestmapping无法找到**

![image-20210214215434439](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210214215434439.png)

此时没有匹配的路径，找到的handler是null

继续for循环，来到第二个handlermapping

![image-20210214215533662](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210214215533662.png)



![image-20210214215557570](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210214215557570.png)

![image-20210214215642771](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210214215642771.png)





----



同理：若是访问/delete

![image-20210214215847337](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210214215847337.png)

也可以得到对应的handler







- 请求进来，挨个尝试所有的HandlerMapping看是否有请求信息。
  - 如果有就找到这个请求对应的handler
  - 如果没有，就是下一个handlerMapping
- springboot自动配置了默认的 `RequestMappingHandlerMapping`（**解析我们当前所有requestmapping方法**），`WelcomePageHandlerMapping`
- 我们需要一些**自定义**的映射处理，也可以自己给容器中放HandlerMapping。`自定义HandlerMapping`



 













### 普通参数与基本注解







#### 注解



`@PathVariable`

**获取路径变量（将变量直接写在路径上）**

```java
//car/1/owner/zhangsan
@GetMapping("/car/{id}/owner/{username}")
public Map<String, Object> getCar(@PathVariable("id") Integer id, @PathVariable("username") String name,
                                   @PathVariable Map<String, String> pv){
    Map<String, Object> map = new HashMap<>();
    map.put("id", id);
    map.put("username", name);
    map.put("pv", pv);
    return map;
}
```

![image-20210214232120349](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210214232120349.png)

---



`RequestHeader`

```java
@RequestHeader("User-Agent") String userAgent,
@RequestHeader Map<String, String> header//获取全部请求头，以Map的方式存储
```

![image-20210214232519795](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210214232519795.png)



---



`@RequestParam`



```java
    @GetMapping("/car/{id}/owner/{username}")
    public Map<String, Object> getCar(@PathVariable("id") Integer id, @PathVariable("username") String name,
                                      @PathVariable Map<String, String> pv,
                                      @RequestHeader("User-Agent") String userAgent,
                                      @RequestHeader Map<String, String> header,
                                      @RequestParam("age") int age,
                                      @RequestParam("inter") List<String> inter,
                                      @RequestParam Map<String, String> params){
        Map<String, Object> map = new HashMap<>();
        map.put("age", age);
        map.put("inter", inter);
        map.put("params", params);//map的key只能有一个，故而多个相同属性名的只会计算一次
        return map;
    }
}
```

![image-20210214234238348](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210214234238348.png)





----



`@CookieValue`



```java
@CookieValue("JSESSIONID") String cookie
@CookieValue("JSESSIONID") Cookie cook)//获取Cookie对象
    
    cook.getName() .getValue()
```



---



`@RequestBody`

**获取请求体：只有POST请求才有请求体**



```html
<form action="/save" method="post">
    用户名：<input type="text" name="username"/><br>
    邮箱：<input type="text" name="email"/><br>
    <input type="submit" value="提交">
</form>
```

```java
@PostMapping("/save")
public Map postMethod(@RequestBody String content){
    Map<String, Object> map = new HashMap<>();
    map.put("content", content);
    return map;
}
```



![image-20210215001328452](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210215001328452.png)





----



`@RequestAttribute`

**获取request域中存放的属性**



**更多的是跳转到页面，页面用el表达式获取这个值${}**

```java
@GetMapping("/goto")
public String goToPage(HttpServletRequest request){
    request.setAttribute("msg", "成功了！");
    request.setAttribute("code", 200);
    return "forward:/success";  //  转发到/success请求  跳转才能保存request域中的数据
}

@ResponseBody
@GetMapping("/success")
public Map success(@RequestAttribute("msg") String msg,
                      @RequestAttribute("code") Integer code,
                      //转发到这里，用的是同一个请求
                      HttpServletRequest request){
    Object msg1 = request.getAttribute("msg");
    Map<String, Object> map = new HashMap<>();
    map.put("reqMethod", msg1);
    map.put("anno_msg", msg);
    map.put("anno_code", code);
    return map;
}
```



![image-20210215002815601](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210215002815601.png)







---





`@MatrixVariable`



/cars/{path}?xxx=xxx&aaa=bbb queryString 查询字符串。`@RequestParam`



/cars/{path; low=34;brand=byd,audi,yd} : 矩阵变量

页面开发，**cookie禁用了**，session里面的内容怎么使用

**session，set(a, b)  ----->  jsessionid  --->  cookie ---->  每次发请携带**



**url重写**：   /abc;jsessionid=xxxx    **把cookie的值使用矩阵变量的进行传递**

k=v,v,v,v

`;`后面是矩阵变量

/boss/1;age=20/2;age=20   









对于路径的处理：`UrlPathHelper`进行解析

```java
//UrlPathHelper

private boolean removeSemicolonContent = true;//移除分号内容

public void setRemoveSemicolonContent(boolean removeSemicolonContent) {
    this.checkReadOnly();
    this.removeSemicolonContent = removeSemicolonContent;
}
```

![image-20210215023014756](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210215023014756.png)

将分号后的内容都删去



```java
public interface WebMvcConfigurer {
    default void configurePathMatch(PathMatchConfigurer configurer) {
    }
```



```java
@Configuration(proxyBeanMethods = false)
@Import(EnableWebMvcConfiguration.class)
@EnableConfigurationProperties({ WebMvcProperties.class,
      org.springframework.boot.autoconfigure.web.ResourceProperties.class, WebProperties.class })
@Order(0)
public static class WebMvcAutoConfigurationAdapter implements WebMvcConfigurer {//同样实现了WebMvcConfigurer接口
```







配置类：

```java
@Configuration
//jdk8默认实现接口，无需每一个接口都实现
public class WebConfig implements WebMvcConfigurer {

//    @Bean //WebMvcConfigurer
//    public WebMvcConfigurer webMvcConfigurer(){
//        return new WebMvcConfigurer() {
//            @Override
//            public void configurePathMatch(PathMatchConfigurer configurer) {
//                UrlPathHelper urlPathHelper = new UrlPathHelper();
//                //设置为不移除分号后面的内容，矩阵变量功能就可以生效
//                urlPathHelper.setRemoveSemicolonContent(false);
//                configurer.setUrlPathHelper(urlPathHelper);
//            }
//        };
//    }

    @Override
    public void configurePathMatch(PathMatchConfigurer configurer) {
        UrlPathHelper urlPathHelper = new UrlPathHelper();
        //设置为不移除分号后面的内容，矩阵变量功能就可以生效
        urlPathHelper.setRemoveSemicolonContent(false);
        configurer.setUrlPathHelper(urlPathHelper);
    }
}
```





```java
//   /cars/sell;low=34;brand=benz,audi,bmw   或者brand=benz;brand=audi...
// springboot默认是禁用了矩阵变量的功能  手动开启
//404：sell要写成路径变量的表示法。。
//请求映射的时候：  矩阵变量必须有url路径变量才能被解析
@GetMapping("/cars/{path}")
public Map carsSell(@MatrixVariable("low") Integer low,
                    @MatrixVariable("brand") List<String> brand,
                    @PathVariable("path") String path){
    Map<String,Object> map = new HashMap<>();

    map.put("low",low);
    map.put("brand",brand);
    map.put("path",path);  //  sell 这个path就相当于后面所有属性的一个标题/代表，pathVar,紧跟在/后面，写完pathVar后再写一个个键值对（矩阵变量）
    System.out.println(map);
    return map;
}
```



![image-20210215041124440](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210215041124440.png)





```java
//  /boss/1;age=20/2;age=10
@GetMapping("/boss/{bossId}/{empId}")
//获得哪一个路径变量下age的值   不指定，默认第一个的值
public Map boss(@MatrixVariable(value = "age", pathVar = "bossId")   Integer bossAge,
                @MatrixVariable(value = "age", pathVar = "empId") Integer empAge){
    Map<String,Object> map = new HashMap<>();
    map.put("bossAge", bossAge);
    map.put("empAge", empAge);
    return map;
}
```









### POJO封装过程































### 参数处理原理









































# 开发小技巧



## Lombok



简化JavaBean开发。

`getter+setter+constructor`



```xml
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
</dependency>
```

安装插件



```java
@Component
@ConfigurationProperties(prefix = "mycar") //和配置文件中哪个前缀匹配
@Data
@ToString//编译时生成
@AllArgsConstructor//全参构造器
@NoArgsConstructor//无参构造器

@Slf4j//注入日志类
log.info("请求进来了....");
```





## DevTools



自动重启

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-devtools</artifactId>
    <optional>true</optional>
</dependency>
```

![image-20210212214546234](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210212214546234.png)

ctrl+f9 build 项目重新编译一遍

**重启Restart**

如果是静态文件，就不会重启

> **热更新**Reload  ---->  JRebel





## Spring Initializr

**项目初始化向导**



需要使用什么场景，添加相关依赖



![image-20210212215423928](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210212215423928.png)



static：静态资源

templates：页面

配置文件

![image-20210212220350768](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210212220350768.png)

引入所有需要的依赖

![image-20210212220458255](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210212220458255.png)



创建主类

![image-20210212220523715](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210212220523715.png)
















# Tip



## 配置类只有一个有参构造器



**有参构造器所有参数的值都会从容器中确定**

```java
public WebMvcAutoConfigurationAdapter(WebProperties webProperties, WebMvcProperties mvcProperties,
      ListableBeanFactory beanFactory, ObjectProvider<HttpMessageConverters> messageConvertersProvider,
      ObjectProvider<ResourceHandlerRegistrationCustomizer> resourceHandlerRegistrationCustomizerProvider,
      ObjectProvider<DispatcherServletPath> dispatcherServletPath,
      ObjectProvider<ServletRegistrationBean<?>> servletRegistrations) {
   this.mvcProperties = mvcProperties;
   this.beanFactory = beanFactory;
   this.messageConvertersProvider = messageConvertersProvider;
   this.resourceHandlerRegistrationCustomizer = resourceHandlerRegistrationCustomizerProvider.getIfAvailable();
   this.dispatcherServletPath = dispatcherServletPath;
   this.servletRegistrations = servletRegistrations;
   this.mvcProperties.checkConfiguration();
}
```



































































































# 坑



## Failed to configure a DataSource



`Failed to configure a DataSource: 'url' attribute is not specified and no embedded`



**如果导入了相关dataSource依赖而没有配置**，则会报错









## 需要注意是跳转页面还是返回值



**血的教训！！！！！！**



无论怎么尝试   都是返回404

最后发现Controller方法上标注的注解是  `@Controller`  ，**应该使用 `@RestController`！！！！！！**

难受啊。。



> 并且记住：如果不跳转页面，一样需要@ResponseBody来标注，表明此时不需要去跳转页面，但同时也不返回数据

```java
//   /cars/sell;low=34;brand=benz,audi,bmw
// springboot默认是禁用了矩阵变量的功能  手动开启
//404：sell要写成路径变量的表示法。。
@GetMapping("/cars/{path}")
public Map carsSell(@MatrixVariable("low") Integer low,
                    @MatrixVariable("brand") List<String> brand,
                    @PathVariable("path") String path){
    Map<String,Object> map = new HashMap<>();

    map.put("low",low);
    map.put("brand",brand);
    map.put("path",path);
    System.out.println(map);
    return map;
}
```















































