# Spring与Web环境集成





```xml
<dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>javax.servlet-api</artifactId>
    <version>4.0.1</version>
</dependency>
<dependency>
    <groupId>javax.servlet.jsp</groupId>
    <artifactId>javax.servlet.jsp-api</artifactId>
    <version>RELEASE</version>
</dependency>
```





```java
public class UserServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        ApplicationContext app = new ClassPathXmlApplicationContext("applicationContext.xml");//获取应用上下文
        UserService userService = app.getBean(UserService.class);
        userService.save();
    }
}
```



## ApplicationContext应用上下文获取方式



Web层通过Spring容器来获取Service，Service内部的dao通过容器注入的。



`ApplicationContext app = new ClassPathXmlApplicationContext("applicationContext.xml");`

每个业务都需要去获取service，**多次**加载配置文件，创建容器



**应用上下文对象**是通过new ClasspathXmlApplicationContext(spring配置文件) 方式获取的，但是每次从容器中获得Bean时都要编写new ClasspathXmlApplicationContext(spring配置文件) ，这样的弊端是**配置文件加载多次，应用上下文对象创建多次。**



在Web项目中，可以使用**ServletContextListener监听Web应用的启动**，我们可以在**Web应用启动**时，就加载Spring的配置文件，创建应用上下文对象ApplicationContext，在将其存储到最大的域**servletContext域**中，这样就可以**在任意位置从域中获得应用上下文ApplicationContext对象**了。









**web.xml配置文件**

```xml
    <servlet>
        <servlet-name>userServlet</servlet-name>
        <servlet-class>com.example.mvc.web.UserServlet</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>userServlet</servlet-name>
        <url-pattern>/userServlet</url-pattern>
    </servlet-mapping>
    
    
<!--    配置监听器-->
    <listener>
        <listener-class>com.example.mvc.listener.ContextLoaderListener</listener-class>
    </listener>

<!--    全局初始化参数-->
    <context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>applicationContext.xml</param-value>
    </context-param>
```



```java
public class ContextLoaderListener implements ServletContextListener {

    @Override
    public void contextInitialized(ServletContextEvent sce) {
        //写死的名字  提到配置文件
        //ApplicationContext app = new ClassPathXmlApplicationContext("applicationContext.xml");
        
        ServletContext servletContext = sce.getServletContext();
        //读取web.xml中的全局参数
        String contextConfigLocation = servletContext.getInitParameter("contextConfigLocation");
        ApplicationContext app = new ClassPathXmlApplicationContext(contextConfigLocation);

        //将Spring的应用上下文对象存储到ServletContext域中
        servletContext.setAttribute("app", app);//这个名字也是耦合死的。。每次都要使用app这个字符串

        System.out.println("Spring容器创建完毕");
    }
}
```



```java
public class WebApplicationContextUtils {
		//静态方法，直接获取ApplicationContext对象
    public static ApplicationContext getWebApplicationContext(ServletContext servletContext){
        return (ApplicationContext) servletContext.getAttribute("app");
    }
}
```



```java
public class UserServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //ApplicationContext app = new ClassPathXmlApplicationContext("applicationContext.xml");
        ServletContext servletContext = req.getServletContext();
        
        //直接从ServletContext域中拿app对象就行
        //ApplicationContext app = (ApplicationContext) servletContext.getAttribute("app");
        
        //不需要具体的字符串来获取app对象了
        ApplicationContext app = WebApplicationContextUtils.getWebApplicationContext(servletContext);

        UserService userService = app.getBean(UserService.class);
        userService.save();
    }
}
```

 



## Spring提供获取应用上下文的工具





上面的分析不用手动实现，Spring提供了一个监听器**ContextLoaderListener**就是**对上述功能的封装**，该监听器内部**加载Spring配置文件**，创建应用上下文对象，**并存储到ServletContext域中**，提供了一个客户端工具**WebApplicationContextUtils供使用者获得应用上下文对象**。



所以我们需要做的只有两件事：

