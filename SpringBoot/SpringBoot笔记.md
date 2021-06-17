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





## 云与原生



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

spring-boot-starter-*,   *代表某种场景



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



条件装配：满足Conditional指定的条件，则进行组件注入



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



### @ConfigurationProperties + @Component



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



> properties中自定义的属性，想绑给哪个**javaBean->一定要在容器中的组件**都可以







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

> 自动配置包？指定了默认的**包规则**
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

↓

得到的包名    **此时注解标注在主类上，主类在com.demo1包中**

![image-20210212045401125](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210212045401125.png)

↓

**Registrar将main程序所在包下的所有组件（如@Controller、@Service、@Mapper、@Component......），批量注册**

![image-20210212045534613](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210212045534613.png)







---



`@Import(AutoConfigurationImportSelector.class)`



`org.springframework.boot.autoconfigure.AutoConfigurationImportSelector#selectImports`

![image-20210212050549253](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210212050549253.png)

↓

利用 **`getAutoConfigurationEntry方法`**给容器中批量导入一些组件



这里面的组件都默认导入容器中

↓

然后：调用 `List<String> configurations = getCandidateConfigurations(annotationMetadata, attributes);`**获取到所有需要导入到容器中的配置类**

在getCandidateConfigurations方法中：

利用工厂：

```java
List<String> configurations = SpringFactoriesLoader.loadFactoryNames(getSpringFactoriesLoaderFactoryClass(),
      getBeanClassLoader());
```

↓

加载：

**`Map<String, List<String>> loadSpringFactories(ClassLoader classLoader)`** **得到所有组件**

即：

**从 `"META-INF/spring.factories"`** 位置来**获取需要加载的类**

默认扫描当前系统里面**所有 `"META-INF/spring.factories"`位置的文件**



![image-20210212051628965](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210212051628965.png)



![image-20210212051802361](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210212051802361.png)



`spring-boot-autoconfigure-2.4.2.jar`包里面也有META-INF/spring.factories文件



文件里面写死了springboot一启动就要给容器中**加载的所有配置类**

![image-20210212052225767](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210212052225767.png)



**最终得到所有需要自动加载autoconfiguration  的类**

![image-20210212050833413](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210212050833413.png)





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



> **`key: value`；kv之间有空格**

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
行内写法：  k: {k1: v1,k2: v2,k3: v3}
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

  - 可以配置**默认的静态资源路径**
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

```yaml
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
    @ConfigurationProperties("spring.web")//原先spring.resources中的属性都改为spring.web.resources
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
    <input type="submit" value="REST-POST">

<!--默认当成get方式GET-张三-->
    <input name="_method" type="hidden" value="DELETE">
    <input type="submit" value="REST-DELETE">

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



**所有对springmvc功能分析，都从`DispatcherServlet` -> `doDispatch()`方法开始**





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







#### Servlet API



> WebRequest、ServletRequest、MultipartRequest、 HttpSession、javax.servlet.http.PushBuilder、Principal、InputStream、Reader、HttpMethod、Locale、TimeZone、ZoneId



`HttpServletRequest request`

![image-20210215192042072](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210215192042072.png)



**ServletRequestMethodArgumentResolver**支持解析的参数类型：

**传入一下任意类型的参数均可**

```java
@Override
    public boolean supportsParameter(MethodParameter parameter) {
        Class<?> paramType = parameter.getParameterType();
        return (WebRequest.class.isAssignableFrom(paramType) ||
                ServletRequest.class.isAssignableFrom(paramType) ||
                MultipartRequest.class.isAssignableFrom(paramType) ||
                HttpSession.class.isAssignableFrom(paramType) ||
                (pushBuilder != null && pushBuilder.isAssignableFrom(paramType)) ||
                Principal.class.isAssignableFrom(paramType) ||
                InputStream.class.isAssignableFrom(paramType) ||
                Reader.class.isAssignableFrom(paramType) ||
                HttpMethod.class == paramType ||
                Locale.class == paramType ||
                TimeZone.class == paramType ||
                ZoneId.class == paramType);
    }
```



判断类型

![image-20210215211111095](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210215211111095.png)



获得request对象：

**拿到原生的request请求并返回**

![image-20210215211143652](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210215211143652.png)









#### 复杂参数



**Map**、**Model（map、model里面的数据会被放在`request的请求域` ，相当于调用了 `request.setAttribute`）、**

Errors/BindingResult、**RedirectAttributes（ 重定向携带数据）**、**ServletResponse（response）**、SessionStatus、UriComponentsBuilder、ServletUriComponentsBuilder





```java
@GetMapping("/params")
public String testParam(Map<String, Object> map,
                        Model model,
                        HttpServletRequest request,
                        HttpServletResponse response){
    map.put("hello", "world");
    model.addAttribute("world", "hello");
    request.setAttribute("message", "HelloWorld");
    Cookie cookie = new Cookie("c1", "v1");
    cookie.setDomain("localhost");
    response.addCookie(cookie);
    return "forward:/success";
}
```



> **表示了：给Map，Model，Request传入参数，都是保存在了Request域中**
>
> 使用 request.getAttribute来获取数据



MapMethodPrecessor可以解析Map类型的参数：

![image-20210216011613776](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210216011613776.png)



是Map，接下来解析，并放入缓存中

![image-20210216011750457](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210216011750457.png)



得到了参数解析器后：

![image-20210216011832255](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210216011832255.png)





接下来进行解析：`MapMethodPrecessor.resolveArgument()`

![image-20210216012014648](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210216012014648.png)



![image-20210216012210635](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210216012210635.png)



Map类型的参数 ，会返回mavContainier.getModel()   ---->   `BindingAwareModelMap`  **是Model也是Map**



----



第二个参数是Model



**`ModelMethodProcessor`**

![image-20210216012620718](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210216012620718.png)



> **调用的是和MapMethodResolver 中一样的方法：`mavContainer.getModel()`**，来获取到值

![image-20210216012713758](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210216012713758.png)



Map类型和Model类型，是一个对象 

![image-20210216013615229](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210216013615229.png)







方法返回值：

![image-20210216020017398](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210216020017398.png)





mavContainer中，看到model和map的值    都是同一个`BindingAwareModelMap`对象

![image-20210216020844653](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210216020844653.png)





```java
//ServletInvocableHandlerMethod
//处理返回结果
this.returnValueHandlers.handleReturnValue(returnValue, this.getReturnValueType(returnValue), mavContainer, webRequest);


//HandlerMethodReturnValueHandlerComposite
public void handleReturnValue(@Nullable Object returnValue, MethodParameter returnType, ModelAndViewContainer mavContainer, NativeWebRequest webRequest) throws Exception {
    HandlerMethodReturnValueHandler handler = this.selectHandler(returnValue, returnType);
    if (handler == null) {
        throw new IllegalArgumentException("Unknown return value type: " + returnType.getParameterType().getName());
    } else {
        handler.handleReturnValue(returnValue, returnType, mavContainer, webRequest);
    }
}


//ViewNameMethodReturnValueHandler
public void handleReturnValue(@Nullable Object returnValue, MethodParameter returnType, ModelAndViewContainer mavContainer, NativeWebRequest webRequest) throws Exception {
    if (returnValue instanceof CharSequence) {
        String viewName = returnValue.toString();
        //将视图名称存入mavContainer中
        mavContainer.setViewName(viewName);
        if (this.isRedirectViewName(viewName)) {
            mavContainer.setRedirectModelScenario(true);
        }
    } else if (returnValue != null) {
        throw new UnsupportedOperationException("Unexpected return type: " + returnType.getParameterType().getName() + " in method: " + returnType.getMethod());
    }

}

```



这是mavContainer就具备了模型和视图两个属性：

![image-20210216021503503](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210216021503503.png)





> 后续步骤见参数处理流程.....







#### 自定义对象参数



```java
/**
 *     姓名： <input name="userName"/> <br/>
 *     年龄： <input name="age"/> <br/>
 *     生日： <input name="birth"/> <br/>
 *     宠物姓名：<input name="pet.name"/><br/>
 *     宠物年龄：<input name="pet.age"/>
 */
@Data
public class Person {
    
    private String userName;
    private Integer age;
    private Date birth;
    private Pet pet;
    
}

@Data
public class Pet {

    private String name;
    private String age;

}
```





```java
//数据绑定：页面提交的请求数据（GET / POST）都可以和对象属性进行绑定
@PostMapping("/saveuser")
public Person saveuser(Person person){
    return person;
}
```



当前方法只有一个参数Person

![image-20210216180012166](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210216180012166.png)







### POJO封装过程







自定义参数类型使用：`ServletModelAttributeMethodProsessor`来解析

![image-20210216182417750](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210216182417750.png)





![image-20210216185326639](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210216185326639.png)







```java
//ModelAttributeMethodProcessor.supportsParameter()
public boolean supportsParameter(MethodParameter parameter) {
    return parameter.hasParameterAnnotation(ModelAttribute.class) || 
        //非简单属性，返回true
        this.annotationNotRequired && !BeanUtils.isSimpleProperty(parameter.getParameterType());
}

//BeanUtils
public static boolean isSimpleProperty(Class<?> type) {
    Assert.notNull(type, "'type' must not be null");
    //是不是简单类型？  返回false
    return isSimpleValueType(type) || type.isArray() && isSimpleValueType(type.getComponentType());
}

public static boolean isSimpleValueType(Class<?> type) {
    return Void.class != type && Void.TYPE != type && (ClassUtils.isPrimitiveOrWrapper(type) || Enum.class.isAssignableFrom(type) || CharSequence.class.isAssignableFrom(type) || Number.class.isAssignableFrom(type) || Date.class.isAssignableFrom(type) || Temporal.class.isAssignableFrom(type) || URI.class == type || URL.class == type || Locale.class == type || Class.class == type);
}
```

不是简单类型，`ModelAttributeMethodProcessor解析器`支持这个自定义参数类型





----



得到了resolver，接下来解析参数：



```java
args[i] = this.resolvers.resolveArgument(parameter, mavContainer, request, this.dataBinderFactory);
```

↓

```java
public Object resolveArgument(MethodParameter parameter, @Nullable ModelAndViewContainer mavContainer, NativeWebRequest webRequest, @Nullable WebDataBinderFactory binderFactory) throws Exception {
    HandlerMethodArgumentResolver resolver = this.getArgumentResolver(parameter);
    if (resolver == null) {
        throw new IllegalArgumentException("Unsupported parameter type [" + parameter.getParameterType().getName() + "]. supportsParameter should be called first.");
    } else {
        return resolver.resolveArgument(parameter, mavContainer, webRequest, binderFactory);
    }
}
```



```java
//org.springframework.web.method.annotation.ModelAttributeMethodProcessor#resolveArgument
```

![image-20210216210431780](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210216210431780.png)

如果在mavContainer中有这个对象？就直接拿出来



创建一个实例：**空Person对象**

![image-20210216210746145](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210216210746145.png)





```java
//ModelAttributeMethodProcessor.resolveArgument()
if (bindingResult == null) {
    //将请求中的数据封装到空的Person实例中
    WebDataBinder binder = binderFactory.createBinder(webRequest, attribute, name);
    if (binder.getTarget() != null) {
        if (!mavContainer.isBindingDisabled(name)) {
            //将请求中断的数据绑定进来
            this.bindRequestParameters(binder, webRequest);
        }

        this.validateIfApplicable(binder, parameter);
        if (binder.getBindingResult().hasErrors() && this.isBindExceptionRequired(binder, parameter)) {
            throw new BindException(binder.getBindingResult());
        }
    }

    if (!parameter.getParameterType().isInstance(attribute)) {
        attribute = binder.convertIfNecessary(binder.getTarget(), parameter.getParameterType(), parameter);
    }

    bindingResult = binder.getBindingResult();
}

Map<String, Object> bindingResultModel = bindingResult.getModel();
mavContainer.removeAttributes(bindingResultModel);
mavContainer.addAllAttributes(bindingResultModel);
return attribute;
```



`WebDataBinder`：web数据绑定器，将请求参数的值绑定到**指定的JavaBean里面--->attribute--->空的Person对象**

![image-20210216211245829](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210216211245829.png)



转换服务：

其中有124个Converters

![image-20210216213841545](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210216213841545.png)

http，文本传输，使用转换器将string转换为integer等其他类型



![image-20210216214003873](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210216214003873.png)



> `WebDataBinder`利用它里面的Converters将请求数据转成指定的数据类型。再次封装到JavaBean中。



```java
//ServletModelAttributeMethodProcessor
protected void bindRequestParameters(WebDataBinder binder, NativeWebRequest request) {
    ServletRequest servletRequest = (ServletRequest)request.getNativeRequest(ServletRequest.class);
    Assert.state(servletRequest != null, "No ServletRequest");
    ServletRequestDataBinder servletBinder = (ServletRequestDataBinder)binder;
    //传入原生Request进行绑定
    servletBinder.bind(servletRequest);
}

//ServletRequestDataBinder
public void bind(ServletRequest request) {
    //获得所有kv对   拿到了请求传过来的所有参数
    MutablePropertyValues mpvs = new ServletRequestParameterPropertyValues(request);
    MultipartRequest multipartRequest = (MultipartRequest)WebUtils.getNativeRequest(request, MultipartRequest.class);
    if (multipartRequest != null) {
        this.bindMultipart(multipartRequest.getMultiFileMap(), mpvs);
    } else if (StringUtils.startsWithIgnoreCase(request.getContentType(), "multipart/")) {
        HttpServletRequest httpServletRequest = (HttpServletRequest)WebUtils.getNativeRequest(request, HttpServletRequest.class);
        if (httpServletRequest != null) {
            StandardServletPartUtils.bindParts(httpServletRequest, mpvs, this.isBindEmptyMultipartFiles());
        }
    }

    this.addBindValues(mpvs, request);
    this.doBind(mpvs);
}
```



mpvs中每个参数都被封装为PropertyValue

![image-20210216221034296](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210216221034296.png)





进行转换，得到转换器getConverter()  ----->  for循环遍历所有的Converters，来匹配类型

![image-20210216223211685](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210216223211685.png)



根据源类型和目标类型来寻找converter

`GenericConversionService`在设置每一个值的时候，找它里面的所有converter，哪个可以将这个数据类型（Request带来参数的字符串）转换到指定类型（javaBean---Integer）

![image-20210216223542950](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210216223542950.png)



set每个kv

![image-20210216231116986](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210216231116986.png)





`bindRequestParameters()`这一步就结束了



得到的attribute，即Person对象中，已经得到了所有的数据

  

> **DataBinder负责将请求数据绑定到JavaBean上**







-----



也可以给WebDataBinder里面放**自己的Converter**；

`private static final class StringToNumber<T extends Number> implements Converter<String, T>`



![image-20210216225252598](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210216225252598.png)

![image-20210216230731260](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210216230731260.png)





### 自定义Converter原理



 

WebDataBinder数据绑定器中，有 ConversionService，其中注册了一百多个Converters，能将String转换成各种类型



将字符串以逗号分隔，作为pet属性的值封装进去

```html
宠物：<input name="pet" value="猫猫,3">
```

异常：   **Mismatch**    就是无法将String直接转为一个对象，**一定要指定为其属性赋值**

![image-20210217025246917](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210217025246917.png)





自定义一个Converter：



```java
//WebMvcConfigurer
/**
 * Add {@link Converter Converters} and {@link Formatter Formatters} in addition to the ones
 * registered by default.
 */
default void addFormatters(FormatterRegistry registry) {
}
```



```java
@Bean //WebMvcConfigurer
public WebMvcConfigurer webMvcConfigurer(){
    return new WebMvcConfigurer() {
        @Override
        public void configurePathMatch(PathMatchConfigurer configurer) {
            UrlPathHelper urlPathHelper = new UrlPathHelper();
            //设置为不移除分号后面的内容，矩阵变量功能就可以生效
            urlPathHelper.setRemoveSemicolonContent(false);
            configurer.setUrlPathHelper(urlPathHelper);
        }

        @Override
        public void addFormatters(FormatterRegistry registry) {
            registry.addConverter(new Converter<String, Pet>() {
                @Override
                public Pet convert(String s) {
                    //以 , 分割
                    if(!StringUtils.isEmpty(s)){
                        Pet pet = new Pet();
                        String[] split = s.split(",");
                        pet.setName(split[0]);
                        pet.setAge(Integer.parseInt(split[1]));
                        return pet;
                    }
                    return null;
                }
            });
        }
    };
}
```





---



分析：

使用`ModelAttributeMethodProcessor.resolveArgument()`解析参数：

现在的converters中有了我们自定义的converter

![image-20210217033259385](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210217033259385.png)



![image-20210217034046227](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210217034046227.png)





执行convert方法

![image-20210217033748336](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210217033748336.png)



返回到调用convert方法的地方：

![image-20210217033851955](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210217033851955.png)



得到了完成数据转换后的Pet对象，并将数据分装进去了

![image-20210217034159013](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210217034159013.png)



再向上返回，args中得到的请求参数已经确定：**将Pet属性对象封装进了Person对象中**

![image-20210217034600630](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210217034600630.png)







  







### 参数处理原理



- HandlerMapping中只好到能处理请求的Handler(Controller.method()方法)

- 为当前Handler找一个适配器`HandlerAdapter`(接口)

  `HandlerAdapter ha = this.getHandlerAdapter(mappedHandler.getHandler());`

  ![image-20210215150600061](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210215150600061.png)

  

  ```java
  protected HandlerAdapter getHandlerAdapter(Object handler) throws ServletException {
      if (this.handlerAdapters != null) {
          Iterator var2 = this.handlerAdapters.iterator();
  
          while(var2.hasNext()) {
              HandlerAdapter adapter = (HandlerAdapter)var2.next();
              if (adapter.supports(handler)) {
                  return adapter;
              }
          }
      }
  ```

  有以下几种**HandlerAdapter**：

  ![image-20210215145928952](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210215145928952.png)

  RequestMappingHandlerAdapter: 支持方法上标注**@RequestMapping**

  HandlerFunctionAdapter：支持**函数式编程**

  挨个匹配寻找合适的Adapter：

  ![image-20210215150245965](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210215150245965.png)





- 执行目标方法：

​		利用适配器来进行handle

```java
//DispatcherServler.doDispatch: 
mv = ha.handle(processedRequest, response, mappedHandler.getHandler());
```

​	

```java
//AbstractHandlerMethodAdapter
@Nullable
public final ModelAndView handle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
    return this.handleInternal(request, response, (HandlerMethod)handler);
}

		↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓

//RequestMappingHandlerAdapter
protected ModelAndView handleInternal{
    //执行目标方法
    mav = this.invokeHandlerMethod(request, response, handlerMethod);
}
```

---

- **参数解析器**

  确定将要执行的目标方法的每一个参数的值

  springmvc目标方法可以写多少种参数类型，取决于**参数类型解析器**。

- ![image-20210215151327736](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210215151327736.png)



	1. 当前解析器是否支持解析当前参数
	2. 支持：调用resolveArgument

**`HandlerMethodArgumentResolver`是一个接口，各种类型的解析器都实现了这个接口并实现了`resolveArgument`这个方法，用来判断当前的参数解析器能否解析传入的这个`parameter`**

![image-20210215151611641](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210215151611641.png)



----



**返回值处理器**   **方法的返回值都写以下这些类型：** 

![image-20210215161214079](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210215161214079.png)





将参数值解析器和返回值处理器都放入ServletInvocableHandlerMethod中

![image-20210215161534328](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210215161534328.png)

![image-20210215161358688](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210215161358688.png)





执行并处理

```java
invocableMethod.invokeAndHandle(webRequest, mavContainer, new Object[0]);
```



这一步是执行目标方法的

```java
Object returnValue = this.invokeForRequest(webRequest, mavContainer, providedArgs);
```



```java
@Nullable
public Object invokeForRequest(NativeWebRequest request, @Nullable ModelAndViewContainer mavContainer, Object... providedArgs) throws Exception {
    //获取方法的所有参数的值
    Object[] args = this.getMethodArgumentValues(request, mavContainer, providedArgs);
    if (logger.isTraceEnabled()) {
        logger.trace("Arguments: " + Arrays.toString(args));
    }
		//利用反射调用目标方法
    return this.doInvoke(args);
}
```



![image-20210215162604174](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210215162604174.png)





```java
//InvocableHandlerMethod
protected Object[] getMethodArgumentValues(NativeWebRequest request, @Nullable ModelAndViewContainer mavContainer, Object... providedArgs) throws Exception {
    MethodParameter[] parameters = this.getMethodParameters();
    if (ObjectUtils.isEmpty(parameters)) {
        return EMPTY_ARGS;
    } else {
        Object[] args = new Object[parameters.length];

        for(int i = 0; i < parameters.length; ++i) {
            MethodParameter parameter = parameters[i];
            parameter.initParameterNameDiscovery(this.parameterNameDiscoverer);
            args[i] = findProvidedArgument(parameter, providedArgs);
            if (args[i] == null) {
                //判断当前解析器是否支持解析当前参数类型
                if (!this.resolvers.supportsParameter(parameter)) {
                    throw new IllegalStateException(formatArgumentError(parameter, "No suitable resolver"));
                }

                try {
                    args[i] = this.resolvers.resolveArgument(parameter, mavContainer, request, this.dataBinderFactory);
                } catch (Exception var10) {
                    if (logger.isDebugEnabled()) {
                        String exMsg = var10.getMessage();
                        if (exMsg != null && !exMsg.contains(parameter.getExecutable().toGenericString())) {
                            logger.debug(formatArgumentError(parameter, exMsg));
                        }
                    }

                    throw var10;
                }
            }
        }
			//返回的args就是确定好的返回值
        return args;
    }
```



第几个参数，标了什么注解  索引位置     什么类型的参数

![image-20210215164515163](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210215164515163.png)





挨个判断所有参数解析器哪个支持解析这个参数（当前判断的这个parameter）

```java
@Nullable
private HandlerMethodArgumentResolver getArgumentResolver(MethodParameter parameter) {
    //从缓存中获取解析器    如果有，直接获取到后就不用再去判断
    HandlerMethodArgumentResolver result = (HandlerMethodArgumentResolver)this.argumentResolverCache.get(parameter);
    if (result == null) {
        Iterator var3 = this.argumentResolvers.iterator();

        while(var3.hasNext()) {
            HandlerMethodArgumentResolver resolver = (HandlerMethodArgumentResolver)var3.next();
            //将每个参数   放到   每个参数解析器中  来判断当前解析器是否支持当前参数类型(注解)
            if (resolver.supportsParameter(parameter)) {
                result = resolver;
                //当前resolver支持这个param，将他们放到缓存中，方便直接拿   缓存机制
                this.argumentResolverCache.put(parameter, resolver);
                break;
            }
        }
    }

    return result;
}
```



需要对每一个参数类型解析器都进行判断：一共27个

![image-20210215165604202](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210215165604202.png)





直到以下这种情况：注解类型和resolver对应上了

![image-20210215170056649](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210215170056649.png)













---





```java
//判断当前解析器是否支持解析当前参数类型
if (!this.resolvers.supportsParameter(parameter)) {
    throw new IllegalStateException(formatArgumentError(parameter, "No suitable resolver"));
}

//判断成功了 
//判断完了支持解析当前参数，继续执行InvocableHandlerMethod.getMethodArgumentValues
try {
    args[i] = this.resolvers.resolveArgument(parameter, mavContainer, request, this.dataBinderFactory);
```





---



- 解析这个参数的值------调用各自HandlerMethodArgumentResolver的resolveArgument方法即可



**自定义类型参数  封装pojo，ServletModelAttributeMethodProcessor  这个参数处理器支持**



```java
@Nullable
public Object resolveArgument(MethodParameter parameter, @Nullable ModelAndViewContainer mavContainer, NativeWebRequest webRequest, @Nullable WebDataBinderFactory binderFactory) throws Exception {
    //从缓存中 拿到当前这个参数的参数解析器  因为之前在判断支不支持参数解析器时已经放入了缓存
    HandlerMethodArgumentResolver resolver = this.getArgumentResolver(parameter);
    if (resolver == null) {
        throw new IllegalArgumentException("Unsupported parameter type [" + parameter.getParameterType().getName() + "]. supportsParameter should be called first.");
    } else {
        return resolver.resolveArgument(parameter, mavContainer, webRequest, binderFactory);
    }
}
```

![image-20210215170706075](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210215170706075.png)



解析得到参数的名称和值



都封装到了请求域中

**注意：注解中的参数名称，和实际url中的参数名称是一一对应的，故可以直接得到**

并且根据请求域中存放的不同类别的mapping，取出所有的kv对，再根据name来得到对应属性的值

![image-20210215171448920](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210215171448920.png)



![image-20210215170959872](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210215170959872.png)



成功返回：

![image-20210215171532561](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210215171532561.png)





@RequestHeader注解：



![image-20210215173553316](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210215173553316.png)







----



```java
@Nullable
public Object invokeForRequest(NativeWebRequest request, @Nullable ModelAndViewContainer mavContainer, Object... providedArgs) throws Exception {
    //根据参数，得到所有参数的  值  如下图：
    Object[] args = this.getMethodArgumentValues(request, mavContainer, providedArgs);
    if (logger.isTraceEnabled()) {
        logger.trace("Arguments: " + Arrays.toString(args));
    }

    return this.doInvoke(args);
}
```



![image-20210215172018964](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210215172018964.png)





----



再返回：



```java
public void invokeAndHandle(ServletWebRequest webRequest, ModelAndViewContainer mavContainer, Object... providedArgs) throws Exception {
    Object returnValue = this.invokeForRequest(webRequest, mavContainer, providedArgs);
```

这里returnValue： 就是当前方法接受到的**所有参数及其值**

![image-20210215163909674](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210215163909674.png)



----

参数解析器的基本类型：`HandlerMethodArgumentResolver`

有很多实现类

![image-20210215175828347](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210215175828347.png)



比如有  MatrixVariableMethodArgumentResolver ，就表示支持**@MatrixVariable**属性，这样这个参数解析器就会起作用







----



**目标方法执行完成**



将所有的数据都放在ModelAndViewContainer，包含要去的页面地址。还包含Model数据



![image-20210216044138506](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210216044138506.png)



**执行完这句话后，使用适配器执行目标方法就结束了**

```java
mv = ha.handle(processedRequest, response, mappedHandler.getHandler());
```



![image-20210216044600388](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210216044600388.png)



---





**处理派发结果：**

```java
this.processDispatchResult(processedRequest, response, mappedHandler, mv, (Exception)dispatchException);
```



```java
public void render(@Nullable Map<String, ?> model, HttpServletRequest request, HttpServletResponse response) throws Exception {
    if (this.logger.isDebugEnabled()) {
        this.logger.debug("View " + this.formatViewName() + ", model " + (model != null ? model : Collections.emptyMap()) + (this.staticAttributes.isEmpty() ? "" : ", static attributes " + this.staticAttributes));
    }

    Map<String, Object> mergedModel = this.createMergedOutputModel(model, request, response);
    this.prepareResponse(request, response);
    //再将得到的hashmap传入  渲染合并输出的模型    这里拿到请求对象↓
    this.renderMergedOutputModel(mergedModel, this.getRequestToExpose(request), response);
    //调用了：InternalResourceView#renderMergedOutputModel
}
```





返回一个HashMap，来表示model中的数据

![image-20210216050002082](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210216050002082.png)





**视图解析功能**

```java
//InternalResourceView#renderMergedOutputModel
protected void renderMergedOutputModel(Map<String, Object> model, HttpServletRequest request, HttpServletResponse response) throws Exception {
    
    //---------------------------------------------------------------------------
    //将model作为Request域的属性
    this.exposeModelAsRequestAttributes(model, request);
    //---------------------------------------------------------------------------

    
    this.exposeHelpers(request);
    String dispatcherPath = this.prepareForRendering(request, response);
    RequestDispatcher rd = this.getRequestDispatcher(request, dispatcherPath);
    if (rd == null) {
        throw new ServletException("Could not get RequestDispatcher for [" + this.getUrl() + "]: Check that the corresponding file exists within your web application archive!");
    } else {
        if (this.useInclude(request, response)) {
            response.setContentType(this.getContentType());
            if (this.logger.isDebugEnabled()) {
                this.logger.debug("Including [" + this.getUrl() + "]");
            }

            rd.include(request, response);
        } else {
            if (this.logger.isDebugEnabled()) {
                this.logger.debug("Forwarding to [" + this.getUrl() + "]");
            }

            rd.forward(request, response);
        }

    }
}
```



```java
//AbstractView.exposeModelAsRequestAttributes
protected void exposeModelAsRequestAttributes(Map<String, Object> model, HttpServletRequest request) throws Exception {
    //-------------------------------------------------------------------
    //遍历model中的所有数据，都加入Request域的属性中
    model.forEach((name, value) -> {
        if (value != null) {
            request.setAttribute(name, value);
        } else {
            request.removeAttribute(name);
        }

    });
}
```



> **进行渲染：并且这一步是在跳转之前将数据都放入请求域**

















#### 流程：



得到的mv：

![image-20210216044055752](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210216044055752.png)

```java
//DispatcherServlet.doDispatch()
mv = ha.handle(processedRequest, response, mappedHandler.getHandler());

//↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓
    
//AbstractHandlerMethodAdapter.handle()
public final ModelAndView handle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
    return this.handleInternal(request, response, (HandlerMethod)handler);
}


//↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓


//RequestMappingHandlerAdapter.handleInternal()
//这里就转换成mav对象了
mav = this.invokeHandlerMethod(request, response, handlerMethod);


//↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓


//RequestMappingHandlerAdapter.invokeHandlerMethod
//
 invocableMethod.invokeAndHandle(webRequest, mavContainer, new Object[0]);

if (!asyncManager.isConcurrentHandlingStarted()) {
    //获得mav对象
    ModelAndView var15 = this.getModelAndView(mavContainer, modelFactory, webRequest);
    return var15;
}

//先执行：ServletInvocableHandlerMethod.invokeAndHandle
//来得到方法的参数及其对应的值
Object returnValue = this.invokeForRequest(webRequest, mavContainer, providedArgs);

//再获取ModelAndView
//↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓


//RequestMappingHandlerAdapter.getModelAndView
private ModelAndView getModelAndView(ModelAndViewContainer mavContainer, ModelFactory modelFactory, NativeWebRequest webRequest) throws Exception {
    modelFactory.updateModel(webRequest, mavContainer);
    if (mavContainer.isRequestHandled()) {
        return null;
    } else {
        ModelMap model = mavContainer.getModel();
        //将mavContainer封装成ModelAndView
        ModelAndView mav = new ModelAndView(mavContainer.getViewName(), model, mavContainer.getStatus());
        if (!mavContainer.isViewReference()) {
            mav.setView((View)mavContainer.getView());
        }

        if (model instanceof RedirectAttributes) {
            Map<String, ?> flashAttributes = ((RedirectAttributes)model).getFlashAttributes();
            HttpServletRequest request = (HttpServletRequest)webRequest.getNativeRequest(HttpServletRequest.class);
            if (request != null) {
                RequestContextUtils.getOutputFlashMap(request).putAll(flashAttributes);
            }
        }

        return mav;
    }
}
```







## 数据处理与内容协商



数据处理：

- 响应页面：go->视图解析与模板引擎  **多用于单体项目**
- 相应数据   **前后端分离**



### 响应JSON



#### jackson.jar+ResponseBody



引入依赖：

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
web场景自动引入了json场景
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-json</artifactId>
      <version>2.3.4.RELEASE</version>
      <scope>compile</scope>
    </dependency>
```

![image-20210217174605589](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210217174605589.png)





给前端自动返回json数据



```java
if (this.argumentResolvers != null) {
    invocableMethod.setHandlerMethodArgumentResolvers(this.argumentResolvers);
}
if (this.returnValueHandlers != null) {
   invocableMethod.setHandlerMethodReturnValueHandlers(this.returnValueHandlers);
}
//参数解析器和返回值解析器都在invocableMethod中
```

##### 返回值解析器

![image-20210217175545217](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210217175545217.png)





```java
public void invokeAndHandle(ServletWebRequest webRequest, ModelAndViewContainer mavContainer,
      Object... providedArgs) throws Exception {
	//这一步中执行了doInvoke，执行完了目标方法  并得到了返回值，即方法中生成的Person对象
   Object returnValue = invokeForRequest(webRequest, mavContainer, providedArgs);
```

![image-20210217180138821](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210217180138821.png)





![image-20210217180340069](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210217180340069.png)



```java
try {
   this.returnValueHandlers.handleReturnValue(
         returnValue, getReturnValueType(returnValue), mavContainer, webRequest);
}
```

**利用所有的返回值处理器来处理返回值**



```java
//HandlerMethodReturnValueHandlerComposite
public void handleReturnValue(@Nullable Object returnValue, MethodParameter returnType, ModelAndViewContainer mavContainer, NativeWebRequest webRequest) throws Exception {
    //返回值对象+返回值类型    先寻找哪一个返回值处理器handler能处理
    HandlerMethodReturnValueHandler handler = this.selectHandler(returnValue, returnType);
    if (handler == null) {
        throw new IllegalArgumentException("Unknown return value type: " + returnType.getParameterType().getName());
    } else {
        handler.handleReturnValue(returnValue, returnType, mavContainer, webRequest);
    }
}


@Nullable
private HandlerMethodReturnValueHandler selectHandler(@Nullable Object value, MethodParameter returnType) {
    //是不是异步的处理器
    boolean isAsyncValue = this.isAsyncReturnValue(value, returnType);
    Iterator var4 = this.returnValueHandlers.iterator();

    HandlerMethodReturnValueHandler handler;
    do {
        do {
            if (!var4.hasNext()) {
                return null;
            }

            handler = (HandlerMethodReturnValueHandler)var4.next();
        } while(isAsyncValue && !(handler instanceof AsyncHandlerMethodReturnValueHandler));
    } while(!handler.supportsReturnType(returnType));

    return handler;
}
```





> 返回值处理器：先判断是否支持这种类型返回值 `supportsReturnType()`，再调用返回值处理器 `handleReturnValue`
>
> ```java
> public interface HandlerMethodReturnValueHandler {
>     boolean supportsReturnType(MethodParameter var1);
> 
>     void handleReturnValue(@Nullable Object var1, MethodParameter var2, ModelAndViewContainer var3, NativeWebRequest var4) throws Exception;
> }
> ```



```java
//RequestResponseBodyMethodProcessor
@Override
public boolean supportsReturnType(MethodParameter returnType) {
    //@ResponseBody注解
   return (AnnotatedElementUtils.hasAnnotation(returnType.getContainingClass(), ResponseBody.class) ||
         returnType.hasMethodAnnotation(ResponseBody.class));
}
```







-----



##### 返回值解析器原理



```java
//HandlerMethodReturnValueHandlerComposite
public void handleReturnValue(@Nullable Object returnValue, MethodParameter returnType, ModelAndViewContainer mavContainer, NativeWebRequest webRequest) throws Exception {
    HandlerMethodReturnValueHandler handler = this.selectHandler(returnValue, returnType);
    if (handler == null) {
        throw new IllegalArgumentException("Unknown return value type: " + returnType.getParameterType().getName());
    } else {
        //接下来处理返回值
        handler.handleReturnValue(returnValue, returnType, mavContainer, webRequest);
    }
}
```



使用 `RequestResponseBodyMethodProcessor`来处理返回值(标了@ResponseBody的)

**MessageConverters 在 `RequestResponseBodyMethodProcessor`中进行调用，所以前提是使用 @ResponseBody注解**

```java
//RequestResponseBodyMethodProcessor
@Override
public void handleReturnValue(@Nullable Object returnValue, MethodParameter returnType,
      ModelAndViewContainer mavContainer, NativeWebRequest webRequest)
      throws IOException, HttpMediaTypeNotAcceptableException, HttpMessageNotWritableException {

   mavContainer.setRequestHandled(true);
   ServletServerHttpRequest inputMessage = createInputMessage(webRequest);
   ServletServerHttpResponse outputMessage = createOutputMessage(webRequest);

   // Try even with null return value. ResponseBodyAdvice could get involved.
   // 使用消息转换器进行写出操作
   writeWithMessageConverters(returnValue, returnType, inputMessage, outputMessage);
}
```

> `AbstractMessageConverterMethodProcessor`类



利用 `MessageConverters`进行处理，将数据写为json

- 内容协商。浏览器会以请求头的方式告诉服务器他能接收什么样的内容类型
- 服务器最终根据自己自身的能力，决定服务器能生产出什么样的内容类型的数据
- springmvc会挨个遍历所有容器底层的`HttpMessageConverter`，看谁能处理
  - 得到`MappingJackson2HttpMessageConverter`可以将对象写为json
  - 利用`MappingJackson2HttpMessageConverter`将对象转为json再写出去

![image-20210217211904243](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210217211904243.png)











能接收的类型：

![image-20210217212344963](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210217212344963.png)



服务器能生产的类型：

![image-20210217212715628](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210217212715628.png)



两端的类型开始匹配

```java
List<MediaType> mediaTypesToUse = new ArrayList<>();
//共28次循环，在mediaTypesToUse中放入到底能写出什么类型的数据
for (MediaType requestedType : acceptableTypes) {
   for (MediaType producibleType : producibleTypes) {
      if (requestedType.isCompatibleWith(producibleType)) {
         mediaTypesToUse.add(getMostSpecificMediaType(requestedType, producibleType));
      }
   }
}
```



因为浏览器直接能接收：`*/*`

![image-20210217213022308](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210217213022308.png)





#### HTTPMessageConverter原理





**`HttpMessageConverter`：看是否支持将此Class类型的对象转为MediaType类型的数据**

（例：支不支持将Person对象转为JSON,或者JSON转为Person）

![image-20210217213644699](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210217213644699.png)







遍历所有messageConverters  （**默认的MessageConverter**）

![image-20210217213334132](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210217213334132.png)



```java
public boolean canWrite(Class<?> clazz, @Nullable MediaType mediaType) {
   return supports(clazz/*返回值类型valueType*/) && canWrite(mediaType);
}
```



0. 只支持Byte类型的返回值

1. 只支持String类型

2. 同1

3. Resource类型

4. ResourceRegion

5. DOMSource.class \  SAXSource.class   \  StAXSource.class  \  StreamSource.class  \  Source.class

6. MultiValueMap

7. ![image-20210217225128506](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210217225128506.png)

   `MappingJackson2HttpMessageConverter`

8. 同7



![image-20210217225603781](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210217225603781.png)



判断成功：接下来使用write方法写为json格式：

```java
genericConverter.write(body, targetType, selectedMediaType, outputMessage);
```



`AbstractGenericHttpMessageConverter#write`

![image-20210217230134323](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210217230134323.png)





![image-20210217230407278](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210217230407278.png)



最终：`MappingJackson2HttpMessageConverter`把对象转为json放到response中 

**利用底层的jackson的objectMapper转换的**

![image-20210217230708585](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210217230708585.png)











#### springmvc支持哪些返回值类型

`supportsReturnType()`

```java
ModelAndView
Model
View
ResponseEntity
ResponseBodyEmitter
StreamingResponseBody
HttpEntity
HttpHeaders
Callable 异步？
DeferredResult
ListenableFuture
CompletionStage
WebAsyncTask
方法标注@ModelAttribute注解
方法标注@ResponseBody注解   ---->   RequestResponseBodyMethodProcessor
```





### 内容协商



**根据客户端*接收能力*  不同，返回不同媒体类型的数据**



#### 引入xml依赖



 

```xml
<dependency>
    <groupId>com.fasterxml.jackson.dataformat</groupId>
    <artifactId>jackson-dataformat-xml</artifactId>
</dependency>
```



#### postman分别测试返回json和xml



这时浏览器返回的内容类型是：比`*/*`优先接收

![image-20210218160931692](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210218160931692.png)

而postman返回的是json形式

修改accept

![image-20210218161523099](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210218161523099.png)



只需要改变请求头中Accept字段，Http协议中规定的，告诉服务器本客户端可以接收的数据类型







#### 开启浏览器参数方式内容协商功能



对于浏览器：没有application/json，只能使用 `*/*`来匹配application/json

**按照权重优先匹配原则，优先匹配application/xml**

![image-20210218190449456](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210218190449456.png)



**权重！！json的低，虽然能匹配到**

![image-20210218190610105](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210218190610105.png)

找到第一个最佳匹配：xml

![image-20210218190658891](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210218190658891.png)







------





为了方便内容协商，开启基于请求参数的内容协商功能。

**开启基于请求参数的内容协商功能**：

```yaml
spring:
  web:
    resources:
      add-mappings: true
```

**带上format字段**

`http://localhost:8080/test/person?format=xml`

`http://localhost:8080/test/person?format=json`



![image-20210218212604351](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210218212604351.png)





开启功能后，多了一个基于参数的内容协商策略

其中也规定了参数，即支持的key：只能写json/xml

![image-20210218213251828](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210218213251828.png)





确定客户端接收什么样的内容类型；



```java
//ContentNegotiationManager
@Override
public List<MediaType> resolveMediaTypes(NativeWebRequest request) throws HttpMediaTypeNotAcceptableException {
   for (ContentNegotiationStrategy strategy : this.strategies) {
      List<MediaType> mediaTypes = strategy.resolveMediaTypes(request);
      if (mediaTypes.equals(MEDIA_TYPE_ALL_LIST)) {
         continue;
      }
      return mediaTypes;
   }
   return MEDIA_TYPE_ALL_LIST;
}
```

↓

```java
//AbstractMappingContentNegotiationStrategy	
//key:json
public List<MediaType> resolveMediaTypeKey(NativeWebRequest webRequest, @Nullable String key)
      throws HttpMediaTypeNotAcceptableException {

   if (StringUtils.hasText(key)) {
      MediaType mediaType = lookupMediaType(key);
      if (mediaType != null) {
         handleMatch(key, mediaType);
         return Collections.singletonList(mediaType);
      }
      mediaType = handleNoMatch(webRequest, key);
      if (mediaType != null) {
         addMapping(key, mediaType);
         return Collections.singletonList(mediaType);
      }
   }
    //如果找不到format参数对应的value，就默认使用*/*，并继续进行下一个strategy的尝试
    //-------(开启了基于请求参数的内容协商功能但没有提供有有效的format参数时)-------
   return MEDIA_TYPE_ALL_LIST;
}
```



1. Parameter策略优先确定是要返回json数据（**获取请求头中的format的值**）

   ​	![image-20210218214310567](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210218214310567.png)



只要解析出了不是 `*/*`的媒体类型，就直接返回。

![image-20210218214535784](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210218214535784.png)



**parameter优先！**

2. 







#### 内容协商原理



先简单debug走一下，得到返回值：

![image-20210218162054938](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210218162054938.png)



处理返回值：

在 `RequestResponseBodyMethodProcessor`中：

![image-20210218162224457](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210218162224457.png)



1. 判断当前响应头中是否已经有确定的媒体类型：MediaType

   `MediaType contentType = outputMessage.getHeaders().getContentType();`

2. 获取客户端(PostMan、浏览器)支持接收的内容类型  ---->  **获取客户端Accept请求头字段**: application/xml

   - `contentNegotiationManager` **内容协商管理器**   默认使用基于请求头的策略来获取Accept字段  

     ![image-20210218211749960](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210218211749960.png)

   - `HeaderContentNegotiationStrategy`确定客户端可以接收的内容类型

   ![image-20210218211957226](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210218211957226.png)

   

   

   `List<MediaType> acceptableTypes = getAcceptableMediaTypes(request);`



​		返回得到acceptableTypes 的值![image-20210218162953297](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210218162953297.png)

![image-20210218163210682](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210218163210682.png)







3. 遍历循环所有的当前西永的MessageConverter，看谁支持操作这个对象--->Person

4. 找到支持操作Person的Converter，把Converter支持的媒体类型mediatype统计出来 `converter.getSupportedMediaTypes()`

   `List<MediaType> producibleTypes = getProducibleMediaTypes(request, valueType, targetType);`



​		拿到所有的messageConverters:

​		![image-20210218163945596](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210218163945596.png)



所有前面的Converter都不支持转换Person对象，

```java
protected List<MediaType> getProducibleMediaTypes(
      HttpServletRequest request, Class<?> valueClass, @Nullable Type targetType) {

   Set<MediaType> mediaTypes =
         (Set<MediaType>) request.getAttribute(HandlerMapping.PRODUCIBLE_MEDIA_TYPES_ATTRIBUTE);
   if (!CollectionUtils.isEmpty(mediaTypes)) {
      return new ArrayList<>(mediaTypes);
   }
   else if (!this.allSupportedMediaTypes.isEmpty()) {
      List<MediaType> result = new ArrayList<>();
       //遍历所有MessageConverters
      for (HttpMessageConverter<?> converter : this.messageConverters) {
         if (converter instanceof GenericHttpMessageConverter && targetType != null) {
            if (((GenericHttpMessageConverter<?>) converter).canWrite(targetType, valueClass, null)) {
                //将当前这个converter支持的媒体类型放入列表中
               result.addAll(converter.getSupportedMediaTypes());
            }
         }
         else if (converter.canWrite(valueClass, null)) {
            result.addAll(converter.getSupportedMediaTypes());
         }
      }
      return result;
   }
   else {
      return Collections.singletonList(MediaType.ALL);
   }
}
```



![image-20210218165646074](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210218165646074.png)



![image-20210218171347895](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210218171347895.png)





![image-20210218171556982](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210218171556982.png)



```java
public MappingJackson2XmlHttpMessageConverter(ObjectMapper objectMapper) {
   super(objectMapper, new MediaType("application", "xml", StandardCharsets.UTF_8),
         new MediaType("text", "xml", StandardCharsets.UTF_8),
         new MediaType("application", "*+xml", StandardCharsets.UTF_8));
   Assert.isInstanceOf(XmlMapper.class, objectMapper, "XmlMapper required");
}
```

最终的结果：

![image-20210218173114734](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210218173114734.png)



**当前系统对于当次请求，支持返回json以及xml的数据**



5. **客户端需要**[application/xml]，**服务端能力**[result ↑ 可以将Person对象转换输出10种类型的数据]，是底层两个converters的结果





6. 进行内容协商的最佳匹配媒体类型

   ![image-20210218174218872](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210218174218872.png)



```java
//AbstractMessageConverterMethodProcessor.writeWithMessageConverters()
for (MediaType requestedType : acceptableTypes) {//能接收的
   for (MediaType producibleType : producibleTypes) {//服务器产生的
      if (requestedType.isCompatibleWith(producibleType)) {
         mediaTypesToUse.add(getMostSpecificMediaType(requestedType, producibleType));
      }
   }
}
```

最终得到的可以使用的

![image-20210218181659066](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210218181659066.png)







7. 用支持 **将对象转为最佳匹配媒体类型** 的Converter，并调用他进行转换

再次遍历所有Converters

![image-20210218182923322](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210218182923322.png)





来到write()方法(`AbstractGenericHttpMessageConverter`)

执行底层转换：

![image-20210218185925255](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210218185925255.png)









#### 自定义MessageConverter



实现多协议数据兼容。



1. `@ResponseBody` 相应数据出去  调用`RequestResponseBodyMethodProcessor`
2. `Processor` 处理方法返回值。通过`MessageConverter`处理

3. 所有的MessageConverter合起来可以支持**各种媒体类型数据的操作**（读and写）
4. 内容协商找到最终的 `MessageConverter`



`WebMvcAutoConfiguration.configureMessageConverters()`创建Converter对象

 

![image-20210218231000527](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210218231000527.png)



导入了jackson处理xml的包，xml的Converter就会自动进来

```java
WebMvcConfigurationSupport
jackson2XmlPresent = ClassUtils.isPresent("com.fasterxml.jackson.dataformat.xml.XmlMapper", classLoader);
```



----



要配置SpringMVC的任何功能，一个入口  给容器中添加一个WebMvcConfigurer



```java
/**
 * 自定义的Converter
 */
public class ARuiMessageConverter implements HttpMessageConverter<Person> {

    @Override
    public boolean canRead(Class<?> clazz, MediaType mediaType) {
        return false;
    }

    @Override
    public boolean canWrite(Class<?> clazz, MediaType mediaType) {
        return clazz.isAssignableFrom(Person.class);
    }

    /**
     * 服务器要统计所有messageConverter都能写出哪些内容类型
     * application/x-arui
     * @return
     */
    @Override
    public List<MediaType> getSupportedMediaTypes() {
        //将字符串解析成媒体类型集合
        //Invalid mime type "x-arui": does not contain '/'  一定要写完整名字
        return MediaType.parseMediaTypes("application/x-arui");
    }

    @Override
    public Person read(Class<? extends Person> clazz, HttpInputMessage inputMessage) throws IOException, HttpMessageNotReadableException {
        return null;
    }

    @Override
    public void write(Person person, MediaType contentType, HttpOutputMessage outputMessage) throws IOException, HttpMessageNotWritableException {
        //自定义协议数据的写出
        String data = person.getUserName() + ";" + person.getAge() + ";" + person.getBirth();

        //写出去
        OutputStream body = outputMessage.getBody();
        body.write(data.getBytes());
    }
}
```



> HttpMessageConverter.canRead():
>
> getPerson(@RequestBody Person person)
>
> 传过来的是xml/json/自定义类型，将传过来的值  **读为**  要接收的类型---Person

**配置文件：向converters中添加自定义Converter类**

```java
@Bean //WebMvcConfigurer
public WebMvcConfigurer webMvcConfigurer(){
    return new WebMvcConfigurer() {
        @Override
        //不是修改---->额外功能的添加
        public void extendMessageConverters(List<HttpMessageConverter<?>> converters) {
            converters.add(new ARuiMessageConverter());
        }
    }
}
```



```java
/**
 * 1.浏览器发请求，直接返回xml      [application/xml]  jackson-xml-converter
 * 2.如果是ajax请求  返回json      [application/json] jackson-json-converter
 * 3.如果用app发请求，返回自定义协议数据 [application/x-arui]   xxxconverter
 *         格式 :属性值1;属性值2;
 * -----------------------------------------------------------------------------
 * 1.添加自定义的MessageConverter进系统底层
 * 2.系统底层就会统计出所有MessageConverter能操作哪些类型
 * 3.客户端内容协商[arui--->arui]
 * @return
 */
@GetMapping("/test/person")
@ResponseBody  //利用返回值处理器里面的消息转换器进行处理
public Person getPerson(){
    Person person = new Person();
    person.setAge(21);
    SimpleDateFormat sd = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
    sd.setTimeZone(TimeZone.getTimeZone("GMT+8"));
    person.setBirth(sd.format(new Date()));
    person.setUserName("dahuang");
    return person;
}
```



客户端能接收的：application/x-arui

![image-20210219001535924](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210219001535924.png)



自定义的Converter：

![image-20210218234524388](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210218234524388.png)



得到服务器可以产生的媒体类型：application/x-ari

![image-20210219001840947](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210219001840947.png)



随后进行最佳匹配,并从所有Converters中寻找可以处理application/x-ari类型的









#### 浏览器与Postman内容协商完全适配



内容协商管理器：    **参数形式的策略strategy**：只支持两种参数

![image-20210219035810759](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210219035810759.png)



自定义内容协商策略(自定义内容协商管理器)



```java
//构造---map
public ParameterContentNegotiationStrategy(Map<String, MediaType> mediaTypes) {
   super(mediaTypes);
}
```



```java
@Bean //WebMvcConfigurer
public WebMvcConfigurer webMvcConfigurer(){
    return new WebMvcConfigurer() {
        /**
         * 自定义内容协商策略
         * @param configurer
         */
        @Override
        public void configureContentNegotiation(ContentNegotiationConfigurer configurer) {
            Map<String, MediaType> mediaTypes = new HashMap<>();
            //指定支持解析哪些参数对应的哪些媒体类型
            mediaTypes.put("json", MediaType.APPLICATION_JSON);
            mediaTypes.put("xml", MediaType.APPLICATION_XML);
            mediaTypes.put("x-arui", MediaType.parseMediaType("application/x-arui"));
            //Map<String, MediaType> mediaTypes
            ParameterContentNegotiationStrategy parameterStrategy = new ParameterContentNegotiationStrategy(mediaTypes);
            //通过 parameterStrategy.setParameterName()来修改参数format的名称
            configurer.strategies(Arrays.asList(parameterStrategy));
        }

        @Override
        //不是修改----额外功能的添加
        public void extendMessageConverters(List<HttpMessageConverter<?>> converters) {
            converters.add(new ARuiMessageConverter());
        }
    }
}
```



自定义的类型已经加入到mediaTypes中

![image-20210219041532386](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210219041532386.png)



> **但 `HeaderContentNegotiationStrategy`就没有加入到内容协商管理器中**，**不会接收发送请求中的Accept参数**,  这样如果不设定format参数，发送的请求 得到的网页端可接收的类型就是 `*/*`，会直接匹配所有类型 ，  即： 服务器可产生的类型中的第一个 `application/json`



**也向`HeaderContentNegotiationStrategy`内容协商管理器中加入`HeaderContentNegotiationStrategy`**

```java
HeaderContentNegotiationStrategy headerContentNegotiationStrategy = new HeaderContentNegotiationStrategy();
configurer.strategies(Arrays.asList(parameterStrategy, headerContentNegotiationStrategy));
```



> 还可以自定义，实现ContentNegotiationStrategy接口





-----



```java
/**
 * 自定义的Converter
 */
public class ARuiMessageConverter implements HttpMessageConverter<Person> {

    @Override
    public boolean canRead(Class<?> clazz, MediaType mediaType) {
        return false;
    }

    @Override
    public boolean canWrite(Class<?> clazz, MediaType mediaType) {
        /**
         * 这里应该完善，同时判断class和 mediaType
         * return supports(clazz) && canWrite(mediaType);
         * 将其修改为不只对Person类起作用，对所有的类都起作用，像Jackson2HttpMessageConverter一样
         * 不过这样write方法有点难实现 .... 每个类的属性个数、名称都不同
         */
        return clazz.isAssignableFrom(Person.class);
    }
```



-------



> 有可能添加的自定义的功能，会覆盖默认很多功能，导致一些默认的功能失效













## 视图解析与模板引擎



视图解析：**SpringBoot默认不支持 JSP，需要引入第三方模板引擎技术实现页面渲染。**



 







### 视图解析原理



#### redirect:/index.html 处理过程



1. 在dispatcherservlet中，通过获得handler，即`requestmappinghandlermapping`

![image-20210220040948176](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210220040948176.png)



得到请求`/index`映射的方法`index()`

![image-20210220041028175](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210220041028175.png)





2. 找到适配器 `RequestMappingHandlerAdapter`

   ```
   HandlerAdapter ha = getHandlerAdapter(mappedHandler.getHandler());
   ```
   
   

3. 执行handle方法

   ```
   mv = ha.handle(processedRequest, response, mappedHandler.getHandler());
   
   
   mav = invokeHandlerMethod(request, response, handlerMethod);
   ```
   
   

4. 在 `invokeForRequest`方法中解析完参数后， 执行映射请求的方法  invoke

   ```java
   public void invokeAndHandle(ServletWebRequest webRequest, ModelAndViewContainer mavContainer,
         Object... providedArgs) throws Exception {
   
      Object returnValue = invokeForRequest(webRequest, mavContainer, providedArgs);
   ```

   ```java
   public Object invokeForRequest(NativeWebRequest request, @Nullable ModelAndViewContainer mavContainer, Object... providedArgs) throws Exception {
       Object[] args = this.getMethodArgumentValues(request, mavContainer, providedArgs);
   ```

   ![image-20210220042309805](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210220042309805.png)



5. 方法返回 **重定向**

   `return "redirect:/index.html";//return "index" -> 默认是转发  每次刷新都重新提交表单`



​		`invokeForRequest`方法执行完成，**解析参数并执行目标方法最后获得返回值**

​		`Object returnValue = invokeForRequest(webRequest, mavContainer, providedArgs);`

​		返回值：

![image-20210220043025682](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210220043025682.png)





6. 解析返回值

   ```java
   this.returnValueHandlers.handleReturnValue(
         returnValue, getReturnValueType(returnValue), mavContainer, webRequest);
   ```

> 之前解析`@Response`时，使用 `RequestResponseBodyMethodProcessor`，使用里面的 `MessageConverter`进行数据类型转换
>
> ​	![image-20210220043329109](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210220043329109.png)



这里循环查找判断使用哪个handler

![image-20210220043751088](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210220043751088.png)

↓

这里返回的是"redirect:/index.html"  所以是String类型，

![image-20210220044059132](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210220044059132.png)



> 这里没有使用@ResponseBody来给页面返回数据



7. 使用  `ViewNameMethodReturnValueHandler`

   ```java
   @Override
   public boolean supportsReturnType(MethodParameter returnType) {
      Class<?> paramType = returnType.getParameterType();
       //返回为空  ||  字符串
      return (void.class == paramType || CharSequence.class.isAssignableFrom(paramType));
   }
   ```



-----





8. **目标方法处理的过程中，所有数据都会被放在`ModelAndViewContainer` 里面，包括数据和视图地址**

   `handler.handleReturnValue(returnValue, returnType, mavContainer, webRequest);`

   ```java
   public void handleReturnValue(@Nullable Object returnValue, MethodParameter returnType,
         ModelAndViewContainer mavContainer, NativeWebRequest webRequest) throws Exception {
   
      if (returnValue instanceof CharSequence) {
         String viewName = returnValue.toString();
          //放到ModelAndViewContainer容器中
         mavContainer.setViewName(viewName);
          //视图名称是否是重定向操作
         if (isRedirectViewName(viewName)) {
             //开启转发行为
            mavContainer.setRedirectModelScenario(true);
         }
          /* viewName以redirect:开头?
          	protected boolean isRedirectViewName(String viewName) {	
   		return (PatternMatchUtils.simpleMatch(this.redirectPatterns, viewName) || viewName.startsWith("redirect:"));
   	}
          */
      }
      else if (returnValue != null) {
         // should not happen
         throw new UnsupportedOperationException("Unexpected return type: " +
               returnType.getParameterType().getName() + " in method: " + returnType.getMethod());
      }
   }
   ```





9. `invocableMethod.invokeAndHandle(webRequest, mavContainer);`执行完成

   接下来得到ModelAndView对象并返回

    `return getModelAndView(mavContainer, modelFactory, webRequest);`



> ​	只要方法的参数是个**自定义类型对象-->从请求参数中确定的**(User)，这个对象也会被放到model中，作为defaultmodel放在MacContainer中

![image-20210220045614840](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210220045614840.png)



```java
private ModelAndView getModelAndView(ModelAndViewContainer mavContainer,
      ModelFactory modelFactory, NativeWebRequest webRequest) throws Exception {

   modelFactory.updateModel(webRequest, mavContainer);
   if (mavContainer.isRequestHandled()) {
      return null;
   }
    //获取model，和参数中的model是一个对象
   ModelMap model = mavContainer.getModel();
    //将mavContainer中的试图名称和model都放入ModelAndView对象中
   ModelAndView mav = new ModelAndView(mavContainer.getViewName(), model, mavContainer.getStatus());
   if (!mavContainer.isViewReference()) {
      mav.setView((View) mavContainer.getView());
   }
    //重定向携带数据  也可以写入参数中，其实就是个Model
    //public interface RedirectAttributes extends Model
   if (model instanceof RedirectAttributes) {
      Map<String, ?> flashAttributes = ((RedirectAttributes) model).getFlashAttributes();
      HttpServletRequest request = webRequest.getNativeRequest(HttpServletRequest.class);
      if (request != null) {
         RequestContextUtils.getOutputFlashMap(request).putAll(flashAttributes);
      }
   }
   return mav;
}
```



![image-20210220050308295](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210220050308295.png)





10. 将 `getModelAndView` 获得的mav对象 :如上图:arrow_up:    返回到 `handleInternal`方法中的调用 ：

    `mav = invokeHandlerMethod(request, response, handlerMethod);`



至此：`mv = ha.handle(processedRequest, response, mappedHandler.getHandler());`执行结束，并得到了当前请求的`ModelAndView`对象

![image-20210220052829485](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210220052829485.png)



------

11. ​	**处理派发结果---页面该如何响应**

     `processDispatchResult(processedRequest, response, mappedHandler, mv, dispatchException);`

    -  进行页面渲染逻辑 `render(mv, request, response);`

      - 根据方法的String返回值（viewname），得到`View`对象 --> [**定义了页面的渲染逻辑**]

        `view = resolveViewName(viewName, mv.getModelInternal(), locale, request);`

        - 所有的视图解析器尝试是否能根据当前返回值，得到view对象

          ![image-20210220150428135](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210220150428135.png)

          

          Thymeleaf视图解析器 new了一个 `RedirectView`并返回

          `ContentNegotiatingViewResolver`里面包含了下面所有的视图解析器，在内部还是利用下面的所有视图解析器得到视图对象

​							![image-20210220154432325](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210220154432325.png)

> 在`DispatcherServlet#resolveViewName`中，遍历所有的`ViewResolvers`，首先遍历到`ContentNegotiatingViewResolver`，在这里获得 `List<View> candidateViews = getCandidateViews(viewName, locale, requestedMediaTypes);`，并且在 `getCandidateViews` 这一方法中，再次遍历`ContentNegotiatingViewResolver`中所有的ViewResolvers ，其中`ThymeleafViewResolver` new了一个RedirectView并返回
>
> 都不需要`ContentNegotiatingViewResolver`后面的几个视图解析器来工作
>
> ![image-20210220153106909](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210220153106909.png)



​					根据返回值（viewname）得到了一个`RedirectView`，返回一个view

![image-20210220150804811](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210220150804811.png)





---



​			视图对象调用render方法进行页面渲染	`view.render(mv.getModelInternal(), request, response);`

​		![image-20210220155106873](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210220155106873.png)



​		`renderMergedOutputModel(mergedModel, getRequestToExpose(request), response);`



- 获取目标url地址

  ```java
  protected void renderMergedOutputModel(Map<String, Object> model, HttpServletRequest request,
  			HttpServletResponse response) throws IOException {
  
      String targetUrl = createTargetUrl(model, request);// -->  /index.html
      targetUrl = updateTargetUrl(targetUrl, model, request, response);
  
      // Save flash attributes
      RequestContextUtils.saveOutputFlashMap(targetUrl, request, response);
  
      // Redirect   重定向
      sendRedirect(request, response, targetUrl, this.http10Compatible);
  }
  //↓调用servlet中原始方法---->重定向！
  response.sendRedirect(encodedURL);
  ```

- `response.sendRedirect(encodedURL);`



> RedirectView如何渲染？？   **就是重定向到指定页面**   

请求结束:+1:







----





#### /dynamic_table 请求





向model中添加数据

![image-20210220165628591](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210220165628591.png)



- 判断是否使用Thymeleaf视图解析器：

  - 返回值（视图名称）以redirect: 前缀开始的  ---  `new RedirectView`    它的底层render是重定向逻辑

  - 返回值以forward: 前缀开始的   --- `new InternalResourceView `   它的底层是  `request.getRequestDispatcher(path)`  `forword(request,response)` **转发**过去

  - `ThymeleafViewResolver#loadView`

    创建一个视图对象`ThymeleafView`

    ```java
    view = (AbstractThymeleafView) beanFactory.configureBean(viewInstance, viewName);
    ```

    **如果返回值不是上面两个方法----返回值是普通字符串：new ThymeleafView**

![image-20210220171805566](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210220171805566.png)



- 执行 `ThymeleafView`的render()方法

```java
public void render(final Map<String, ?> model, final HttpServletRequest request, final HttpServletResponse response)
        throws Exception {
    renderFragment(this.markupSelectors, model, request, response);
}
```



拿到需要给页面渲染的数据  model：

![image-20210220172242015](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210220172242015.png)



经过各种处理后：

```java
viewTemplateEngine.process(templateName, processMarkupSelectors, context, templateWriter);
```





**自定义视图解析器+自定义视图 ( render() )**







### 模板引擎-Thymeleaf



**现代化、服务端Java模板引擎**







#### 基本语法





**表达式：**

| 表达式名字 | 语法   | 用途                               |
| ---------- | ------ | ---------------------------------- |
| 变量取值   | ${...} | 获取请求域、session域、对象等值    |
| 选择变量   | *{...} | 获取上下文对象值                   |
| 消息       | #{...} | 获取国际化等值                     |
| 链接       | @{...} | 生成链接                           |
| 片段表达式 | ~{...} | jsp:include 作用，引入公共页面片段 |





---



字面量：

文本值: **'one text'** **,** **'Another one!'** **,…**数字: **0** **,** **34** **,** **3.0** **,** **12.3** **,…**布尔值: **true** **,** **false**

空值: **null**

变量： one，two，.... 变量不能有空格



----



文本操作



字符串拼接: **+**

变量替换: **|The name is ${name}|** 





-----



条件运算:

If-then: **(if) ? (then)**

If-then-else: **(if) ? (then) : (else)**

Default: (value) **?: (defaultvalue)**



----





特殊操作



无操作： _





#### 设置属性值-th:attr



设置单个值

```html
<form action="subscribe.html" th:attr="action=@{/subscribe}">
  <fieldset>
    <input type="text" name="email" />
    <input type="submit" value="Subscribe!" th:attr="value=#{subscribe.submit}"/>
  </fieldset>
</form>
```



设置多个值

`<img src="../../images/xxx.png"  th:attr="src=@{/images/xxx.png},title=#{logo},alt=#{logo}" />`





以上两个的代替写法 th:xxx

`<input type="submit" value="Subscribe!" th:value="#{subscribe.submit}"/> <form action="subscribe.html" th:action="@{/subscribe}">`





#### 迭代



```html
<tr th:each="prod : ${prods}">
        <td th:text="${prod.name}">Onions</td>
        <td th:text="${prod.price}">2.41</td>
        <td th:text="${prod.inStock}? #{true} : #{false}">yes</td>
</tr>


<tr th:each="prod,iterStat : ${prods}" th:class="${iterStat.odd}? 'odd'">
  <td th:text="${prod.name}">Onions</td>
  <td th:text="${prod.price}">2.41</td>
  <td th:text="${prod.inStock}? #{true} : #{false}">yes</td>
</tr>
```







#### 条件运算



```html
<a href="comments.html"
th:href="@{/product/comments(prodId=${prod.id})}"
th:if="${not #lists.isEmpty(prod.comments)}">view</a>

<div th:switch="${user.role}">
  <p th:case="'admin'">User is an administrator</p>
  <p th:case="#{roles.manager}">User is a manager</p>
  <p th:case="*">User is some other thing</p>
</div>
```





#### 属性优先级



![image.png](../picture/SpringBoot%E7%AC%94%E8%AE%B0/1605498132699-4fae6085-a207-456c-89fa-e571ff1663da.png)





### thymeleaf的使用



导入starter依赖

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
```



自动配置好了thymeleaf



```java
@Configuration(proxyBeanMethods = false)
@EnableConfigurationProperties(ThymeleafProperties.class)
@ConditionalOnClass({ TemplateMode.class, SpringTemplateEngine.class })
@AutoConfigureAfter({ WebMvcAutoConfiguration.class, WebFluxAutoConfiguration.class })
public class ThymeleafAutoConfiguration { }
```





自动配好的策略

- 1、所有thymeleaf的**配置值**都在 `ThymeleafProperties`
- 2、配置好了 **SpringTemplateEngine** 
- 3、配好了 **ThymeleafViewResolver** 
- 4、只需要直接开发页面





```java
//ThymeleafProperties
@ConfigurationProperties(prefix = "spring.thymeleaf")
public class ThymeleafProperties {

   private static final Charset DEFAULT_ENCODING = StandardCharsets.UTF_8;

   public static final String DEFAULT_PREFIX = "classpath:/templates/";//放到templates目录中

   public static final String DEFAULT_SUFFIX = ".html";
```



```java
@GetMapping("/thytest")
public String thyTest(Model model){
    //model中的数据会被放在请求域中，request.setAttribute()
    model.addAttribute("msg", "你好 thymeleaf");
    model.addAttribute("link", "http://www.baidu.com");
    return "success";
}
```



```html
<!DOCTYPE html>
<!--名称空间-->
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<h1 th:text="${msg}">哈哈 </h1>
<h2>
    <a href="www.github.com" th:href="${link}">百度</a><br>
    <a href="www.github.com" th:href="@{link}">百度2</a><br>
</h2>
</body>
</html>
```

**如果没有经过模板引擎的渲染，就是默认值href。经过模板引擎后，就是取出的动态值th:href**



> ${link}       取出Request域中存的link值作为地址 ，超链接到目标地址
>
> @{link}      以link为地址，而不是以Request域中存的link值作为地址   `http://localhost:8080/world/link`
>
> ​		href中加上"/"，动态配置项目名称，这样项目中的资源可以自动配置根路径



```yaml
server:
  servlet:
    context-path: /world #项目工程的路径，所有请求都要加/world/
```







### 构建后台管理系统





 

```java
@Slf4j
@Controller
public class IndexController {

    /**
     * 来登录页
     * 无法直接进入后台管理页面  在templates中无法直接访问
     * @return
     */
    @GetMapping(value = {"/", "/login"})
    public String loginPage(){
        return "login";
    }


    @PostMapping("/login")
    //表单以post方式提交到到 /login
    public String index(User user, HttpSession session, Model model){
        if(StringUtils.hasLength(user.getUserName()) && StringUtils.hasLength(user.getPassword())){
            //把登陆成功的用户保存起来
            session.setAttribute("loginUser", user);
            log.info("登陆成功: " + user.getUserName());
            //登陆成功  重定向到index.html ；防止刷新时 表单重复提交
            return "redirect:/index.html";//return "index" -> 默认是转发  每次刷新都重新提交表单
        }else{
            model.addAttribute("msg", "账号密码错误");
            log.info("账号密码错误");
            //回到登录页面
            return "login";
        }

    }

    /**
     * 经过请求处理，通过模板引擎解析，无法直接访问templates中的资源
     * 将index.html作为请求
     * 在index.html页面不断刷新，只是刷新的是/index.html这个访问请求
     * 通过模板引擎跳转到真正的index.html页面
     * @return
     */
    @GetMapping("/index.html")
    public String indexPage(HttpSession session, Model model){
        //判断是否登录   拦截器  过滤器 统一配置
        Object loginUser = session.getAttribute("loginUser");
        if(loginUser != null){
            return "index";
        }
        model.addAttribute("msg", "请重新登陆");
        return "login";
    }
}
```







```java
@Controller
public class TableController {

    @GetMapping("/basic_table")
    public String basic_table(){
        return "table/basic_table";
    }

    @GetMapping("/dynamic_table")
    public String dynamic_table(Model model){

        //表格的内容是动态遍历的
        //添加数据到model(request域)中
        List<User> users = Arrays.asList(new User("zhangsan", "123456"),
                new User("lisi", "123123"),
                new User("aaa", "123321"),
                new User("bbb", "123111"));
        model.addAttribute("users",users);
        return "table/dynamic_table";
    }

    @GetMapping("/responsive_table")
    public String responsive_table(){
        return "table/responsive_table";
    }

    @GetMapping("/editable_table")
    public String editable_table(){
        return "table/editable_table";
    }
}
```







添加错误信息提示：从请求域中获取登录时放入的错误信息：

![image-20210219174648025](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210219174648025.png)

![image-20210219174657623](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210219174657623.png)







修改header显示的用户名称：

![image-20210219175408953](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210219175408953.png)

![image-20210219175424003](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210219175424003.png)







#### 抽取公共页面



整个管理系统：左侧和上面都是一样的，中间的区域内容不同

![image-20210219192235683](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210219192235683.png)



![image-20210219192330078](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210219192330078.png)







![image-20210219194204948](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210219194204948.png)



地址使用thmeleaf，**可以动态加上项目名**



引入：

![image-20210219201051659](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210219201051659.png)



公共页面common.html

![image-20210219212509356](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210219212509356.png)



![image-20210219212532545](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210219212532545.png)





```html
<!-- Placed js at the end of the document so the pages load faster -->
<div th:replace="common :: #commonscript"></div>
```



引入几个公共部分：

![image-20210219212224347](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210219212224347.png)







----





完成提取后，要修改左侧菜单的值，只需要在公共页面修改就行



![image-20210219213536520](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210219213536520.png)







退出log out:跳转到登录页面

![image-20210219222308623](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210219222308623.png)



修改header用户名

![image-20210219222334699](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210219222334699.png)

```
[[${session.loginUser.userName}]]
```





#### 遍历数据



向model中传入数据

```java
@GetMapping("/dynamic_table")
public String dynamic_table(Model model){

    //表格的内容是动态遍历的
    List<User> users = Arrays.asList(new User("zhangsan", "123456"),
            new User("lisi", "123123"),
            new User("aaa", "123321"),
            new User("bbb", "123111"));
    model.addAttribute("users",users);
    return "table/dynamic_table";
}
```



![image-20210219230339161](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210219230339161.png)



![image-20210219230325796](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210219230325796.png)













#### tip



将需要使用模板引擎解析的html文件都加上

`lang="en" xmlns:th="http://www.thymeleaf.org"`命名空间



---



```html
th:href="@{/css/style.css}"
```

一定记得 th 的链接形式写成@{}









## 拦截器



### 完善用户管理系统的访问权限







**访问每个页面都需要登录！！**



`HandlerInterceptor`

![image-20210220182741979](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210220182741979.png)



![image-20210220183508663](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210220183508663.png)









```java
/**
 * Add Spring MVC lifecycle interceptors for pre- and post-processing of
 * controller method invocations and resource handler requests.
 * Interceptors can be registered to apply to all requests or be limited
 * to a subset of URL patterns.
 */
default void addInterceptors(InterceptorRegistry registry) {
}
```













但此时登录页的所有样式都出问题了！.....

![image-20210220184956813](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210220184956813.png)



> **拦截器不止拦截了动态请求，也拦截了所有静态资源的访问**   只显示html页面的基础文字内容

![image-20210220190046420](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210220190046420.png)



改进：放行静态资源文件夹中的资源

```java
@Configuration
public class AdminWebConfig implements WebMvcConfigurer {
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(new LoginIntercepter())
                .addPathPatterns("/**")//拦截所有请求  包括静态资源
                .excludePathPatterns("/", "/login", "/css/**", "/fonts/**", "/js/**", "/images/**");//这两个操作需要放行，都能访问到登录页面
    }
}
```





```java
    /**
     * 经过请求处理，通过模板引擎解析，无法直接访问templates中的资源
     * 将index.html作为请求
     * 在index.html页面不断刷新，只是刷新的是/index.html这个访问请求
     * 通过模板引擎跳转到真正的index.html页面
     * @return
     */
    @GetMapping("/index.html")
    public String indexPage(HttpSession session, Model model){
        //判断是否登录   拦截器  过滤器 统一配置
//        Object loginUser = session.getAttribute("loginUser");
//        if(loginUser != null){
//            return "index";
//        }
//        model.addAttribute("msg", "请重新登陆");
//        return "login";
        //可以去掉登录检查了
        return "index";
    }
```





---



此时因为是重定向，取不出放在session中的消息



```java
/**
 * 登录检查
 * 1.配置好拦截器要拦截哪些请求
 * 2.把这些配置放在容器中
 */
@Slf4j
public class LoginIntercepter implements HandlerInterceptor {

    @Override
    //目标方法执行之前
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {

        log.info("拦截的请求：" + request.getRequestURI());
        //登录检查逻辑
        HttpSession session = request.getSession();
        Object loginUser = session.getAttribute("loginUser");
        if(loginUser != null){
            return true;//放行
        }
        //跳转到登录页面   取不到消息
        request.setAttribute("msg", "请先登录！");
//        response.sendRedirect("/");
        //转发：
        request.getRequestDispatcher("/").forward(request, response);

        return false;//拦截住  未登录
    }

    @Override
    //目标方法执行完成以后
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {

    }

    @Override
    //页面渲染之后
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {

    }
}
```





> 1. 编写一个拦截器，实现HandlerInterceptor接口
> 2. 拦截器注册到容器中(实现`WebMvcConfigurer`的`addInterceptors`方法)
> 3. 指定拦截规则【如果是拦截所有，静态资源也会被拦截】







### 拦截器原理



1. 根据当前请求，找到可以处理请求的handler，以及handler的所有拦截器

自定义拦截器已经配置进了handler     **处理器执行链**

![image-20210220213924471](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210220213924471.png)





2. 先来 **顺序执行**  所有拦截器的preHandle方法

**目标方法执行之前：**

执行拦截器的preHandle方法：如果返回false(即没通过拦截器)，就直接返回，请求处理结束。**并且不会执行目标方法**

```java

if (!mappedHandler.applyPreHandle(processedRequest, response)) {
   return;
}

boolean applyPreHandle(HttpServletRequest request, HttpServletResponse response) throws Exception {
    for (int i = 0; i < this.interceptorList.size(); i++) {
        HandlerInterceptor interceptor = this.interceptorList.get(i);
        //interceptor.preHandle
        //只要任何一个拦截器执行失败，返回false
        if (!interceptor.preHandle(request, response, this.handler)) {
            triggerAfterCompletion(request, response, null);
            return false;
        }
        //记录执行过的拦截器 
        this.interceptorIndex = i;
    }
    return true;
}

void triggerAfterCompletion(HttpServletRequest request, HttpServletResponse response, @Nullable Exception ex) {
    //已经执行过的拦截器
    //倒序执行拦截器中的afterCompletion方法   -------  原理就在这
    for (int i = this.interceptorIndex; i >= 0; i--) {
        HandlerInterceptor interceptor = this.interceptorList.get(i);
        try {
            interceptor.afterCompletion(request, response, this.handler, ex);
        }
        catch (Throwable ex2) {
            logger.error("HandlerInterceptor.afterCompletion threw exception", ex2);
        }
    }
}
```



3. 如果任何一个拦截器执行失败返回false，直接跳出不执行目标方法

4. 所有拦截器都返回True，执行目标方法

5.  **倒序**执行所有拦截器的postHandle方法`mappedHandler.applyPostHandle(processedRequest, response, mv);`

   ---

   执行期间有**任何异常**，都会直接触发执行**拦截器的`aftercompletion`方法**：

![image-20210220220005744](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210220220005744.png)



6. 页面 **成功渲染完成**后，也会倒序触发afterCompletion()

```java
//DispatcherServlet#processDispatchResult 渲染(render)完页面后:
if (mappedHandler != null) {
   // Exception (if any) is already handled..
   mappedHandler.triggerAfterCompletion(request, response, null);
}
```





![image-20210220220920110](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210220220920110.png)

> 永远都会触发**已经执行过**的拦截器的afterCompletion方法





## 文件上传



### 基本操作



单文件和多文件上传

```java
<div class="form-group">
    <label for="exampleInputFile">头像</label>
    <input type="file" name="headerImg" id="exampleInputFile">
</div>

<div class="form-group">
    <label for="exampleInputFile">照片</label>
    <input type="file" name="photos" multiple>
</div>
```

 `multiple`代表多文件上传



```html
<form role="form" th:action="@{/upload}" method="post" enctype="multipart/form-data">
```



```java
/**
 * MultipartFile 自动封装上传过来的文件
 */
@PostMapping("/upload")
public String upload(@RequestParam("email") String email,
                     @RequestParam("username") String username,
                     @RequestPart("headerImg") MultipartFile headerImg,
                     @RequestPart("photos") MultipartFile[] photos){
    log.info("email:" + email);
    log.info("username:" + username);
    log.info("headerImg:" + headerImg.getOriginalFilename());
    log.info("photosNum:" + photos.length);

    return "index";
}
```



![image-20210220232002686](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210220232002686.png)





----



### 保存文件





```java
/**
 * MultipartFile 自动封装上传过来的文件
 */
@PostMapping("/upload")
public String upload(@RequestParam("email") String email,
                     @RequestParam("username") String username,
                     @RequestPart("headerImg") MultipartFile headerImg,
                     @RequestPart("photos") MultipartFile[] photos) throws IOException {
    log.info("email:" + email);
    log.info("username:" + username);
    log.info("headerImg:" + headerImg.getOriginalFilename());
    log.info("photosNum:" + photos.length);

    if(!headerImg.isEmpty()){
        //保存到文件服务器，OSS服务器
        headerImg.transferTo(new File("D:/desktop/" + headerImg.getOriginalFilename()));
    }
    if(photos.length != 0){
        for(MultipartFile photo : photos){
            if(!photo.isEmpty()){
                photo.transferTo(new File("D:/desktop/" + photo.getOriginalFilename()));
            }
        }
    }
    return "index";
}
```





### 改变文件上传大小限制





![image-20210220232752040](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210220232752040.png)



**需要标明单位：MB**

```yaml
spring:
  servlet:
    multipart:
      max-file-size: 20MB  #单个文件最大
      max-request-size: 40MB  # 所有文件总大小
```

:ok:









### 文件上传参数解析器





`MultipartAutoConfiguration`中自动配置了：`StandardServletMultipartResolver`【文件上传解析器】

```java
@Bean
@ConditionalOnMissingBean({MultipartConfigElement.class, CommonsMultipartResolver.class})
public MultipartConfigElement multipartConfigElement() {
    return this.multipartProperties.createMultipartConfig();
}

@Bean(name = {"multipartResolver"})
@ConditionalOnMissingBean({MultipartResolver.class})
public StandardServletMultipartResolver multipartResolver() {
```



属性都在属性类： `MultipartProperties`中

----



1. 请求进来使用文件上传解析器判断并封装文件上传请求 `StandardServletMultipartResolver.isMultipart()`

```java
processedRequest = checkMultipart(request);
```



**请求的内容类型是`multipart/`开头**，

![image-20210221035852972](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210221035852972.png)



解析这个请求，封装为：`StandardMultipartHttpServletRequest`类并返回

```java
return this.multipartResolver.resolveMultipart(request);
```



其中有request + 参数名称 + multipart文件

![image-20210221040527419](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210221040527419.png)





```java
//如果不等于request，就相当于在上面将request封装为了multipart类型的request，开启multipart
multipartRequestParsed = (processedRequest != request);
```

![image-20210221041919778](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210221041919778.png)



2. 参数解析器来解析请求中的文件内容封装成`MultipartFile`

   goto: `RequestMappingHandlerAdapter#invokeHandlerMethod`

   使用哪个参数解析器呢?

![image-20210221042134051](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210221042134051.png)



3. 使用所有的参数解析器来解析所有的参数

   先获取参数及其值、类型，

   `InvocableHandlerMethod#getMethodArgumentValues`

![image-20210221043107013](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210221043107013.png)



```java
if (!this.resolvers.supportsParameter(parameter))
    
public boolean supportsParameter(MethodParameter parameter) {
    return this.getArgumentResolver(parameter) != null;
}
```

​		使用所有的参数解析器来匹配这个参数 `supportsParameter()`

先从缓存中查看是否判断过了：在判断 `if (!this.resolvers.supportsParameter(parameter))`时就已经将解析器添加进缓存

执行`args[i] = this.resolvers.resolveArgument(parameter, mavContainer, request, this.dataBinderFactory);`时，直接从缓存中获取解析器

![image-20210221043340604](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210221043340604.png)



判断条件：

- 参数上使用注解 `@RequestPart("headerImg") MultipartFile headerImg,`

```java
//RequestPartMethodArgumentResolver
public boolean supportsParameter(MethodParameter parameter) {
   if (parameter.hasParameterAnnotation(RequestPart.class)) {
      return true;
   }
   else {
      if (parameter.hasParameterAnnotation(RequestParam.class)) {
         return false;
      }
      return MultipartResolutionDelegate.isMultipartArgument(parameter.nestedIfOptional());
   }
}
```

- 判断是否是：   **在不标注@RequestPart的情况下**
  - MultipartFile类？   
  - MultipartFile集合？MultipartFileCollection
  - MultipartFile数组？MultipartFileArray
  - Part类？集合？数组？

```java
public static boolean isMultipartArgument(MethodParameter parameter) {
    Class<?> paramType = parameter.getNestedParameterType();
    return MultipartFile.class == paramType || isMultipartFileCollection(parameter) || isMultipartFileArray(parameter) || Part.class == paramType || isPartCollection(parameter) || isPartArray(parameter);
}
```



![image-20210221044236465](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210221044236465.png)







---



resolveArgument：

![image-20210221044837655](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210221044837655.png)



解析参数：

![image-20210221044943394](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210221044943394.png)



先拿到文件上传请求：

![image-20210221045008246](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210221045008246.png)



是文件上传数组？(根据 name---photos)  将上传的文件都封装为MultipartFile  List    并将其转为数组[]类型

![image-20210221045053151](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210221045053151.png)





3. **将request中文件信息封装为一个Map**，并通过getFiles()取出

![image-20210221045222480](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210221045222480.png)

在解析文件上传请求的时候，就已经将他们都封装到Map中了

![image-20210221050140914](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210221050140914.png)





4. 获得了所有参数的值args，并执行目标方法：

   ![image-20210221050600481](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210221050600481.png)

```java
@Nullable
public Object invokeForRequest(NativeWebRequest request, @Nullable ModelAndViewContainer mavContainer, Object... providedArgs) throws Exception {
    Object[] args = this.getMethodArgumentValues(request, mavContainer, providedArgs);
    if (logger.isTraceEnabled()) {
        logger.trace("Arguments: " + Arrays.toString(args));
    }
		//传递目标方法的参数值args，通过反射执行方法
    return this.doInvoke(args);
}
```











----



文件复制工具类 :star2:  来实现文件流的拷贝

![image-20210221050826246](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210221050826246.png)













## 异常处理





### 错误处理



#### 默认规则



默认规则：

- springboot提供`/error`处理所有错误的映射
- 对于机器客户端，它将生成JSON响应，其中包含错误，HTTP状态和异常消息的详细信息。对于浏览器客户端，响应一个“whitelabel”错误视图，以HTML格式呈现相同的数据

浏览器：

![image-20210221154734575](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210221154734575.png)

postman（带上Cookie信息）

![image-20210221155020489](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210221155020489.png)

![image-20210221155213313](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210221155213313.png)



如果将Accept修改为text/html，也是接受白页

![image-20210221170957862](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210221170957862.png)



只是因为在浏览器中text/html的权重最大：所以先判断  执行返回白页

![image-20210221171115244](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210221171115244.png)







- **对其进行自定义，添加View解析为error**



- 要完全换默认行为，可以实现ErrorController，并注册该类型的Bean定义，或添加ErrorAttributes类型的组件以使用现有机制但替换其内容



- 放在error下的4xx，5xx页面会被**自动解析**   出现对应

![image-20210221155821825](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210221155821825.png)



打印错误信息：

![image-20210221160742607](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210221160742607.png)

![image-20210221160716081](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210221160716081.png)







### 异常处理自动配置原理



![image-20210221161803002](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210221161803002.png)



- `ErrorMvcAutoConfiguration`自动配置异常处理规则

  - 1.容器中的组件：类型：`DefaultErrorAttributes`   id: `errorAttributes`

    ​	`public class DefaultErrorAttributes implements ErrorAttributes, HandlerExceptionResolver, Ordered {`

    **定义了响应的错误页面中包含哪些数据**

  ```java
  public Map<String, Object> getErrorAttributes(WebRequest webRequest, ErrorAttributeOptions options) {
      Map<String, Object> errorAttributes = this.getErrorAttributes(webRequest, options.isIncluded(Include.STACK_TRACE));
      if (Boolean.TRUE.equals(this.includeException)) {
          options = options.including(new Include[]{Include.EXCEPTION});
      }
  
      if (!options.isIncluded(Include.EXCEPTION)) {
          errorAttributes.remove("exception");
      }
  
      if (!options.isIncluded(Include.STACK_TRACE)) {
          errorAttributes.remove("trace");
      }
  
      if (!options.isIncluded(Include.MESSAGE) && errorAttributes.get("message") != null) {
          errorAttributes.put("message", "");
      }
  
      if (!options.isIncluded(Include.BINDING_ERRORS)) {
          errorAttributes.remove("errors");
      }
  
      return errorAttributes;
  }
  ```

  ![image-20210221175927406](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210221175927406.png)

  

  - 2.容器中的组件：类型：`BasicErrorController` id: `basicErrorController`

    - ```java
      @Controller
      //(从配置文件中取出配置的error path)处理默认：/error路径的请求
      @RequestMapping({"${server.error.path:${error.path:/error}}"})
      public class BasicErrorController extends AbstractErrorController {
      ```

    - 1. 如果是**页面**  ，则响应：`new ModelAndView("error", model)`页面

      ```java
      @RequestMapping(
          produces = {"text/html"}
      )
      public ModelAndView errorHtml(HttpServletRequest request, HttpServletResponse response) {
          //....
          return modelAndView != null ? modelAndView : new ModelAndView("error", model);
      	//这里的error就是去寻找配置到容器中的id为error的View组件作为白页
      ```

    - 2. 以json数据响应出去  **错误信息键值对**

      ```java
      @RequestMapping
      public ResponseEntity<Map<String, Object>> error(HttpServletRequest request) {
      ```

  - 3.容器中还会配置一个组件 View  id是error   (**响应默认错误页**)

    ```java
    @Conditional({ErrorMvcAutoConfiguration.ErrorTemplateMissingCondition.class})
    protected static class WhitelabelErrorViewConfiguration {
        private final ErrorMvcAutoConfiguration.StaticView defaultErrorView = new ErrorMvcAutoConfiguration.StaticView();
        
        @Bean(
            name = {"error"}
        )
        @ConditionalOnMissingBean(//在容器中没有id为error的Bean时  ----- 可以自定义一个View放入容器！
            name = {"error"}
        )
        public View defaultErrorView() {
            return this.defaultErrorView;
        }
    ```

  - 4.容器中放组件：`BeanNameViewResolver`   用来解析视图   

    **请求发到/error路径 -----> `BasicErrorController` 跳转到“error”视图，`BeanNameViewResolver`   视图解析器按照返回的视图名error， 作为组件id，在容器中找到视图View对象**

    ```java
    @Bean
    @ConditionalOnMissingBean
    public BeanNameViewResolver beanNameViewResolver() {
        BeanNameViewResolver resolver = new BeanNameViewResolver();
        resolver.setOrder(2147483637);
        return resolver;
    }
    ```

![image-20210221165747062](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210221165747062.png)



如果想要返回页面，就会找error视图【`StaticView`】。（默认是一个白页  内含错误信息）





- 5.容器中的组件 `DefaultErrorViewResolver`  id：`conventionErrorViewResolver`
  - 如果发生错误，会以HTTP的状态码作为  视图页地址(viewName)  , 找到`error/`目录下的`viewName.html`
  - ![image-20210221171949089](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210221171949089.png)









### 异常处理步骤流程



1. 执行目标方法 `mv = ha.handle(processedRequest, response, mappedHandler.getHandler());`

   - 执行成功，返回ModelAndView
   - 目标方法运行期间有任何异常都会被**catch**；并且用 `dispatchException`进行封装

2. 执行时出现异常：

   doInvoke(): 

   ![image-20210221192709933](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210221192709933.png)

   而且标志当前请求结束：

   ```java
   finally {
      webRequest.requestCompleted();
   }
   ```

   catch到异常：

   ![image-20210221192857433](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210221192857433.png)



3. (即使前面执行目标方法出现异常，仍然会) **进入视图解析流程**   进行页面渲染

   ```java
   processDispatchResult(processedRequest, response, mappedHandler, mv, dispatchException);
   //传入捕获的Exception，此时mv==null
   ```



```java
//有异常？
if (exception != null) {
   if (exception instanceof ModelAndViewDefiningException) {
      logger.debug("ModelAndViewDefiningException encountered", exception);
      mv = ((ModelAndViewDefiningException) exception).getModelAndView();
   }
   else {
       //拿到handler处理器   就是目标对应的Controller中的方法basic_table()
      Object handler = (mappedHandler != null ? mappedHandler.getHandler() : null);
       //处理handler中出现的异常
      mv = processHandlerException(request, response, handler, exception);
      errorView = (mv != null);
   }
}
```

![image-20210221193344863](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210221193344863.png)





4. 处理方法异常 ----  **来处理handler中发生的异常** `DispatcherServlet#processHandlerException`

   处理完成返回一个ModelAndView：`mv = processHandlerException(request, response, handler, exception);`

   - 遍历所有的 `handlerExceptionResolvers`，看谁能处理当前异常

     `List<HandlerExceptionResolver> handlerExceptionResolvers`  **处理器异常解析器**

     ![image-20210221205947355](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210221205947355.png)

     系统默认的异常解析器：

     **DefaultErrorAttributes就是一个HandlerExceptionResolver**

![image-20210221193824854](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210221193824854.png)

​			

- `DefaultErrorAttributes`  先来处理异常。把异常信息保存到request域，并且返回null
  - ![image-20210221210429947](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210221210429947.png)

- `HandlerExceptionResolverComposite`，再次遍历其中的三个异常解析器来尝试解析异常



5. **遍历了当前默认的所有异常解析器，都无法解析这个异常，返回null...**，将这个异常**抛出去** throw ex

   **只是为了保存错误信息，为了后面/error的处理**

-----

6. 上一次请求抛出异常并且没有任何解析器能处理，**最终底层就会发送  /error请求**

   - `BasicErrorController` 会来处理 /error请求

   - 找到handler：`BasicErrorController` 的errorHtml方法

     ![image-20210221212508959](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210221212508959.png)

   - 得到一系列错误的相关信息：

     ![image-20210221212936627](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210221212936627.png)

   - ```java
     public ModelAndView errorHtml(HttpServletRequest request, HttpServletResponse response) {
         HttpStatus status = this.getStatus(request);
         Map<String, Object> model = Collections.unmodifiableMap(this.getErrorAttributes(request, this.getErrorAttributeOptions(request, MediaType.TEXT_HTML)));
         response.setStatus(status.value());
         ModelAndView modelAndView = this.resolveErrorView(request, response, status, model);
         return modelAndView != null ? modelAndView : new ModelAndView("error", model);
     }
     ```

   - 解析错误视图：遍历所有的ErrorViewResolver，看谁能够解析这个异常 **默认只有一个ErrorViewResolver（自动配置类中放置的`DefaultErrorViewResolver`  ）**

      先得到页面名称，再根据页面名称来判断是否能找到模板引擎 --> `ThymeleafTemplateAvailabilityProvider`，

     **有模板引擎就使用模板引擎来解析这个视图名称："/error/500"，自动加上themeleaf的前后缀，如果没有才使用`resolveResource()`**![image-20210221213745339](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210221213745339.png)

     若没有模板引擎：使用`resolveResource()`方法为errorViewName加上.html后缀，并返回html页面

     ![image-20210221214513851](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210221214513851.png)









----



#### 404处理流程



发送一个不存在的资源/请求，找不到对应Controller的Handler方法后，会认为要找静态资源，于是在下面四个地址中去寻找

![image-20210222050756714](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210222050756714.png)





> 中间的流程没太读懂....



在找不到资源后，同样发送一个/error请求：

![image-20210222052230505](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210222052230505.png)

然后BasicController来返回errorView      status：404

![image-20210222052325468](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210222052325468.png)









### 定制错误处理逻辑



#### 自定义错误页

- error/404.html   error/5xx.html；**有精确的错误状态码页面**就匹配精确，没有就找**4xx.html**；如果有没有就触发白页

400错误  Bad Request

![image-20210222015743592](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210222015743592.png)



将404.html修改为4xx.html

并打印状态以及错误消息：

![image-20210222020238181](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210222020238181.png)

![image-20210222020308333](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210222020308333.png)







----





#### @ControllerAdvice + @ExceptionHandler处理全局异常



```java
/**
 * 处理整个web controller的异常
 */
@Slf4j
@ControllerAdvice
public class GlobalExceptionHandler {
		//当前是异常处理器handler  标注处理哪些类型的异常？
    @ExceptionHandler({ArithmeticException.class, NullPointerException.class})
    public String handleArithException(Exception e){
        log.error("异常："+ e);
        return "login";//视图地址 ModelAndView
    }
}
```



分析：



- `processHandlerException`处理异常，遍历所有handler异常解析器

  ![image-20210222021528657](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210222021528657.png)

  0. 哪个方法标了**`@ExceptionHandler`注解**

在 `ExceptionHandlerExceptionResover`中找到标注注解的方法，用来执行处理异常的方法![image-20210222022250436](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210222022250436.png)

这个处理异常的方法被当成一个正常的方法去执行，并且获得返回值：包装的是一个Exception对象：

![image-20210222022551856](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210222022551856.png)





> 在 `ExceptionHandlerExceptionResolver#doResolveHandlerMethodException`方法中：
>
> 执行：`exceptionHandlerMethod.invokeAndHandle(webRequest, mavContainer, arguments);`，**来获得请求参数及其值**，
>
> ![image-20210222022859671](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210222022859671.png)
>
> 正常返回，封装了一个Exception对象

将返回值“login”作为viewname封装在mavContainer中：

![image-20210222023220126](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210222023220126.png)

继而获得mav对象并返回



- 在 `processHandlerException`方法中就获得了异常解析器解析得到的mav对象（没有标注@ExceptionHandler就没有返回结果，跳转到/error请求）



- 接着 `processDispatchResult`方法得到了mav对象，并且此时mav含有需要跳转的视图view。渲染这个页面：

  ```java
  // Did the handler return a view to render?
  if (mv != null && !mv.wasCleared()) {
      //渲染流程
     render(mv, request, response);
     if (errorView) {
        WebUtils.clearErrorRequestAttributes(request);
     }
  }
  ```

> 底层是  **ExceptionHandlerExceptionResolver类提供支持，专门处理@ExceptionHandler注解**





#### @ResponseStatus+自定义异常





```java
//403  自定义错误消息   这个reason就是错误消息中的message
@ResponseStatus(value = HttpStatus.FORBIDDEN, reason = "用户数量太多")
public class UserTooManyException extends RuntimeException {
    public UserTooManyException(String message){
        super(message);
    }
    public UserTooManyException(){

    }
}
```



```java
@GetMapping("/dynamic_table")
public String dynamic_table(Model model){

    //表格的内容是动态遍历的
    List<User> users = Arrays.asList(new User("zhangsan", "123456"),
            new User("lisi", "123123"),
            new User("aaa", "123321"),
            new User("bbb", "123111"));
    model.addAttribute("users",users);
    if(users.size() > 3){
        throw new UserTooManyException();
    }
    return "table/dynamic_table";
}
```



![image-20210222024728588](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210222024728588.png)



---



分析：

- catch到自定义的异常

![image-20210222034900591](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210222034900591.png)



- 解析异常：

  使用`ResponseStatusExceptionResolver`可以解析注解 `@ResponseStatus`



```java
//ResponseStatusExceptionResolver#doResolveException
//解析出注解中包含的信息并封装到status中，再将status封装进ModelAndView并返回
ResponseStatus status = AnnotatedElementUtils.findMergedAnnotation(ex.getClass(), ResponseStatus.class);
if (status != null) {
   return resolveResponseStatus(status, request, response, handler, ex);
}
```

![image-20210222035904356](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210222035904356.png)

![image-20210222040110702](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210222040110702.png)

> **将@ResponseStatus注解的信息组装成ModelAndView返回**
>
> *底层调用SendError方法：相当于给Tomcat发了一个/error请求 ↑*
>
> ***并且一旦发送了 `response.sendError()`，这次请求就结束了！***
>
> `return new ModelAndView();`返回了一个空的ModelAndView对象回去

**ModelAndView也是空的**，没有封装任何信息，因为转发/error请求已经发出去了

![image-20210222040410110](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210222040410110.png)



- 给 `rocessDispatchResult`方法返回了null   ----->   **相当于任何解析器都没能解析   其实是解析了，只不过sendError，不再返回ModelAndView对象了**



- 然后再次给服务器发/error请求：   随后转发到错误页面

  ![image-20210222041104654](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210222041104654.png)







#### Spring底层的异常，如参数类型转换异常





**记录异常： `MissingServletRequestParameterException`**

![image-20210222041507699](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210222041507699.png)



- `DefaultExceptionHandlerResolver` **来处理框架底层的异常（spring框架定义的异常）** ，判断异常的类型：

  ![image-20210222041909260](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210222041909260.png)

![image-20210222042245641](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210222042245641.png)

```java
protected ModelAndView handleMissingServletRequestParameter(MissingServletRequestParameterException ex,
      HttpServletRequest request, HttpServletResponse response, @Nullable Object handler) throws IOException {
	//又执行sendError操作    int SC_BAD_REQUEST = 400;   并保存异常的信息
   response.sendError(HttpServletResponse.SC_BAD_REQUEST, ex.getMessage());
   return new ModelAndView();
}
```



**此次请求立即结束，Tomcat服务器再发出一个/error请求**

> **若/error请求未得到响应，就返回最原始的响应页面：**
>
> ![image-20210222042354695](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210222042354695.png)
>
> 
>
> 而现在SpringMVC底层有 `BasicErrorController`来处理/error请求







#### 自定义实现HandlerExceptionResolver



```java
@Component
public class CustomerHandlerExceptionResolver implements HandlerExceptionResolver {
    @Override
    public ModelAndView resolveException(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) {
        try {
            response.sendError(666,"搞一个错误~");
        } catch (IOException e) {
            e.printStackTrace();
        }
        return new ModelAndView();
        //return new ModelAndView("login").addObject("msg", "###有一个错误###");
    }
}
```







可以看到多出了一个自定义的HandlerExceptionResolver

![image-20210222044301472](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210222044301472.png)



**但我们这个异常解析器现在不能生效！！因为在这之前DefaultHandlerExceptionResolver就已经解析完成**



想要让自定义的异常解析器生效：指定顺序



```java
@Order(value = Ordered.HIGHEST_PRECEDENCE)//数组越小，优先级越高
@Component
public class CustomerHandlerExceptionResolver implements HandlerExceptionResolver {
```

顺序调到了第一位：

![image-20210222044822754](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210222044822754.png)

> 并且因为方法resolveException返回了ModelAndView对象了，所以轮不到其他的异常解析器来解析，**直接中断遍历并返回了**



:+1:古德古德

![image-20210222044722718](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210222044722718.png)







---



当然也可以指定一个返回页面：

```java
@Order(value = Ordered.HIGHEST_PRECEDENCE)//数组越小，优先级越高
@Component
public class CustomerHandlerExceptionResolver implements HandlerExceptionResolver {
    @Override
    public ModelAndView resolveException(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) {
        return new ModelAndView("login").addObject("msg", "###有一个错误###");
    }
}
```



![image-20210222045049131](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210222045049131.png)



-----

问题来了：

**所有的异常**现在都会显示：

![image-20210222045924620](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210222045924620.png)

。。。。。。。。

**自定义异常处理器优先级太高了。。**，可以作为默认的全局异常处理规则





#### ErrorViewResolver



实现自定义解析异常视图？



- response.sendError()  /error请求就会转给Controller
- 异常没有任何解析器能处理，tomcat底层也会转给Controller

Controller最终要去到`ErrorView`，是由`ErrorViewResolver`解析的  --->  error/4xx.html

- 自动配置了 `DefaultErrorViewResolver`









## Web原生组件注入(Servlet,Filter,Listener)



SpringMVC中是把这些组件配置在web.xml中

`<servlet>  <listener> <filter>`





### 使用Servlet API



```java
//指定原生Servlet组件都放在哪里
@ServletComponentScan(basePackages = "com.example.bootwebadmin")//spring提供的，扫描Servlet

@SpringBootApplication
public class BootWebAdminApplication {

    public static void main(String[] args) {
        SpringApplication.run(BootWebAdminApplication.class, args);
    }

}
```



```java
@WebServlet(urlPatterns = "/my")
public class MyServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        resp.getWriter().write("666666");
    }
}
```

**效果：直接响应，没有经过Spring的拦截器**



一定要使用 `@ServletComponentScan(basePackages = "com.example.bootwebadmin")`来扫描组件







----





Filter

```java
@Slf4j
@WebFilter(urlPatterns = {"/css/*","/images/*"})
public class MyFilter implements Filter {
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
        log.info("MyFilter初始化完成");
    }

    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        log.info("MyFilter工作");
        filterChain.doFilter(servletRequest, servletResponse);//放行
    }

    @Override
    public void destroy() {
        log.info("MyFilter销毁");

    }
}
```



![image-20210222153442834](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210222153442834.png)





---





Listener



```java
@Slf4j
@WebListener
public class MyServletContextListener implements ServletContextListener {
    @Override
    public void contextInitialized(ServletContextEvent sce) {
        log.info("MyServletContextListener监听到项目初始化完成");
    }

    @Override
    public void contextDestroyed(ServletContextEvent sce) {
        log.info("MyServletContextListener监听到项目销毁");
    }
}
```



![image-20210222153709260](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210222153709260.png)









### RegistrationBean



![image-20210222155241314](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210222155241314.png)







```java
@Configuration
public class MyRefistConfig {
    @Bean
    public ServletRegistrationBean myServlet(){
        MyServlet myServlet = new MyServlet();
        return new ServletRegistrationBean(myServlet, "/myser", "/my");
    }

    @Bean
    public FilterRegistrationBean myFilter(){
       // return new FilterRegistrationBean(new MyFilter(), myServlet());//专门拦截上面myServlet方法的路径
        FilterRegistrationBean filterRegistrationBean = new FilterRegistrationBean(new MyFilter());
        filterRegistrationBean.setUrlPatterns(Arrays.asList("/css/*", "/images/*"));
        return filterRegistrationBean;
    }

    @Bean
    public ServletListenerRegistrationBean myListener(){
        MyServletContextListener myServletContextListener = new MyServletContextListener();
        return new ServletListenerRegistrationBean(myServletContextListener);
    }
}
```





----

### 一些问题



1. 

```java
@Configuration/*(proxyBeanMethods = false)*/
//当前类每次调用类中的方法，都会新建一次
//但这里面存在依赖关系的组件，互相调用  创建太多内存   保证是单实例的
```





2. 为什么自己写的myServlet**不会经过spring的拦截器**



一共有两个Servlet：

- /my
- DispatcherServlet    ---   /路径

先解释：↓



#### DispatcherServlet如何注册进来：



`DispatcherServletAutoConfiguration`

- 给容器中放了一个组件：`dispatcherServlet`，属性绑定到 `WebMvcProperties`，`prefix = "spring.mvc"`，在配置文件中修改对应属性



- 给容器中放了 `DispatcherServletRegistrationBean`   也是**继承自：`extends ServletRegistrationBean<DispatcherServlet>`**

  通过`ServletRegistrationBean<DispatcherServlet>`**把DispatcherServlet配置进来**



```java
@Bean(
    name = {"dispatcherServletRegistration"}
)
@ConditionalOnBean(
    value = {DispatcherServlet.class},
    name = {"dispatcherServlet"}
)
public DispatcherServletRegistrationBean dispatcherServletRegistration(DispatcherServlet dispatcherServlet, WebMvcProperties webMvcProperties, ObjectProvider<MultipartConfigElement> multipartConfig) {
    
    DispatcherServletRegistrationBean registration = new DispatcherServletRegistrationBean(dispatcherServlet, webMvcProperties.getServlet().getPath()); //像我们自定义的一样，配置路径，这里从配置文件默认值中获得：String path = "/";
    registration.setName("dispatcherServlet");
    registration.setLoadOnStartup(webMvcProperties.getServlet().getLoadOnStartup());
    multipartConfig.ifAvailable(registration::setMultipartConfig);
    return registration;
}
```



```yaml
mvc:
  servlet:
    path: /  #默认/路径   修改后DispatcherServlet所有请求都是以/xxx/开始的
```





- Tomcat-Servlet

  如果多个Servlet都能处理到同一层路径，**精确优先原则**

  A:  /my/      ----- /my/2    √

  B:  /my/1    ----- /my/1    √



当我们发送/my路径时，根据精确优先原则，来到MyServlet进行处理，**不会来到DispatcherServlet**



![image-20210222173221690](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210222173221690.png)









## 嵌入式Servlet容器



创建springboot应用时，无需外置部署服务器，应用里面**内置了服务器**



### 切换嵌入式Servlet容器



#### 原理



- 默认支持的webServer
  - Tomcat，Jetty，Undertow
  - ServletWebServerApplicationContext  容器启动寻找`ServletWebServerFactory`并引导创建服务器
- 切换服务器



- 原理：

  - SpringBoot应用启动发现当前是Web应用。Web场景包-导入Tomcat

  - web应用创建web版的**IOC容器**：`ServletWebServerApplicationContext`，启动的时候寻找 `ServletWebServerFactory`(servlet的web服务器工厂 --->  **生产Servletweb服务器**)，执行 `createWebServer`

  - SpringBoot底层默认有很多的WebServer工厂：`TomcatServletWebServerFactory`, `JettyServletWebServerFactory`, or `UndertowServletWebServerFactory`

  - 底层直接会有一个**自动配置类**：**`ServletWebServerFactoryAutoConfiguration`**

  - 自动配置类`ServletWebServerFactoryAutoConfiguration` 导入了：`ServletWebServerFactoryAutoConfiguration`(配置类)

    ![image-20210222180221221](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210222180221221.png)

  - 如果当前项目导入了Tomcat依赖，就放一个TomcatServletWebServerFactory。 **根据动态判断系统中到底导入了哪个web服务器的包(默认是导入web-starter导入Tomcat包)，容器中就有Tomcatweb服务器工厂**

  - TomcatServletWebServerFactory创建出tomcat服务器并启动



```java
private void createWebServer() {
    WebServer webServer = this.webServer;
    ServletContext servletContext = this.getServletContext();
    if (webServer == null && servletContext == null) {
        StartupStep createWebServer = this.getApplicationStartup().start("spring.boot.webserver.create");
        //在容器中寻找这个组件   0个/ >1个  都会抛出异常
        //return (ServletWebServerFactory)this.getBeanFactory().getBean(beanNames[0], ServletWebServerFactory.class);
        //拿到web服务器工厂
        ServletWebServerFactory factory = this.getWebServerFactory();
        createWebServer.tag("factory", factory.getClass().toString());
        //拿到web服务器
        //org.springframework.boot.web.embedded.tomcat.TomcatServletWebServerFactory#getWebServer
        //以前 双击执行要做的工作，用代码实现了  
        this.webServer = factory.getWebServer(new ServletContextInitializer[]{this.getSelfInitializer()});
        createWebServer.end();
        this.getBeanFactory().registerSingleton("webServerGracefulShutdown", new WebServerGracefulShutdownLifecycle(this.webServer));
        this.getBeanFactory().registerSingleton("webServerStartStop", new WebServerStartStopLifecycle(this, this.webServer));
    } else if (servletContext != null) {
        try {
            this.getSelfInitializer().onStartup(servletContext);
        } catch (ServletException var5) {
            throw new ApplicationContextException("Cannot initialize servlet context", var5);
        }
    }

    this.initPropertySources();
}
```

![image-20210222182832043](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210222182832043.png)



**内嵌服务器，就是手动把启动服务器的代码调用(Tomcat的核心jar包存在)**



![image-20210222185839919](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210222185839919.png)



![image-20210222190932509](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210222190932509.png)

`TomcatWebServer`的构造器执行初始化方法initialize()，------  在初始化方法中执行start() `this.tomcat.start();`





#### 切换服务器



![image-20210222191310199](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210222191310199.png)



```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
导入了：
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-tomcat</artifactId>
  <version>2.4.3</version>
  <scope>compile</scope>
</dependency>
```





```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
    <exclusions>
        <exclusion>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-tomcat</artifactId>
        </exclusion>
    </exclusions>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-undertow</artifactId>
</dependency>
```



这时启动的就是Undertow服务器

![image-20210222191913793](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210222191913793.png)









### 定制Servlet容器







1. 配置文件修改

配置服务器属性：

![image-20210222215426855](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210222215426855.png)



修改配置文件server.xxx  

![image-20210222215447235](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210222215447235.png)





----





2. 直接自定义：`ConfigurableServletWebServerFactory`

并且通过set方法给服务器配置属性

![image-20210222220016495](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210222220016495.png)



![image-20210222220037170](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210222220037170.png)



----





3. 实现 `WebServerFactoryCustomizer<ConfigurableServletWebServerFactory>`

   ```java
   public class ServletWebServerFactoryCustomizer implements WebServerFactoryCustomizer<ConfigurableServletWebServerFactory>, Ordered {
   ```

   ServletWebServerFactoryCustomizer  `：Servletweb服务器工厂定制化器



**把工厂的属性和配置文件的值绑定**

**传入`ConfigurableServletWebServerFactory`作为参数，把配置文件中的参数值配置到工厂中，生成一个服务器**

![image-20210222220259858](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210222220259858.png)





> **xxxxCustomizer：定制化器，可以改变xxx的默认规则**



```java
@Component
public class CustomizationBean implements WebServerFactoryCustomizer<ConfigurableServletWebServerFactory> {

    @Override
    public void customize(ConfigurableServletWebServerFactory server) {
        //在定制化器中修改属性值
        server.setPort(9000);
    }

}
```





## 定制化原理







### 定制化的常见方式



-  配合编写自定义的配置类 xxxConfiguration，使用@Bean  替换、增加容器中默认组件；视图解析器  

- 修改配置文件

- xxxxCustomizer  定制化器

- web应用，**编写配置类实现`WebMvcConfigurer`接口**，**并实现WebMvcConfigurer的方法**，即可定制化web功能， + @Bean给容器中再扩展一些组件

  - ```java
    @Configuration
    public class AdminWebConfig implements WebMvcConfigurer {
        @Override
        public void addInterceptors(InterceptorRegistry registry) {
    ```

- @EnableWebMvc + WebMvcConfigurer配置类  + @Bean可以**全面接管SpringMvc**，所有规则**全部自己重新配置**，实现定制和扩展功能

  **SpringMVCAutoConfiguration中的自动配置都不生效**:arrow_down:

  

![image-20210223011525596](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210223011525596.png)



例子：

```java
@EnableWebMvc //全面接管？ 静态资源，视图解析器，欢迎页  这些自动配置全部失效
@Configuration
public class AdminWebConfig implements WebMvcConfigurer {
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(new LoginIntercepter())
                .addPathPatterns("/**")//拦截所有请求  包括静态资源
                .excludePathPatterns("/", "/login", "/css/**", "/fonts/**", "/js/**", "/images/**","/aa/**");//这两个操作需要放行，都能访问到登录页面
    }

    //定义静态资源行为
    @Override
    public void addResourceHandlers(ResourceHandlerRegistry registry) {
        //访问aa路径下的所有请求，都去/classpath:/static/下面进行匹配
        registry.addResourceHandler("/aa/**").addResourceLocations("classpath:/static/");
    }
}
```



可以配置的一些组件

![image-20210223013127884](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210223013127884.png)



---

### @EnableWebMvc原理

1. WebMvcAutoConfiguration 默认的SpringMvc的自动配置功能类，静态资源、欢迎页、内容协商策略...

2. 一旦使用@EnableWebMvc，会**导入：`@Import(DelegatingWebMvcConfiguration.class)`**--->  致使`WebMvcAutoConfiguration `失效

3. **DelegatingWebMvcConfiguration**作用：

   - **把容器中配置的所有WebMvcConfigurer拿过来**，所有功能的定制都是这些Configurer一起生效(可能有多个类都实现了WebMvcConfigurer)
   - **把这些功能合起来，一起生效**
   - 但这里面也**有一些实现了的方法**   **非常底层的组件**，如：`requestMappingHandlerMapping`实现最基本的请求映射功能，`mvcContentNegotiationManager`，这些组件依赖的组件，也都是从容器中获取的

   ```java
   //注入WebMvcConfigurer
   @Autowired(required = false)
   public void setConfigurers(List<WebMvcConfigurer> configurers) {
      if (!CollectionUtils.isEmpty(configurers)) {
         this.configurers.addWebMvcConfigurers(configurers);
      }
   }
   ```

4. **只保证了SpringMVC最基本的使用**

5.   `WebMvcAutoConfiguration `里面的配置要**能生效**： **`@ConditionalOnMissingBean({WebMvcConfigurationSupport.class})`**

   ```java
   public class DelegatingWebMvcConfiguration extends WebMvcConfigurationSupport {
   ```

6. `@EnableWebMvc` 导致了`WebMvcAutoConfiguration `失效

















### 原理分析





**场景starter**  ->   xxxx**AutoConfiguration**   ->    导入xxx**组件**   ->   绑定xxx**Properties**配置类   ->   **绑定配置文件项**









# 数据访问





## SQL



### 数据源的自动配置-HikariDataSource

![image-20210223035534508](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210223035534508.png)

![image-20210223035540148](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210223035540148.png)



`springboot-starter-data-xxx`



#### 1.导入jdbc场景

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-jdbc</artifactId>
    <version>2.4.2</version>
</dependency>
```







![image-20210223040411584](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210223040411584.png)

还需要一个数据库驱动？

为什么导入jdbc场景，官方不导入驱动？**官方不知道我们要操作什么数据库**

对mysql做了版本仲裁：  

![image-20210223040654338](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210223040654338.png)



```xml
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
</dependency>
```



数据库版本要和驱动版本对应：

![image-20210223040858476](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210223040858476.png)



> 想要修改版本：
>
> 1、依赖就近原则，直接引入具体版本的依赖
>
> 2、直接修改properties属性，maven属性的就近优先原则
>
> ![image-20210223041204181](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210223041204181.png)







#### 2.分析自动配置



jdbc的自动配置包：

![image-20210223041429979](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210223041429979.png)



自动配置的类：

- `DataSourceAutoConfiguration`：**数据源**的自动配置

  - 修改数据源相关配置：`@EnableConfigurationProperties(DataSourceProperties.class)`绑定配置文件，`prefix = "spring.datasource"`

  - 数据库连接池的配置，在容器中没有DataSource才配置的，并且根据容器中是否有对应的数据源类，来配置数据源

    ```java
    @Configuration(proxyBeanMethods = false)
    @Conditional(PooledDataSourceCondition.class)
    @ConditionalOnMissingBean({ DataSource.class, XADataSource.class })
    @Import({ DataSourceConfiguration.Hikari.class, DataSourceConfiguration.Tomcat.class,
          DataSourceConfiguration.Dbcp2.class, DataSourceConfiguration.OracleUcp.class,
          DataSourceConfiguration.Generic.class, DataSourceJmxConfiguration.class })
    protected static class PooledDataSourceConfiguration {
    ```

  - 通过制定数据源的类型：`spring.datasource.type`来判断使用哪一个连接池

  - 底层配置好的连接池是：`HikariDataSource`

- `DataSourceTransactionManagerAutoConfiguration`：**事务**管理器的自动配置

- `JdbcTemplateAutoConfiguration：JDBCTemplate`的自动配置，可以来对数据库进行crud

  - 可以修改 `(prefix = "spring.jdbc")`配置项，来修改JdbcTemplate的配置项
  - @Bean向容器中添加了JdbcTemplate组件

- `JndiDataSourceAutoConfiguration：jndi`的自动配置

- `XADataSourceAutoConfiguration`：分布式事务相关的自动配置





#### 3.修改配置项



```yaml
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/test?useSSL=false&serverTimezone=GMT%2B8
    username: root
    password: 123456
    driver-class-name: com.mysql.cj.jdbc.Driver
#    type: com.zaxxer.hikari.HikariDataSource
```



测试：

```java
@Autowired
JdbcTemplate jdbcTemplate;

@Test
void contextLoads() {
    Long aLong = jdbcTemplate.queryForObject("select count(*) from sys_role", Long.class);
    log.info("查询到的数量：" + aLong);
}
```











### 使用Druid数据源



> https://github.com/alibaba/druid/wiki/%E5%B8%B8%E8%A7%81%E9%97%AE%E9%A2%98





整合第三方技术：

- 自定义

- **找starter**





#### 自定义方式

引入依赖：

```xml
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid</artifactId>
    <version>1.1.10</version>
</dependency>
```



- **给容器中通过bean标签来配置数据源，xml配置文件** 



- 配置类

```java
@Configuration
public class MyDataSourceConfig {

    @Bean
    //默认的自动配置是容器中没有数据源才会配置Hikari数据源 @ConditionalOnMissingBean(Datasource.class)
    @ConfigurationProperties(prefix = "spring.datasource")//和配置文件中的属性绑定  名字相同
    public DataSource dataSource(){
        DruidDataSource druidDataSource = new DruidDataSource();
//        druidDataSource.setUrl();
//        druidDataSource.setUsername();
//        druidDataSource.setPassword();
        return druidDataSource;
    }
}
```





监控页功能

```java
//配置druid的监控页功能
@Bean
public ServletRegistrationBean statViewServlet(){
    ServletRegistrationBean<StatViewServlet> statViewServletServletRegistrationBean =
            new ServletRegistrationBean<>(new StatViewServlet(), "/druid/*");
    return statViewServletServletRegistrationBean;
}
```

![image-20210223172409955](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210223172409955.png)



需要开启监控功能才能监控sql执行

```java
//加入监控功能
druidDataSource.setFilters("stat");
```

![image-20210223172928102](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210223172928102.png)







WebStatFilter用于采集web-jdbc关联监控的数据：

```java
//WebStatFilter 用于采集web-jdbc关联监控的数据
@Bean
public FilterRegistrationBean webStatFilter(){
    FilterRegistrationBean<WebStatFilter> webStatFilterFilterRegistrationBean =
            new FilterRegistrationBean<>(new WebStatFilter());
    webStatFilterFilterRegistrationBean
            .setUrlPatterns(Arrays.asList("/*"));
    webStatFilterFilterRegistrationBean.addInitParameter("exclusions", "*.js,*.gif,*.jpg,*.png,*.css,*.ico,/druid/*");

    return webStatFilterFilterRegistrationBean;
}
```

![image-20210223190609009](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210223190609009.png)



![image-20210223190622522](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210223190622522.png)



防火墙功能：

```java
//加入监控功能   防火墙功能
druidDataSource.setFilters("stat,wall");
```



![image-20210223191601382](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210223191601382.png)







配置监控页的访问权限

设置用户名和密码

```java
//配置druid的监控页功能
@Bean
public ServletRegistrationBean statViewServlet(){
    ServletRegistrationBean<StatViewServlet> statViewServletServletRegistrationBean =
            new ServletRegistrationBean<>(new StatViewServlet(), "/druid/*");
    statViewServletServletRegistrationBean.addInitParameter("loginUsername", "hcr");
    statViewServletServletRegistrationBean.addInitParameter("loginPassword", "123456");
    return statViewServletServletRegistrationBean;
}
```

![image-20210223192146061](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210223192146061.png)



> **需要通过配置@Bean，向容器中添加Filter/Servlet组件并配置属性**





#### 使用starter整合





```xml
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid-spring-boot-starter</artifactId>
    <version>1.1.17</version>
</dependency>
```



自动配置

![image-20210223194400367](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210223194400367.png)



**直接使用DataSource在配置文件中的值来初始化DruidDataSource**

并且要在DataSourceAutoConfiguration自动配置类之前来自动配置DruidDataSourceAutoConfigure，因为DataSourceAutoConfiguration中会配置HikariDataSource，容器中就有了DataSource类型的组件，无法再配置DruidDataSource

![image-20210223194947293](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210223194947293.png)



扩展配置项："spring.datasource.druid"



**导入类**：

- DruidSpringAopConfiguration.class，监控SpringBean，配置项：`spring.datasource.druid.aop-patterns`
- DruidStatViewServletConfiguration.class，开启druid监控页功能，`"spring.datasource.druid.stat-view-servlet.enabled", havingValue = "true"`，当 `spring.datasource.druid.stat-view-servlet.enabled=true`时才配置
- DruidWebStatFilterConfiguration.class，配置filter，`spring.datasource.druid.web-stat-filter.enabled=true`时才开启。 
- DruidFilterConfiguration.class，**所有druid自己filter的配置，开启各种功能**
  - 就是我们自定义配置的这个操作：`druidDataSource.setFilters("stat,wall");`
  - 下面这个就是配置文件中代表开启属性的属性路径值，再去详细配置每个功能的属性

![image-20210223195548874](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210223195548874.png)





```java
@Import({DruidSpringAopConfiguration.class,
    DruidStatViewServletConfiguration.class,
    DruidWebStatFilterConfiguration.class,
    DruidFilterConfiguration.class})
```



----



**基于配置文件来开启**



![image-20210223210733188](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210223210733188.png)



```yaml
spring:
  servlet:
    multipart:
      max-file-size: 20MB
      max-request-size: 40MB
  datasource:
    url: jdbc:mysql://localhost:3306/test?useSSL=false&serverTimezone=GMT%2B8
    username: root
    password: huangchenrui20
    driver-class-name: com.mysql.cj.jdbc.Driver
    druid:
    
      filters: stat,wall  #开启几个功能组件
      
      filter: #详细配置各个filter
        stat:
          slow-sql-millis: 1000  #慢sql记录
          log-slow-sql: true
          enabled: true
        wall: # 详细配置防火墙的功能
          enabled: true
          
      aop-patterns: com.example.bootwebadmin.*  #监控SpringBean 
      
      stat-view-servlet:  #配置监控页功能
        enabled: true
        login-username: hcrui
        login-password: 123
        reset-enable: false   #无法重置
#        url-pattern: /druid/* 也有默认配置

      web-stat-filter:   # 监控web和uri
        enabled: true
#        url-pattern: /*  这两个都有默认配置
#        exclusions: '*.js,*.gif,*.jpg,*.png,*.css,*.ico,/druid/*'
```









### 整合MyBatis操作



> 使用JdbcTemplate和DataSource：
>
> JdbcTemplate在执行时，从DataSource中获取连接进行操作：
>
> `Connection con = DataSourceUtils.getConnection(obtainDataSource());`
>
> **使用JdbcTemplate操作数据库**





使用MyBatis操作数据库



```xml
<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>2.1.4</version>
</dependency>
```



#### 配置模式



- 全局配置文件
- SqlSessionFactory  - 自动配置好了
  - `factory.setDataSource(dataSource);`使用容器中的数据源DruidDataSource
- SqlSession，自动配置了 `SqlSessionTemplate`，其中有`SqlSession`
- `@Import(AutoConfiguredMapperScannerRegistrar.class)`
- Mapper：只要我们写的操作MyBatis接口标注了**@Mapper注解**，就会被**自动扫描进来**



`@EnableConfigurationProperties(MybatisProperties.class)`MyBatis的配置项绑定类，绑定属性：`MYBATIS_PREFIX = "mybatis"`

修改配置文件中mybatis开始的所有属性



----



配置mybatis：

```yaml
# 配置mybatis规则
mybatis:
  config-location: classpath:mybatis/mybatis-config.xml # 全局配置文件位置
  mapper-locations: classpath:mybatis/mapper/*.xml  # sql映射文件位置
```



**Mapper接口 --->  绑定mapper映射文件 ---> 指定mapper接口和方法，以及要执行的sql语句**

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<!--    public User getUser(Long id); //实现写在xml配置文件中  -->
<mapper namespace="com.example.bootwebadmin.mapper.UserMapper">
    <select id="getUser" resultType="com.example.bootwebadmin.bean.User">
        select * from user where id=#{id}
    </select>
</mapper>
```



全局配置文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
    <!DOCTYPE configuration
            PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
            "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
<!--    开启驼峰命名策略-->
</configuration>
```



`mybatis.configuration`就相当于改mybatis全局配置文件中的值，不使用xml全局配置文件：

```yaml
# 配置mybatis规则
mybatis:
#  config-location: classpath:mybatis/mybatis-config.xml   不建议
  mapper-locations: classpath:mybatis/mapper/*.xml
  
  configuration:  # 写了这个配置属性，就不写上面的config-location  不能同时重复
  # 指定mybatis全局配置文件中的相关配置项
    map-underscore-to-camel-case: true #开启驼峰命名
```

UserMapper接口：

```java
@Mapper
public interface UserMapper {
    public User getUser(Long id); //实现写在xml配置文件中
}
```



```java
@Autowired
UserService userService;
@GetMapping("/user")
@ResponseBody
public User getById(@RequestParam("id") Long id){
    return userService.getUserById(id);
}
```





- 导入mybatis官方starter
- 编写mapper接口 **@Mapper注解，扫描**
- 编写sql映射文件并绑定mapper接口
- 在 application.yaml中指定Mapper配置文件的位置，以及指定全局配置文件的信息(**建议直接配置在mybatis.configuration下**)













#### 注解模式



```java
@Mapper
public interface CityMapper {

    @Select("select * from city where id=#{id}")
    public City getById(Long id);
}
```



```java
@Autowired
CityService cityService;
@GetMapping("/city")
@ResponseBody
public City getCityById(Long id){
    return cityService.getById(id);
}
```





混合模式：



```java
public void insert(City city);
```

> **在列名的地方不需要写引号......**，直接写进去列名就可以。。耽误了好久

**拿到自增主键的值，并放回city对象中**

```xml
<mapper namespace="com.example.bootwebadmin.mapper.CityMapper">
    <insert id="insert" useGeneratedKeys="true" keyProperty="id">
        insert into city (name,state,country) values(#{name},#{state},#{country})
    </insert>
</mapper>
```







mapper标签中的方法也可以使用注解来完成，**得到自动生成的主键，**

```java
@Insert("insert into city (name,state,country) values(#{name},#{state},#{country})")
@Options(useGeneratedKeys = true, keyProperty = "id")
public void insert(City city);
```



----

**最佳实战：**

- 进入mybatis-starter
- 配置application.yaml中，**指定mapper-location位置即可**
- 编写mapper接口，并标注**@Mapper**注解  (也可以使用@MapperSacn，mapper接口就可以不用标注@Mapper注解了,**不推荐！**)
- 简单方法直接注解方式
- 复杂方法，编写mapper.xml进行绑定映射











### 整合MyBatis-Plus完成CRUD



[MyBatis-Plus](https://github.com/baomidou/mybatis-plus)（简称 MP）是一个 [MyBatis](http://www.mybatis.org/mybatis-3/) 的增强工具，在 MyBatis 的基础上只做增强不做改变，为简化开发、提高效率而生。

[mybatis plus 官网](https://baomidou.com/)

建议安装 **MybatisX** 插件





```xml
    <dependency>
        <groupId>com.baomidou</groupId>
        <artifactId>mybatis-plus-boot-starter</artifactId>
        <version>Latest Version</version>
    </dependency>
```





自动配置：

- `MybatisPlusProperties`配置文件， 绑定的配置项：`prefix = Constants.mybatis-plus`
  - `mapperLocations`自动配置了默认值：`"classpath*:/mapper/**/*.xml"` **任意包的类路径下的所有mapper文件夹下任意路径下的所有xml都是sql映射文件**
- 自动配置`SqlSessionFactory`，从容器中自动确定组件DruidDataSource
- 自动配置好了`SqlSessionTemplate`
- 配置 `AutoConfiguredMapperScannerRegistrar`，@Mapper标注的接口也会被自动扫描。也可以直接@MapperScan批量扫描





优点：

只需要Mapper接口 **继承BaseMapper**，就可以拥有基础crud能力



![image-20210224104143639](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210224104143639.png)



```java
@Data
@ToString
public class Users {
    //所有属性都应该在数据库中

    //@TableField(exist = false) //代表这个属性在表中不存在，sql就不会去查询这个属性
    //private String username;
    
    private Long id;
    private String name;
    private int age;
    private String email;
}
```



```java
//什么都不用写
public interface UsersMapper extends BaseMapper<Users> {
}
```







```java
@Autowired
UsersMapper usersMapper;
@Test
public void testUsersMapper(){
    Users users = usersMapper.selectById(3L);
    log.info("用户信息："+users);
}
```







**默认传入类型和数据库中相同类名的表名  对应**

```java
public interface UsersMapper extends BaseMapper<Users> 
```

指定表名：

```java
@TableName("users")
public class Users {
```



Service接口和实现类：

```java
public interface UsersService extends IService<Users> {}

@Service
public class UsersServiceImpl extends ServiceImpl<UsersMapper, Users> implements UsersService {
   //自动继承了诸多方法
}
```









![image-20210224113646773](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210224113646773.png)





#### 实现分页功能





获得分页条：

![image-20210224114411969](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210224114411969.png)

 



![image-20210224115310995](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210224115310995.png)

这个应该从数据库中查

![image-20210224115425132](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210224115425132.png)











**从page中获取对象记录**

```java
/**
 * 分页记录列表
 *
 * @return 分页对象记录列表
 */
List<T> getRecords();
```



```html
<tbody>
<!--        所有数据被放在page对象里-->
        <tr class="gradeX" th:each="user,stat:${page.records}">
<!--            <td th:text="${stat.count}">#</td>-->
            <td th:text="${user.id}">Id</td>
            <td th:text="${user.name}"></td>
            <td>[[${user.age}]]</td>
            <td>[[${user.email}]]</td>
            <td>X</td>
        </tr>
        </tbody>
        </table>
            <div class="row-fluid">
                <div class="span6">
                    <div class="dataTables_info" id="dynamic-table_info">
                        当前第 [[${page.current}]] 页 总计 [[${page.pages}]] 页 共 [[${page.total}]] 条记录</div>
                </div>
                <div class="span6">
                    <div class="dataTables_paginate paging_bootstrap pagination">
                        <ul>
                            <li class="prev disabled"><a href="#">← Previous</a></li>
                            按照实际的页数，创建页数的索引，
                            <li th:class="${num == page.current}?'active':''" th:each="num:${#numbers.sequence(1,page.pages)}">   
                                给请求传入参数 (xxx=xx)
                                <a th:href="@{/dynamic_table(pn=${num})}">[[${num}]]</a></li>
                            <li class="next"><a href="#">Next → </a></li>
                        </ul>
                    </div>
                </div>
            </div>
```



向容器中添加分页拦截器

```java
@Configuration
public class MyBatisConfig {

    @Bean
    public MybatisPlusInterceptor mybatisPlusInterceptor(){
        MybatisPlusInterceptor mybatisPlusInterceptor = new MybatisPlusInterceptor();
        //这是分页拦截器
        PaginationInnerInterceptor paginationInnerInterceptor = new PaginationInnerInterceptor();
        paginationInnerInterceptor.setOverflow(true);//设置请求的最大页后操作，true调回首页
        paginationInnerInterceptor.setMaxLimit(500L);
        mybatisPlusInterceptor.addInnerInterceptor(paginationInnerInterceptor);
        return mybatisPlusInterceptor;
    }
}
```



Page类相关属性：

![](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210224210111974.png)

```java
/**
 * 分页构造函数
 *
 * @param current 当前页
 * @param size    每页显示条数
 */
public Page(long current, long size) {
    this(current, size, 0);
}
```



将分页对象放入model中，设置**需要查找的页数**和**每一页的数据条数**后，每次拿出该页的所有数据

```java
@Autowired
UsersService usersService;
@GetMapping("/dynamic_table")
public String dynamic_table(Model model,
                            @RequestParam(value = "pn", defaultValue = "1") int pn){
    //从数据库中查出user表中的用户进行展示

    List<Users> list = usersService.list();
    //model.addAttribute("users", list);
    //分页查询数据
    //当前页码  从pn这个参数来获得
    //设置当前页和每一页的数据
    Page<Users> usersPage = new Page<>(pn, 2);
    //page:分页查询结果
    Page<Users> page = usersService.page(usersPage, null);
    model.addAttribute("page", page);

    return "table/dynamic_table";
}
```









#### 删除功能



**传入参数pn，表示当前页数**

```java
<td>
    <a th:href="@{/user/delete/{id}(id=${user.id},pn=${page.current})}" class="btn btn-danger btn-sm" type="button">删除</a>
</td>
```



```java
@GetMapping("/user/delete/{id}")
public String deleteUser(@PathVariable("id") Long id,
                         @RequestParam(value = "pn", defaultValue = "1")int pn,
                         RedirectAttributes redirectAttributes){

    usersService.removeById(id);
    //重定向携带数据
    redirectAttributes.addAttribute("pn",pn);
    //在哪一页删的，还是回到当前页
    return "redirect:/dynamic_table";
}
```



removeById方法：调用的是传入Iservice的Mapper接口的deleteById方法(Mapper接口继承BaseMapper)

```java
default boolean removeById(Serializable id) {
    return SqlHelper.retBool(getBaseMapper().deleteById(id));
}
```















## NoSQL





Redis 是一个开源（BSD许可）的，内存中的**数据结构存储系统**，它可以用作**数据库、缓存和消息中间件**。 它支持多种类型的数据结构，如 [字符串（strings）](http://www.redis.cn/topics/data-types-intro.html#strings)， [散列（hashes）](http://www.redis.cn/topics/data-types-intro.html#hashes)， [列表（lists）](http://www.redis.cn/topics/data-types-intro.html#lists)， [集合（sets）](http://www.redis.cn/topics/data-types-intro.html#sets)， [有序集合（sorted sets）](http://www.redis.cn/topics/data-types-intro.html#sorted-sets) 与范围查询， [bitmaps](http://www.redis.cn/topics/data-types-intro.html#bitmaps)， [hyperloglogs](http://www.redis.cn/topics/data-types-intro.html#hyperloglogs) 和 [地理空间（geospatial）](http://www.redis.cn/commands/geoadd.html) 索引半径查询。 Redis 内置了 [复制（replication）](http://www.redis.cn/topics/replication.html)，[LUA脚本（Lua scripting）](http://www.redis.cn/commands/eval.html)， [LRU驱动事件（LRU eviction）](http://www.redis.cn/topics/lru-cache.html)，[事务（transactions）](http://www.redis.cn/topics/transactions.html) 和不同级别的 [磁盘持久化（persistence）](http://www.redis.cn/topics/persistence.html)， 并通过 [Redis哨兵（Sentinel）](http://www.redis.cn/topics/sentinel.html)和自动 [分区（Cluster）](http://www.redis.cn/topics/cluster-tutorial.html)提供高可用性（high availability）





### Redis自动配置



```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```





![image-20210224230142732](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210224230142732.png)





自动配置类`RedisAutoConfiguration`

- `RedisProperties`属性类，对redis的配置 `"spring.redis"`
- 两种配置的客户端`LettuceConnectionConfiguration`，`JedisConnectionFactory`， 连接工厂也准备好了，`xxxConnectionFactory`
- 自动注入了`RedisTemplate<Object, Object>`, xxxxTemplate：用于操作redis。k,v都是Object类型
- 自动注入`StringRedisTemplate`，**K,V存储**，且**K,V都是String类型**
  - 底层使用这两个Template就可以操作redis









### redis环境搭建





1. 阿里云redis数据库
2. 申请 公网链接地址
3. 修改白名单，默认所有地址均可访问 0.0.0.0/0







![image-20210225003023204](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210225003023204.png)

**不写用户名**

```yaml
redis:
  url: redis://Hcr161213@r-2ze2pix0xlbt35rlo6pd.redis.rds.aliyuncs.com:6379
  #或者：
  host:  
  post: 
  pasword:
```







### RedisTemplate与Lettuce









```java
@Autowired
StringRedisTemplate stringRedisTemplate;

@Test
public void testRedis(){
    ValueOperations<String, String> operations = stringRedisTemplate.opsForValue();
    //通过RedisConnectionFactory获得RedisConnection对象，调用set方法
    operations.set("hello", "world");
    System.out.println(operations.get("hello"));
}
```





















### 切换至jedis





需要导入jedis相关jar包

![image-20210225005749398](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210225005749398.png)



同时导入Lettuce和jedis

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
<dependency>
    <groupId>redis.clients</groupId>
    <artifactId>jedis</artifactId>
</dependency>
```



![image-20210225010049434](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210225010049434.png)



配置客户端类型：

```yaml
spring:
    redis:
    url: redis://Hcr161213@r-2ze2pix0xlbt35rlo6pd.redis.rds.aliyuncs.com:6379
    client-type: jedis
    lettuce:
      pool:
        max-active: 10
        min-idle: 5 
    jedis:
      pool:
        max-active: 10
```





### 实现访问统计功能



```java
@Component
public class RedisUrlCountInterceptor implements HandlerInterceptor {

    @Autowired
    StringRedisTemplate stringRedisTemplate;

    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        String uri = request.getRequestURI();
        //默认每次访问当前uri就会计数+1
        stringRedisTemplate.opsForValue().increment(uri);
        return true;
    }
}
```



>  * 同样的Interceptor 和 filter 有相同的功能，用哪个好？
>      * Filter是Servlet定义的原生组件，好处：是脱离了spring也能使用
>      * Interceptor是spring定义的接口，只能在spring中使用，可以使用spring的自动装配功能

```java
@Configuration
public class AdminWebConfig implements WebMvcConfigurer {



    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(new LoginInterceptor())
                .addPathPatterns("/**")//拦截所有请求  包括静态资源
                .excludePathPatterns("/", "/login", "/css/**", "/fonts/**", "/js/**", "/images/**","/city/**");//这两个操作需要放行，都能访问到登录页面

        //如果是new的RedisUrlCountInterceptor，自动注入的StringRedisTemplate没法使用，只有容器中的组件才能使用@Autowired
        //从容器中获取RedisUrlCountInterceptor
        registry.addInterceptor(redisUrlCountInterceptor)
                .addPathPatterns("/**")
                .excludePathPatterns("/", "/login", "/css/**", "/fonts/**", "/js/**", "/images/**");
    }

    /**
     * 同样的Interceptor 和 filter 有相同的功能，用哪个好？
     * Filter是Servlet定义的原生组件，好处：是脱离了spring也能使用
     * Interceptor是spring定义的接口，只能在spring中使用，可以使用spring的自动装配功能
     */
    @Autowired
    RedisUrlCountInterceptor redisUrlCountInterceptor;
}
```





![image-20210225020317885](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210225020317885.png)



```java
    /**
     * 经过请求处理，通过模板引擎解析，无法直接访问templates中的资源
     * 将index.html作为请求
     * 在index.html页面不断刷新，只是刷新的是/index.html这个访问请求
     * 通过模板引擎跳转到真正的index.html页面
     * @return
     */
    @GetMapping("/index.html")
    public String indexPage(HttpSession session, Model model){

        ValueOperations<String, String> operations = stringRedisTemplate.opsForValue();
        String s = operations.get("/index.html");
        String s1 = operations.get("/basic_table");

        model.addAttribute("indexCount", s);
        model.addAttribute("basicTableCount", s1);
        return "index";
    }
    @Autowired
    StringRedisTemplate stringRedisTemplate;
```



![image-20210225021142771](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210225021142771.png)





![image-20210225021126580](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210225021126580.png)













# 单元测试





## JUnit5的变化



作为最新版本的JUnit框架，JUnit5与之前版本的Junit框架有很大的不同。由三个不同子项目的几个不同模块组成。

> **JUnit 5 = JUnit Platform + JUnit Jupiter + JUnit Vintage**

- **JUnit Platform**: Junit Platform是在JVM上启动测试框架的基础，不仅支持Junit自制的测试引擎，其他测试引擎也都可以接入。 

- **JUnit Jupiter**: JUnit Jupiter提供了JUnit5的新的编程模型，是**JUnit5新特性的核心**。内部 包含了一个**测试引擎**，用于在Junit Platform上运行。  

- **JUnit Vintage**: 由于JUint已经发展多年，为了照顾老的项目，JUnit Vintage提供了**兼容J**Unit4.x,Junit3.x的测试引擎。



![image-20210225030540964](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210225030540964.png)







现在：

```java
@SpringBootTest
class BootWebAdminApplicationTests {
    @Test
    void contextLoads() {
    }
}
```



以前：

@SpringbootTest + @RunWith(SpringTest.class)

```java
//spring中的测试类
//让pringJunit负责创建Spring容器，但是需要将配置文件的名称告诉它
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration("classpath:ApplicationContext.xml")
```

junit4版本：

![image-20210225031534094](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210225031534094.png)





---



引入依赖：

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-test</artifactId>
    <scope>test</scope>
</dependency>
```



![image-20210225031217385](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210225031217385.png)



> 注意：
>
> **SpringBoot 2.4 以上版本移除了默认对 Vintage 的依赖。如果需要兼容junit4需要自行引入（不能使用junit4的功能 @Test -> org.junit.test）**
>
> **JUnit 5’s Vintage Engine Removed from** **`spring-boot-starter-test`,如果需要继续兼容junit4需要自行引入vintage**
>
> **引入vintage**
>
> ```xml
> <dependency>
>     <groupId>org.junit.vintage</groupId>
>     <artifactId>junit-vintage-engine</artifactId>
>     <scope>test</scope>
>     <exclusions>
>         <exclusion>
>             <groupId>org.hamcrest</groupId>
>             <artifactId>hamcrest-core</artifactId>
>         </exclusion>
>     </exclusions>
> </dependency>
> ```
>
> 







SpringBoot整合Junit以后：

- 编写测试方法：@Test标注(需要使用junit5版本的注解)
- junit类具有spring的功能，自动装配`@Autowired`，`@Transactional`标注测试方法，**测试完成后自动回滚**







## JUnit5常用注解



- @Test ：表示方法是测试方法。但是与JUnit4的@Test不同，他的职责非常单一，不能声明任何属性，拓展的测试将会由Jupiter提供额外测试

- @ParameterizedTest：表示方法是参数化测试

- @RepeatedTest：表示方法可重复执行 ， 进行多次测试

- @DisplayName：为测试类或者测试方法**设置展示名称**

- **@BeforeEach**：表示在**每个**单元测试之前执行

- @AfterEach：表示在**每个**单元测试之后执行

- @BeforeAll，@AfterAll。在**所有**单元测试之前/后执行，**需要设置为static静态方法**

- **@Tag :**表示单元测试类别，类似于JUnit4中的`@Categories`

- **@Disabled :**表示测试类或测试方法**不执行**，类似于JUnit4中的@Ignore

- **@Timeout :**表示测试方法运行如果超过了指定时间将会返回错误

  - `java.util.concurrent.TimeoutException: testTimeout() timed out after 1 second`

- **@ExtendWith :**为测试类或测试方法提供扩展类引用 `@ExtendWith(SpringExtension.class)`

  普通的测试类，不具备springboot的自动注入功能：@Autowired，需要@SpringbootTest

![image-20210225154345358](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210225154345358.png)







## 断言



断言（assertions）是测试方法中的核心部分，用来对测试需要满足的条件进行验证。

**这些断言方法都是 `org.junit.jupiter.api.Assertions` 的  静态  方法**。JUnit 5 内置的断言可以分成如下几个类别：

**检查业务逻辑返回的数据是否合理。**



> **所有的测试运行结束以后，会有一个详细的测试报告；**

---

简单断言



用来对单个值进行简单的验证。如：

| 方法            | 说明                                                     |
| --------------- | -------------------------------------------------------- |
| assertEquals    | 判断两个**对象**或两个**原始类型**是否相等，添加错误信息 |
| assertNotEquals | 判断两个对象或两个原始类型是否不相等                     |
| assertSame      | 判断两个对象**引用**是否指向**同一个对象**               |
| assertNotSame   | 判断两个对象引用是否指向不同的对象                       |
| assertTrue      | 判断给定的布尔值是否为 true                              |
| assertFalse     | 判断给定的布尔值是否为 false                             |
| assertNull      | 判断给定的对象引用是否为 null                            |
| assertNotNull   | 判断给定的对象引用是否不为 null                          |



```
org.opentest4j.AssertionFailedError: 
Expected :5
Actual   :6
```

**前面断言失败，后面的代码都不会执行**

---



数组断言

通过 assertArrayEquals 方法来判断两个对象或原始类型的**数组**是否相等,**判断数组的值**

```java
@Test
@DisplayName("array assertion")
public void array() {
 assertArrayEquals(new int[]{1, 2}, new int[] {1, 2});
}
```





---



组合断言



assertAll 方法接受多个 org.junit.jupiter.api.Executable 函数式接口的实例作为要验证的断言，可以通过 **lambda 表达式**很容易的提供这些断言

**所有**断言全部需要成功

```java
@Test
@DisplayName("assert all")
public void all() {
 assertAll("Math",
    () -> assertEquals(2, 1 + 1),
    () -> assertTrue(1 > 0)
 );
}
```





---

异常断言



在JUnit4时期，想要测试方法的异常情况时，需要用**@Rule**注解的ExpectedException变量还是比较麻烦的。而JUnit5提供了一种新的断言方式**Assertions.assertThrows()** ,配合函数式编程就可以进行使用。



**如果不抛出异常就报错**

```java
@Test
void testEx(){
    //断定业务逻辑一定出现异常
    Assertions.assertThrows(ArithmeticException.class, ()->{int i = 10 / 1;},
            "计算正常运行了");
}
```



> 计算正常运行了 ==> Expected java.lang.ArithmeticException to be thrown, but nothing was thrown.





---

超时断言



Junit5还提供了**Assertions.assertTimeout()** 为测试方法设置了超时时间

```java
@Test
@DisplayName("超时测试")
public void timeoutTest() {
    //如果测试方法时间超过1s将会异常
    Assertions.assertTimeout(Duration.ofMillis(1000), () -> Thread.sleep(500));
}
```



---



快速失败



通过 fail 方法直接使得测试失败

```java
@Test
@DisplayName("fail")
public void shouldFail() {
 fail("This should fail");
}
```





业务逻辑开发完成后，maven-test功能，测试所有的类，得出测试报告，具体哪里出错.......







## 前置条件



JUnit 5 中的前置条件（**assumptions【假设】**）类似于断言，不同之处在于**不满足的断言会使得测试方法失败**，

而不满足的**前置条件只会使得测试方法的执行终止**。 前置条件可以看成是测试方法执行的前提，当该前提不满足时，就没有继续执行的必要。



![image-20210225191747069](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210225191747069.png)



assumeTrue 和 assumFalse 确保给定的条件为 true 或 false，不满足条件会使得测试执行终止。assumingThat 的参数是表示条件的布尔值和对应的 Executable 接口的实现对象。只有条件满足时，Executable 对象才会被执行；当条件不满足时，测试执行并不会终止。



## 嵌套测试



JUnit 5 可以通过 Java 中的内部类和**@Nested** 注解实现嵌套测试，从而可以更好的把相关的测试方法**组织在一起**。在内部类中可以使用@BeforeEach 和@AfterEach 注解，而且嵌套的层次没有限制。



- **嵌套测试情况下，外层的test  不能驱动  内层的@BeforeEach之类的方法运行**



- 内层的test   可以驱动  外层的@Before/after等方法





```java
@DisplayName("A stack")
class TestingAStackDemo {

    Stack<Object> stack;

    @Test
    @DisplayName("is instantiated with new Stack()")
    void isInstantiatedWithNew() {
        new Stack<>();
    }

    @Nested
    @DisplayName("when new")
    class WhenNew {

        @BeforeEach
        void createNewStack() {
            stack = new Stack<>();
        }

        @Test
        @DisplayName("is empty")
        void isEmpty() {
            assertTrue(stack.isEmpty());
        }

        @Test
        @DisplayName("throws EmptyStackException when popped")
        void throwsExceptionWhenPopped() {
            assertThrows(EmptyStackException.class, stack::pop);
        }

        @Test
        @DisplayName("throws EmptyStackException when peeked")
        void throwsExceptionWhenPeeked() {
            assertThrows(EmptyStackException.class, stack::peek);
        }

        @Nested
        @DisplayName("after pushing an element")
        class AfterPushing {

            String anElement = "an element";

            @BeforeEach
            void pushAnElement() {
                stack.push(anElement);
            }

            @Test
            @DisplayName("it is no longer empty")
            void isNotEmpty() {
                assertFalse(stack.isEmpty());
            }

            @Test
            @DisplayName("returns the element when popped and is empty")
            void returnElementWhenPopped() {
                assertEquals(anElement, stack.pop());
                assertTrue(stack.isEmpty());
            }

            @Test
            @DisplayName("returns the element when peeked but remains not empty")
            void returnElementWhenPeeked() {
                assertEquals(anElement, stack.peek());
                assertFalse(stack.isEmpty());
            }
        }
    }
}
```







## 参数化测试



参数化测试是JUnit5很重要的一个新特性，它使得用**不同的参数多次运行测试**成为了可能，也为我们的单元测试带来许多便利。



利用**@ValueSource**等注解，指定入参，我们将可以使用不同的参数进行多次单元测试，而不需要每新增一个参数就新增一个单元测试，省去了很多冗余代码。



- **@ValueSource**: 为参数化测试指定**入参来源(数组形式)**，支持八大基础类以及String类型,Class类型
- **@NullSource**: 表示为参数化测试提供一个null的入参
- **@EnumSource**: 表示为参数化测试提供一个枚举入参
- **@CsvFileSource**：表示读取指定CSV文件内容作为参数化测试入参
- **@MethodSource**：表示读取指定方法的返回值作为参数化测试入参(注意方法返回需要是一个流)





> 当然如果参数化测试仅仅只能做到指定普通的入参还达不到让我觉得惊艳的地步。真正感到他的强大之处的地方在于他可以支持外部的各类入参。如:CSV,YML,JSON 文件甚至方法的返回值也可以作为入参。只需要去实现**ArgumentsProvider**接口，任何外部文件都可以作为它的入参。





```java
@ParameterizedTest
@MethodSource("method")    //指定方法名 
@DisplayName("方法来源参数")
public void testWithExplicitLocalMethodSource(String name) {
    System.out.println(name);
    Assertions.assertNotNull(name);
}

//需要是static方法
static Stream<String> method() {
    return Stream.of("apple", "banana");
}
```





## 迁移指南





在进行迁移的时候需要注意如下的变化：

- 注解在 org.junit.jupiter.api 包中，断言在 org.junit.jupiter.api.Assertions 类中，前置条件在 org.junit.jupiter.api.Assumptions 类中。
- 把@Before 和@After 替换成@BeforeEach 和@AfterEach。
- 把@BeforeClass 和@AfterClass 替换成@BeforeAll 和@AfterAll。
- 把@Ignore 替换成@Disabled。
- 把@Category 替换成@Tag。
- 把@RunWith、@Rule 和@ClassRule 替换成@ExtendWith。















# 指标监控





## SpringBoot Actuator





未来每一个微服务在云上部署以后，我们都需要对其进行监控、追踪、审计、控制等。SpringBoot就抽取了Actuator场景，使得我们每个微服务快速引用即可获得生产级别的应用监控、审计等功能。





```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```







![image-20210225224518099](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210225224518099.png)







### 如何使用





- 引入场景

- 访问 `http://localhost:8080/actuator/**`

- 暴露所有监控信息为HTTP



**都默认以jmx方式暴露，web方式只暴露health和info端点**

```yaml
management:
  endpoints:
    # 默认开启所有监控端点
    enabled-by-default: true
    web:
      exposure:
        include: '*'  # 以web方式暴露所有端点，本来只是暴露health info
```





测试：

http://localhost:8080/actuator/beans

http://localhost:8080/actuator/configprops

http://localhost:8080/actuator/metrics

http://localhost:8080/actuator/metrics/jvm.gc.pause



**[http://localhost:8080/actuator/](http://localhost:8080/actuator/metrics)endpointName/detailPath**





## 可视化

https://github.com/codecentric/spring-boot-admin



参照文档

https://codecentric.github.io/spring-boot-admin/2.3.1/#getting-started



创建一个server：







应用成功上线

![image-20210226151300765](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210226151300765.png)





使用ip注册：

```yaml
spring:
    boot:
      admin:
        client:
          url: http://localhost:8888

          instance:
            prefer-ip: true #使用ip注册进来
application:
  name: boot-web-admin
```



![image-20210226151713560](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210226151713560.png)



![image-20210226151926902](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210226151926902.png)



![image-20210226151941550](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210226151941550.png)





**监控应用底层的actuator**













## Actuator Endpoints





### 最常使用的端点



| ID                 | 描述                                                         |
| ------------------ | ------------------------------------------------------------ |
| `auditevents`      | 暴露当前应用程序的审核事件信息。需要一个`AuditEventRepository组件`。 |
| `beans`            | 显示应用程序中所有Spring Bean的完整列表。                    |
| `caches`           | 暴露可用的缓存。                                             |
| `conditions`       | 显示自动配置的所有条件信息，包括匹配或不匹配的原因。         |
| `configprops`      | 显示所有`@ConfigurationProperties`。                         |
| `env`              | 暴露Spring的属性`ConfigurableEnvironment`                    |
| `flyway`           | 显示已应用的所有Flyway数据库迁移。 需要一个或多个`Flyway`组件。 |
| `health`           | 显示应用程序运行状况信息。                                   |
| `httptrace`        | 显示HTTP跟踪信息（默认情况下，最近100个HTTP请求-响应）。需要一个`HttpTraceRepository`组件。 |
| `info`             | 显示应用程序信息。                                           |
| `integrationgraph` | 显示Spring `integrationgraph` 。需要依赖`spring-integration-core`。 |
| `loggers`          | 显示和修改应用程序中日志的配置。                             |
| `liquibase`        | 显示已应用的所有Liquibase数据库迁移。需要一个或多个`Liquibase`组件。 |
| `metrics`          | 显示当前应用程序的“指标”信息。                               |
| `mappings`         | 显示所有`@RequestMapping`路径列表。                          |
| `scheduledtasks`   | 显示应用程序中的计划任务。                                   |
| `sessions`         | 允许从Spring Session支持的会话存储中检索和删除用户会话。需要使用Spring Session的基于Servlet的Web应用程序。 |
| `shutdown`         | 使应用程序正常关闭。默认禁用。                               |
| `startup`          | 显示由`ApplicationStartup`收集的启动步骤数据。需要使用`SpringApplication`进行配置`BufferingApplicationStartup`。 |
| `threaddump`       | 执行线程转储。                                               |





如果您的应用程序是Web应用程序（Spring MVC，Spring WebFlux或Jersey），则可以使用以下附加端点：

| ID           | 描述                                                         |
| ------------ | ------------------------------------------------------------ |
| `heapdump`   | 返回`hprof`堆转储文件。                                      |
| `jolokia`    | 通过HTTP暴露JMX bean（需要引入Jolokia，不适用于WebFlux）。需要引入依赖`jolokia-core`。 |
| `logfile`    | 返回日志文件的内容（如果已设置`logging.file.name`或`logging.file.path`属性）。支持使用HTTP`Range`标头来检索部分日志文件的内容。 |
| `prometheus` | 以Prometheus服务器可以抓取的格式公开指标。需要依赖`micrometer-registry-prometheus`。 |





最常用的Endpoint

- **Health：健康状况**
- **Metrics：运行时指标**
- **Loggers：日志记录**







### Health Endpoint



健康检查端点，我们一般用于在**云平台**，平台会定时的**检查应用的健康状况**，我们就需要Health Endpoint可以为平台返回当前应用的一系列**组件健康状况**的集合。

重要的几点：

- health endpoint返回的结果，应该是**一系列健康检查后的一个汇总报告**，需要**所有的组件都健康**
- 很多的健康检查默认已经自动配置好了，比如：数据库、redis等
- 可以很容易的添加自定义的健康检查机制



> "status": "UP"   太简单了...

```yaml
endpoint: # 具体配置某一个端点 
  health:
    show-details: always #显示详细信息
  # enabled: true  单独开放指定的端点
```



![image-20210225233354648](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210225233354648.png)









### Metrics Endpoint





提供**详细的、层级的、空间指标信息**，这些信息可以被pull（主动推送）或者push（被动获取）方式得到；

- 通过Metrics对接多种**监控系统**
- 简化核心Metrics开发
- 添加自定义Metrics或者扩展已有Metrics



![image-20210225233615312](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210225233615312.png)











## 定制Endpoints



### 定制health信息





```java
@Component
//MyCon是健康信息的名称
public class MyConHealthIndicator extends AbstractHealthIndicator {

    /**
     * 真实的检查方法
     * @param builder
     * @throws Exception
     */
    @Override
    protected void doHealthCheck(Health.Builder builder) throws Exception {
        //数据库：获取连接，进行测试
        Map<String, Object> map = new HashMap<>();
        //检查完成
        if(1 == 1){
            //builder.up();
            builder.status(Status.UP);
            map.put("count", 1);
            map.put("ms", 100);
        }else{
            //builder.down();
            builder.status(Status.OUT_OF_SERVICE);
            map.put("err", "连接超时");
            map.put("ms", 3000);
        }
        //返回信息
        builder.withDetail("code", 100)
                .withDetails(map);
    }
}
```

```yaml
management:
    endpoint:
      health:
        show-details: always
```





![image-20210226040150316](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210226040150316.png)







### 定制info信息



1. 编写配置文件

```yaml
info:
  appName: boot-web-admin
  version: 1.0.1
  mavenProjectName: @project.artifactId@  # @project.xxx@获取maven文件中的标签值
  mavenProjectVersion: @project.version@
```



![image-20210226040847205](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210226040847205.png)

2. 实现InfoContributor

两种方式会一起显示

```java
@Component
public class AppInfoConttributor implements InfoContributor {
    /**
     * 有时候信息需要去调用方法来得到，这时候就不能直接写在配置文件中
     * @param builder
     */
    @Override
    public void contribute(Info.Builder builder) {
        builder.withDetail("msg", "hello")
                .withDetails(Collections.singletonMap("hello", "world"));
    }
}
```







### 定制Metrics信息





#### SpringBoot支持自动适配的Metrics

- **JVM** metrics, report utilization of:

- - Various memory and buffer pools
  - Statistics related to garbage collection
  - Threads utilization
  - Number of classes loaded/unloaded

- **CPU** metrics
- File descriptor metrics
- Kafka consumer and producer metrics
- **Log4j2** metrics: record the number of events logged to Log4j2 at each level
- Logback metrics: record the number of events logged to Logback at each level
- Uptime metrics: report a gauge for uptime and a fixed gauge representing the application’s absolute start time
- **Tomcat** metrics (`server.tomcat.mbeanregistry.enabled` must be set to `true` for all Tomcat metrics to be registered)
- [Spring Integration](https://docs.spring.io/spring-integration/docs/5.4.1/reference/html/system-management.html#micrometer-integration) metrics









#### 增加定制Metrics





```java
@Service
public class CityService {

    @Autowired
    CityMapper cityMapper;

    //从容器中拿到MeterRegistry
    Counter counter;
    public CityService(MeterRegistry meterRegistry){
        this.counter = meterRegistry.counter("CityService.getById.count");
    }

    public City getById(Long id){
        counter.increment();
        return cityMapper.getById(id);
    }

    public void saveCity(City city){
        cityMapper.insert(city);
    }
}
```





![image-20210226044402467](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210226044402467.png)













#### 定制Endpoints



场景：开发**ReadinessEndpoint**来管理程序是否就绪，或者**Liveness****Endpoint**来管理程序是否存活；



```java
@Component
@Endpoint(id = "myservice")
public class MyServiceEndPoint {

    @ReadOperation//读方法
    public Map getDockerInfo(){//是个属性，不能传参
        //端点的读操作
        return Collections.singletonMap("dockerinfo", "docker started...");
    }


    @WriteOperation
    public void stopDocker(){
        //在jconsole  jmx中执行操作
        System.out.println("docker stopped!");
    }
}
```



![image-20210226045744782](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210226045744782.png)













# 原理解析







## Profile功能



为了方便**多环境适配**，springboot简化了profile功能。







### application-profile功能



- 默认配置文件  application.yaml；任何时候都会加载
- 指定环境配置文件  application-{env}.yaml
- 激活指定环境

- - 配置文件激活
  - 命令行激活：java -jar xxx.jar --**spring.profiles.active=prod  --person.name=haha**

- - - **修改配置文件的任意值，命令行优先**

- 默认配置与环境配置同时生效
- **同名**配置项，**profile配置优先**



```yaml
person.name=zhangsan
spring.profiles.active=test # 在默认配置文件中指定激活的环境
server.port=8080
```



```yaml
person:
  name: test-张三
server:
  port: 7000
```



![image-20210226155125293](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210226155125293.png)









### @Profile条件装配功能

```java
@Configuration(proxyBeanMethods = false)
@Profile("production")//还可以标在方法上，只有在指定的环境下，指定的方法/类才生效   
public class ProductionConfiguration {

    // ...

}
```





### profile分组





```properties
spring.profiles.group.production[0]=proda
spring.profiles.group.production[1]=prodb

spring.profiles.group.mytest[0]=test

使用：--spring.profiles.active=production  # 两个环境都加载进来  其中的值都显示（若为互补）
```











## 外部化配置



1. Default **properties** (specified by setting `SpringApplication.setDefaultProperties`).
2. [`@PropertySource`](https://docs.spring.io/spring/docs/5.3.1/javadoc-api/org/springframework/context/annotation/PropertySource.html) annotations on your `@Configuration` classes. Please note that such property sources are not added to the `Environment` until the application context is being refreshed. This is too late to configure certain properties such as `logging.*` and `spring.main.*` which are read before refresh begins.
3. **Config data (such as** **`application.properties`** **files)**
4. A `RandomValuePropertySource` that has properties only in `random.*`.
5. **OS environment variables**.
6. Java System **properties** (`System.getProperties()`).
7. JNDI attributes from `java:comp/env`.
8. `ServletContext` init parameters.
9. `ServletConfig` init parameters.
10. Properties from `SPRING_APPLICATION_JSON` (inline JSON embedded in an environment variable or system property).
11. **Command line** arguments.
12. `properties` attribute on your tests. Available on [`@SpringBootTest`](https://docs.spring.io/spring-boot/docs/2.4.0/api/org/springframework/boot/test/context/SpringBootTest.html) and the [test annotations for testing a particular slice of your application](https://docs.spring.io/spring-boot/docs/current/reference/html/spring-boot-features.html#boot-features-testing-spring-boot-applications-testing-autoconfigured-tests).
13. [`@TestPropertySource`](https://docs.spring.io/spring/docs/5.3.1/javadoc-api/org/springframework/test/context/TestPropertySource.html) annotations on your tests.
14. [Devtools global settings properties](https://docs.spring.io/spring-boot/docs/current/reference/html/using-spring-boot.html#using-boot-devtools-globalsettings) in the `$HOME/.config/spring-boot` directory when devtools is active.





可以获得系统的环境变量、属性：

![image-20210226182810507](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210226182810507.png)

![image-20210226183750677](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210226183750677.png)





### 外部配置源



常用：**Java属性文件**、**YAML文件**、**环境变量**、**命令行参数**；都可以获取到



> **指定环境优先，外部优先，后面的方法可以覆盖前面的同名配置项**







### 配置文件查找位置

(1) classpath 根路径   **/java  /resources**

(2) classpath 根路径下config目录

(3) jar包当前目录

(4) jar包当前目录的config目录

(5) /config子目录的直接子目录（linux目录）



> 从前向后依次加载，**后面的方法可以覆盖前面的同名配置项**







### 配置文件加载顺序：

1. 　当前jar包内部的application.properties和application.yml
2. 　当前jar包内部的application-{profile}.properties 和 application-{profile}.yml
3. 　引用的外部jar包的application.properties和application.yml
4. 　引用的外部jar包的application-{profile}.properties 和 application-{profile}.yml



> 制定环境优先，外部优先，**后面的方法可以覆盖前面的同名配置项**











## 自定义starter





### starter启动原理



starter-pom引入 **autoconfigurer** 包



![image-20210226194142551](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210226194142551.png)











![image-20210226195238615](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210226195238615.png)







- autoconfigure包中配置使用 **META-INF/spring.factories** 中 **EnableAutoConfiguration 的值，使得项目启动加载指定的自动配置类**
- **编写自动配置类 xxxAutoConfiguration -> xxxxProperties**

- - **@Configuration**
  - **@Conditional**
  - **@EnableConfigurationProperties**
  - **@Bean**
  - ......

> **引入starter** **--- xxxAutoConfiguration --- 容器中放入组件 ---- 绑定xxxProperties ----** **配置项**









### 自定义starter

**atguigu-hello-spring-boot-starter（启动器）**

**atguigu-hello-spring-boot-starter-autoconfigure（自动配置包）**







# SpringBoot原理



Spring原理【[Spring注解](https://www.bilibili.com/video/BV1gW411W7wy?p=1)】、**SpringMVC**原理、**自动配置原理**、SpringBoot原理





## SpringBoot启动过程



- **创建SpringApplication----------------------------------------------------------------------------------**

  - 保存一些信息，

  - 判定当前应用的类型，ClassUtils->Servlet

  - bootstrappers ，初始启动引导器 List，去spring.factories文件中找org.springframework.boot.Bootstrapper类型

  - 找 `ApplicationContextInitializer`初始化器，也是 **`去spring.factories文件中找指定的组件ApplicationContextInitializer`**

    - **这里还有一个RestartScopeInitializer是springboot-devtools调用的**
    - ![image-20210226232758715](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210226232758715.png)
    - ![image-20210226232900069](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210226232900069.png)

  - 找 **`ApplicationListener`应用监听器**。也**是在spring.factories文件中找**

    - ![image-20210226233147179](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210226233147179.png)

  - ```java
    public SpringApplication(ResourceLoader resourceLoader, Class<?>... primarySources) {
       this.resourceLoader = resourceLoader;
       Assert.notNull(primarySources, "PrimarySources must not be null");
       this.primarySources = new LinkedHashSet<>(Arrays.asList(primarySources));
       this.webApplicationType = WebApplicationType.deduceFromClasspath();
        //getSpringFactoriesInstances  从spring.factories找对应类型
        //Bootstrapper
       this.bootstrappers = new ArrayList<>(getSpringFactoriesInstances(Bootstrapper.class));
        //ApplicationContextInitializer
       setInitializers((Collection) getSpringFactoriesInstances(ApplicationContextInitializer.class));
        //ApplicationListener
       setListeners((Collection) getSpringFactoriesInstances(ApplicationListener.class));
        //找第一个有main方法的作为主程序
       this.mainApplicationClass = deduceMainApplicationClass();
    }
    ```





---



- **运行SpringApplication-------------------------------------------------------------------------**

  - ```java
    return new SpringApplication(primarySources).run(args);
    ```

  - StopWatch：记录应用的启动时间

  - 创建引导上下文 `createBootstrapContext`，context环境

    - 如果在构造的时候找到了bootstrappers，遍历并执行`Bootstrappers`接口的intitialize()方法来完成对引导启动器上下文环境设置

  - 让当前应用进入headless模式 `java.awt.headless`

  - 获取所有**RunListener（运行监听器）**   [为了方便所有Listener进行事件感知]

    - 通过 `getSpringFactoriesInstances`在spring.factories文件中获取 **`SpringApplicationRunListener.class`**(**接口**)
    - ![image-20210226234437869](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210226234437869.png)
    - ![image-20210227020227483](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210227020227483.png)

  - `listeners.starting`,遍历所有的SpringApplicationRunListener，调用**starting**方法，

    - **通知事件--->相当于通知所有感兴趣系统正在启动过程的人，项目正在starting**

  - 保存命令行参数 `ApplicationArguments`

  - 准备环境 `prepareEnvironment`

    - 返回或者创建基础环境信息对象， `StandardServletEnvironment`
    - 配置环境信息对象，读取所有的配置源的配置属性值  profiles
    - 绑定环境信息
    - 监听器调用`environmentPrepared`方法，遍历每个listener，再调用各自**`environmentPrepared`**方法。**通知所有的监听器当前环境准备完成**
    - 返回环境信息

  - 打印banner

  - **`createApplicationContext`，创建IOC容器**

    - 根据当前项目类型->Servlet，创建容器
    - 当前会创建 `AnnotationConfigServletWebServerApplicationContext`

  - **准备ApplicationContext   IOC容器的基本信息**  --->  `prepareContext()`

    - 保存环境信息
    - IOC容器的后置处理流程
    - 应用初始化器 `applyInitializers(context)`。**使用前面准备好的initializers**
      - 遍历所有的`ApplicationContextInitializer`，调用 `initialize(context)`方法，来对ioc容器进行初始化的扩展
    - 遍历所有的listeners ，**`listeners.contextPrepared(context);`**  
      - `EventPublishingRunListener`
      - 通知所有的监听器context准备好了
    - 监听器调用 **`listeners.contextLoaded(context);`**，**通知IOC容器已经加载完毕了**

  - 刷新IOC容器  `refreshContext(context);`

    - ```java
      //org.springframework.context.support.AbstractApplicationContext#refresh
      // Instantiate all remaining (non-lazy-init) singletons.
      //实例化容器中的所有组件  单实例！
      finishBeanFactoryInitialization(beanFactory);
      ```

  - 容器刷新完成后 `afterRefresh`  。。。do nothing

  - 所有监听器调用started方法 **`listeners.started(context);`** **通知所有监听器已经启动了**

    - `SpringApplicationRunListener`的各种功能，都使用到了
    - ![image-20210227014427738](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210227014427738.png)

  - 调用所有的**runners `callRunners(context, applicationArguments);`**

    - 获取**容器**中的`ApplicationRunner` **接口**
    - 获取**容器**中的`CommandLineRunner` **接口**
    - 合并所有runners，并且按照@Order进行排序
    - 遍历所有的runner，调用两种runner各自的**run方法**

  - **如果以上有异常，调用listener的failed方法**

  - 调用所有监听器的running方法：**`listeners.running(context);`**  **项目正在运行**

  - running如果有问题，继续通知所有监听器的failed方法

- **结束，返回 IOC容器`ConfigurableApplicationContext`**

> 如果添加了依赖spring-boot-devtools，则会**重启**再执行
>
> 见：https://www.wencst.com/archives/1108
>
> ![image-20210227003552947](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210227003552947.png)







## Application Events and Listeners



https://docs.spring.io/spring-boot/docs/current/reference/html/spring-boot-features.html#boot-features-application-events-and-listeners

**ApplicationContextInitializer**

**ApplicationListener**

**SpringApplicationRunListener**



这三个组件，都是在**spring.factories**配置文件中找的，**getSpringFactoriesInstances()**





```java
public class MySpringApplicationRunListener implements SpringApplicationRunListener {

    private SpringApplication springApplication;
    //需要有参构造器
    public MySpringApplicationRunListener(SpringApplication application, String[] args) {
        this.springApplication = application;
    }

    @Override
    public void starting(ConfigurableBootstrapContext bootstrapContext) {
        System.out.println("MySpringApplicationRunListener ... starting...");
    }

    @Override
    public void starting() {
        System.out.println("MySpringApplicationRunListener ... starting...");

    }

    @Override
    public void environmentPrepared(ConfigurableBootstrapContext bootstrapContext, ConfigurableEnvironment environment) {
        System.out.println("MySpringApplicationRunListener ... environmentPrepared...");

    }

    @Override
    public void environmentPrepared(ConfigurableEnvironment environment) {
        System.out.println("MySpringApplicationRunListener ... environmentPrepared...");

    }



    @Override
    public void contextLoaded(ConfigurableApplicationContext context) {
        System.out.println("MySpringApplicationRunListener ... contextLoaded...");

    }

    @Override
    public void started(ConfigurableApplicationContext context) {
        System.out.println("MySpringApplicationRunListener ... started...");

    }

    @Override
    public void running(ConfigurableApplicationContext context) {
        System.out.println("MySpringApplicationRunListener ... running...");

    }

    @Override
    public void failed(ConfigurableApplicationContext context, Throwable exception) {
        System.out.println("MySpringApplicationRunListener ... failed...");

    }
}
```



```java
public class MyApplicationListener implements ApplicationListener {
    @Override
    public void onApplicationEvent(ApplicationEvent event) {
        System.out.println("MyApplicationListener... onApplicationEvent...");
    }
}
```

![image-20210227034252055](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210227034252055.png)



```java
public class MyApplicationContextInitializer implements ApplicationContextInitializer {
    @Override
    public void initialize(ConfigurableApplicationContext applicationContext) {
        System.out.println("MyApplicationContextInitializer....initialize...");
    }
}
```



**spring.factories**

```properties
# MyApplicationContextInitializer
org.springframework.context.ApplicationContextInitializer=\
  com.example.bootweb01.listener.MyApplicationContextInitializer

# MyApplicationListener
org.springframework.context.ApplicationListener=\
  com.example.bootweb01.listener.MyApplicationListener

# MySpringApplicationRunListener
org.springframework.boot.SpringApplicationRunListener=\
  com.example.bootweb01.listener.MySpringApplicationRunListener
```











## ApplicationRunner 与 CommandLineRunner



这两个组件需要从 **容器中得到**



```java
@Order(1)
//放到容器中
@Component
public class MyCommandLineRunner implements CommandLineRunner {
    @Override
    public void run(String... args) throws Exception {
        System.out.println("MyCommandLineRunner... run");
    }
}
```





```java
//应用启动做一个一次性事情，使用这两个Runner
@Order(2)//数字越大，优先级越高
@Component
public class MyApplicationRunner implements ApplicationRunner {
    @Override
    public void run(ApplicationArguments args) throws Exception {
        System.out.println("MyApplicationRunner... run...");
    }
}
```









![image-20210227034057201](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210227034057201.png)

![image-20210227034135053](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210227034135053.png)





















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











## /* /**区别



- /*是**Servlet**中的写法  表示所有请求（静态资源和/xxx请求）
- /** 是**Spring**中的写法，表示所有请求















## classpath:



在main包下的java和resources路径**中**的所有资源，直接都会放到classes路径下

![image-20210226191703998](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210226191703998.png)



(config是resources中的文件夹)

![image-20210226191805296](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210226191805296.png)













## @EnableConfigurationProperties







@EnableConfigurationProperties(xxxProperties.class)注解的作用是：使    **使用 @ConfigurationProperties 注解的类**生效。



> 如果一个配置类只配置了@ConfigurationProperties注解，而没有使用@Component，那么在IOC容器中是获取不到properties 类转化的bean，其中的数据也就无法拿到。
>
> @EnableConfigurationProperties 相当于把  **使用了配置文件中的值  的properties类**    注入到了容器中

也可以：不使用 `@EnableConfigurationProperties` 进行注册，使用 `@Component` 注册，达到相同的效果







































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











## sql语法问题



> **在列名的地方不需要写引号......**，直接写进去列名就可以。。耽误了好久

**拿到自增主键的值，并放回city对象中**

```xml
<mapper namespace="com.example.bootwebadmin.mapper.CityMapper">
    <insert id="insert" useGeneratedKeys="true" keyProperty="id">
        insert into city (name,state,country) values(#{name},#{state},#{country})
    </insert>
</mapper>
```







## mybatis-mapper配置文件错误



如果没有mapper标签，也会报错

![image-20210224035739482](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210224035739482.png)



如果还需要这个配置文件，需要保留mapper标签，否则直接删去该mapper.xml文件即可

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.example.bootwebadmin.mapper.CityMapper">
<!--    <insert id="insert" useGeneratedKeys="true" keyProperty="id">-->
<!--        insert into city (name,state,country) values(#{name},#{state},#{country})-->
<!--    </insert>-->
</mapper>
```









## Springboot整合Mybatis扫不到mapper接口



如果springboot的主程序main和mapper接口的目录不在同一个目录下，即以下情况：

不仅需要使用@ComponentScan扫描组件注解（**这里扫的是spring注解**），还需要使用@MapperScan来扫描**mybatis的注解**

![image-20210304150232193](../picture/SpringBoot%E7%AC%94%E8%AE%B0/image-20210304150232193.png)



```java
@MapperScan(basePackages = {"com.example.mapper"})
@ComponentScan(basePackages = {"com.example.*"})
```