①在web.xml中配置ContextLoaderListener监听器（导入spring-web坐标）



```xml
<!--    配置监听器-->
    <listener>
        <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
    </listener>

<!--    全局初始化参数-->
    <context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:applicationContext.xml</param-value>
    </context-param>
```



②使用WebApplicationContextUtils**获得应用上下文对象ApplicationContext**



```java
public class UserServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //ApplicationContext app = new ClassPathXmlApplicationContext("applicationContext.xml");
        ServletContext servletContext = req.getServletContext();
        //直接从ServletContext域中拿app对象就行
        //ApplicationContext app = (ApplicationContext) servletContext.getAttribute("app");
        //不需要具体的字符串来获取app对象了
        //ApplicationContext app = WebApplicationContextUtils.getWebApplicationContext(servletContext);
        
        ApplicationContext app = WebApplicationContextUtils.getWebApplicationContext(servletContext);
        UserService userService = app.getBean(UserService.class);
        userService.save();
    }
}
```





> Spring集成web环境步骤
>
> ​      ①配置ContextLoaderListener监听器
>
> ​      ②使用WebApplicationContextUtils获得应用上下文







# SpringMVC简介





SpringMVC 是一种基于 Java 的实现 **MVC 设计模型**的请求驱动类型的轻量级 Web 框架，属于SpringFrameWork 的后续产品，已经融合在 Spring Web Flow 中。 

SpringMVC 已经成为目前最主流的MVC框架之一，并且随着Spring3.0 的发布，全面超越 Struts2，成为最优秀的 MVC 框架。它通过一套**注解**，让一个简单的 **Java 类**成为**处理请求的控制器**，而**无须实现任何接口**。同时它还**支持 RESTful 编程风格的请求**。



业务复杂，出现很多servlet，每个servlet内部，执行的行为很多都是重复的。

接收请求参数。。。

**抽取出一个组件**

---



流程图：



Servlet的**共有行为**，框架封装（前端控制器是servlet，非常复杂  ）

SpringMVC负责。

特有行为自己来编写。

## SpringMVC入门



需求：客户端发起请求，服务器端接收请求，执行逻辑并进行视图跳转。

**开发步骤**



![image-20210201021617103](../picture/SpringMVC%E7%AC%94%E8%AE%B0/image-20210201021617103.png)





①导入SpringMVC相关坐标



![image-20210201022746200](../picture/SpringMVC%E7%AC%94%E8%AE%B0/image-20210201022746200.png)

②配置SpringMVC核心控制器DispathcerServlet



```xml
<!--    配置SpringMVC的前端控制器-->
    <servlet>
        <servlet-name>dispatcherServlet</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
<!--        springmvc这个配置文件就是控制器controller使用-->
<!--        servlet初始化参数-->
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:spring-mvc.xml</param-value>
        </init-param>
<!--        服务器启动时就去创建这个servlet，默认是第一次访问时创建-->
        <load-on-startup>1</load-on-startup>
    </servlet>

    <servlet-mapping>
        <servlet-name>dispatcherServlet</servlet-name>
<!--        每次在访问时任何请求都要走这个servlet  *.xxx资源扩展名是xxx时走这个servlet-->
        <url-pattern>/</url-pattern>
    </servlet-mapping>
```

> 这里的url-pattern  一定要设置为 /  即在每次输入url地址时都要跳转到这个servlet，**查看是否存在映射**



![image-20210201042149125](../picture/SpringMVC%E7%AC%94%E8%AE%B0/image-20210201042149125.png)







③创建Controller类和视图页面

④使用注解配置Controller类中业务方法的映射地址



```java
@Controller
public class UserController {

    @RequestMapping("/quick")
    //请求映射  访问/quick时，映射到save方法，执行对应逻辑，跳到jsp页面中
    public String save(){
        System.out.println("Controller save running...");
        return "success.jsp";
    }
}
```



⑤配置SpringMVC核心文件 spring-mvc.xml



```xml
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans 
                    http://www.springframework.org/schema/beans/spring-beans.xsd
http://www.springframework.org/schema/context
http://www.springframework.org/schema/context/spring-context.xsd
">

<!--    也可以在applicationcontext中配置-->
<!--    为controller配置组件扫描-->
    <context:component-scan base-package="com.example.mvc.controller"/>
```



⑥客户端发起请求测试





![image-20210201024931135](../picture/SpringMVC%E7%AC%94%E8%AE%B0/image-20210201024931135.png)







> **web端发来请求/quick，**SpringMVC核心控制器`DispathcerServlet`将其**映射到`save()`函数**
>
> view页面是`success.jsp`，将这个页面传给  SpringMVC核心控制器DispathcerServlet  执行分发跳转操作。

![image-20210201041055689](../picture/SpringMVC%E7%AC%94%E8%AE%B0/image-20210201041055689.png)







## SpringMVC流程图示



![image-20210201151525626](../picture/SpringMVC%E7%AC%94%E8%AE%B0/image-20210201151525626.png)







![image-20210201151622916](../picture/SpringMVC%E7%AC%94%E8%AE%B0/image-20210201151622916.png)





> SpringMVC的开发步骤 ：
>
>    ①导入SpringMVC相关坐标
>
>    ②配置SpringMVC**核心控制器DispathcerServlet**
>
>    ③创建Controller类和视图页面
>
>    ④使用注解配置Controller类中业务方法的映射地址
>
>    ⑤配置SpringMVC核心文件 spring-mvc.xml
>
>    ⑥客户端发起请求测试











# SpringMVC的组件解析



## SpringMVC的执行流程



![image-20210201153701158](../picture/SpringMVC%E7%AC%94%E8%AE%B0/image-20210201153701158.png)



①用户发送请求至前端控制器DispatcherServlet。

---

②DispatcherServlet收到**请求**调用**HandlerMapping处理器映射器**。（根据请求找资源）

③处理器映射器找到具体的处理器(可以根据**xml配置、注解**进行查找)，生成处理器对象及处理器拦截器(如果有则生成)一并返回给DispatcherServlet。

---

④DispatcherServlet调用**HandlerAdapter处理器适配器**。

⑤HandlerAdapter经过适配**调用具体的处理器(Controller，也叫后端控制器)**。

⑥Controller执行完成返回**Model And View**。

⑦HandlerAdapter将controller执行结果ModelAndView**返回给DispatcherServlet**。

---

⑧DispatcherServlet将ModelAndView传给**ViewReslover视图解析器**。

⑨ViewReslover解析后**返回具体View**。

---

⑩DispatcherServlet根据View进行渲染视图（即将模型数据填充至视图中）。DispatcherServlet**响应用户**。





## SpringMVC组件解析



1. **前端控制器：DispatcherServlet**

​    用户请求到达前端控制器，它就相当于 MVC 模式中的 C，DispatcherServlet 是整个流程控制的中心，由它调用其它组件处理用户的请求，DispatcherServlet 的存在**降低了组件之间的耦合性**。



2. **处理器映射器：HandlerMapping**

​    HandlerMapping 负责根据用户请求找到 Handler 即处理器，SpringMVC 提供了不同的**映射器**实现不同的**映射方式**，例如：**配置文件方式，实现接口方式，注解方式等**。



3. **处理器适配器：HandlerAdapter**

​    通过 HandlerAdapter 对处理器进行执行，这是***适配器模式的应用***，通过扩展适配器可以对更多类型的处理器进行执行。



4. **处理器：Handler**

​    它就是我们**开发中要编写的具体业务控制器**。由 DispatcherServlet 把**用户请求**转发到 Handler。由Handler 对具体的用户**请求进行处理**。



5. **视图解析器：View Resolver**

​    View Resolver 负责将处理结果生成 View 视图，View Resolver 首先根据逻辑视图名解析成物理视图名，即具体的页面地址，再生成 View 视图象，最后对 View 进行渲染将处理结果**通过页面展示给用户**。



6. **视图：View**

​    SpringMVC 框架提供了很多的 View 视图类型的支持，包括：jstlView、freemarkerView、pdfView等。**最常用的视图就是 jsp**。一般情况下需要通过页面标签或页面模版技术**将模型数据通过页面展示给用户**，需要由程序员根据业务需求开发具体的页面





## SpringMVC注解解析



`@RequestMapping`

作用：用于**建立请求 URL 和处理请求方法之间的对应关系**

- 位置：

  ​      类上，请求URL 的第一级访问目录。此处**不写的话，就相当于应用的根目录**

  ​      方法上，请求 URL 的第二级访问目录，与类上的使用@ReqquestMapping标注的一级目录一起组成访问虚拟路径

- 属性：

  ​      value：用于指定请求的URL。它和path属性的作用是一样的

  ​      method：用于指定请求的方式

  ​      params：用于指定限制请求参数的条件。它支持简单的表达式。**要求请求参数的key和value必须和配置的一模一样**

例如：

​      **params = {"accountName"}，表示请求参数必须有accountName**

​      **params = {"moeny!100"}，表示请求参数中money不能是100**







---

![image-20210201190424723](../picture/SpringMVC%E7%AC%94%E8%AE%B0/image-20210201190424723.png)



```java
@Controller
@RequestMapping("/userCon")
public class UserController {

    @RequestMapping("/quick")
    //请求映射  访问/quick时，映射到save方法，执行对应逻辑，跳到jsp页面中
    public String save(){
        System.out.println("Controller save running...");
        //这时候如果只写"success.jsp" 服务器会默认为是相对地址
        // quick资源所在地址是userCon
        // 在当前路径下去跳转到success.jsp:   /userCon/success.jsp
        //要加上"/"，来表示从工程名目录开始跳转页面
        return "/success.jsp";
    }
}
```





---



![image-20210201191141353](../picture/SpringMVC%E7%AC%94%E8%AE%B0/image-20210201191141353.png)

```java
@RequestMapping(value = "/quick", method = RequestMethod.POST)
```





![image-20210201191355315](../picture/SpringMVC%E7%AC%94%E8%AE%B0/image-20210201191355315.png)



```java
@RequestMapping(value = "/quick", method = RequestMethod.GET, params = {"username"})
```



![image-20210201191421202](../picture/SpringMVC%E7%AC%94%E8%AE%B0/image-20210201191421202.png)







---



mvc命名空间引入

> **spring和SpringMVC各自扫描各自层，各司其职**



指定扫哪个注解**(注解类型)**或者不扫哪个注解

```xml
<context:component-scan base-package="com.example.mvc.controller">
    <context:include-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
</context:component-scan>
```

![image-20210201191927233](../picture/SpringMVC%E7%AC%94%E8%AE%B0/image-20210201191927233.png)





![image-20210201192318068](../picture/SpringMVC%E7%AC%94%E8%AE%B0/image-20210201192318068.png)



---

指定前缀后缀



**转发行为，默认前缀后缀**

![image-20210201192511480](../picture/SpringMVC%E7%AC%94%E8%AE%B0/image-20210201192511480.png)



```java
return "forward:/success.jsp";
```

---

**修改为重定向**

![image-20210201192725704](../picture/SpringMVC%E7%AC%94%E8%AE%B0/image-20210201192725704.png)

```java
return "redirect:/success.jsp";
```

---

有 **set方法**

![image-20210201210010926](../picture/SpringMVC%E7%AC%94%E8%AE%B0/image-20210201210010926.png)



在配置文件中**复写视图解析器**



资源路径都要加一个**/jsp**  扩展名也需要写**.jsp**

```java
return "/jsp/success.jsp";
```

通过配置 `InternalResourceViewResolver`的 `prefix`和 `suffix`参数

```xml
<!--    也可以在applicationcontext中配置-->
<!--    为controller配置组件扫描-->
    <context:component-scan base-package="com.example.mvc.controller"/>

<!--    手动配置内部资源视图解析器-->
    <bean id="viewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
<!--        设置属性  /jsp/ +  success  + .jsp-->
        <property name="prefix" value="/jsp/"/>
        <property name="suffix" value=".jsp"/>
    </bean>
```

```java
return "success";
```





# SpringMVC的数据相应





数据响应方式：

1)	**页面跳转**

​	直接返回字符串  `return "seccess.jsp"`

​	通过**ModelAndView对象**返回

2） **回写数据** 

​	直接返回字符串

​	返回对象或集合    



## 页面跳转

---

直接返回字符串：此种方式会将返回的字符串与**视图解析器**的前后缀**拼接**后跳转

![image-20210201225159242](../picture/SpringMVC%E7%AC%94%E8%AE%B0/image-20210201225159242.png)



---



返回ModelAndView类的对象



![image-20210201225834448](../picture/SpringMVC%E7%AC%94%E8%AE%B0/image-20210201225834448.png)





```java
@Controller
public class UserController {
    //这些方法都是SpringMVC来调用

    @RequestMapping("/quick5")
    //SpringMVC在调用的时候注入
    //httpServletRequest是原生的javaweb产生的对象，model是框架封装的对象
    public String save5(HttpServletRequest httpServletRequest){
        httpServletRequest.setAttribute("username", "芜湖！");
        return "success";
    }

    @RequestMapping("/quick4")
    public String save4(Model model){
        //来设置模型数据
        model.addAttribute("username", "crhuang");
        //字符串代表视图
        return "success";
    }

    @RequestMapping("/quick3")
    public ModelAndView save3(ModelAndView modelAndView){
        //SpringMVC对方法的参数可以进行自动注入，提供一个ModelAndView对象
        modelAndView.addObject("username", "hcr");
        modelAndView.setViewName("success");

        return modelAndView;
    }

    @RequestMapping("/quick2")
    public ModelAndView save2(){
        /*
        Model:模型  封装数据
        View：视图  展示数据
         */
        ModelAndView modelAndView = new ModelAndView();
        //设置视图名称
        modelAndView.setViewName("success");
        //设置模型数据  在页面中取到相应数据
        modelAndView.addObject("username", "hcr");

        return modelAndView;
    }



    @RequestMapping(value = "/quick", method = RequestMethod.GET, params = {"username"})
    //请求映射  访问/quick时，映射到save方法，执行对应逻辑，跳到jsp页面中
    public String save(){
        System.out.println("Controller save running...");
        //这时候如果只写"success.jsp" 服务器会默认为是相对地址
        // quick资源所在地址是userCon
        // 在当前路径下去跳转到success.jsp:   /userCon/success.jsp
        //要加上"/"，来表示从工程名目录开始跳转页面
        return "success";
    }
}
```







































# Tips



## 搭建web环境



添加webapp目录以及web配置文件

![image-20210131180603583](../picture/SpringMVC%E7%AC%94%E8%AE%B0/image-20210131180603583.png)



配置Tomcat服务器



![image-20210131180647745](../picture/SpringMVC%E7%AC%94%E8%AE%B0/image-20210131180647745.png)







![image-20210131180705841](../picture/SpringMVC%E7%AC%94%E8%AE%B0/image-20210131180705841.png)





![image-20210131180739924](../picture/SpringMVC%E7%AC%94%E8%AE%B0/image-20210131180739924.png)







![image-20210131180816135](../picture/SpringMVC%E7%AC%94%E8%AE%B0/image-20210131180816135.png)



![image-20210131180825646](../picture/SpringMVC%E7%AC%94%E8%AE%B0/image-20210131180825646.png)



选择artifact加入

![image-20210131180842061](../picture/SpringMVC%E7%AC%94%E8%AE%B0/image-20210131180842061.png)



配置路径

![image-20210131180925781](../picture/SpringMVC%E7%AC%94%E8%AE%B0/image-20210131180925781.png)















































































































































