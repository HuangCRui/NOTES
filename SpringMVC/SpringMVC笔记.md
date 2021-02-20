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

 



## Spring提供获取applicationcontext的工具





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
<!--        每次在访问时任何请求都要走这个servlet(没有后缀，只是路径访问) -->
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





# SpringMVC的数据响应





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

> 不带前缀forward / redirect 就会尝试和视图解析器进行拼接
>
> 带前缀，如 `return "redirect:/role/list"` 就会直接跳转

![image-20210201225159242](../picture/SpringMVC%E7%AC%94%E8%AE%B0/image-20210201225159242.png)



---



返回ModelAndView类的对象



![image-20210201225834448](../picture/SpringMVC%E7%AC%94%E8%AE%B0/image-20210201225834448.png)





```java
@Controller
public class UserController {
    //这些方法都是SpringMVC来调用

    //在Controller方法的形参上可以直接使用原生的HttpServeltRequest对象，只需声明即可
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





## 回写数据



>  **回写数据** 
>
> ​	直接返回字符串
>
> ​	返回对象或集合    



通过SpringMVC框架注入的response对象，使用`response.getWriter().print(“hello world”)` 回写数据，此时不需要视图跳转，业务方法返回值为void

将需要回写的**字符串直接返回**，但此时需要通过`@ResponseBody`注解告知SpringMVC框架，方法返回的字符串**不是跳转是直接在http响应体中返回**



```java
//返回字符串，是想跳转还是字符串展示？告诉SpringMVC是要进行字符串展示
//文.件[/jsp/hello huangcr.jsp] 未找到

@ResponseBody//不进行页面跳转，直接回写数据
@RequestMapping("/quick7")
public String save7() throws IOException {
    return "hello huangcr";
}

//注入response对象
@RequestMapping("/quick6")
public void save6(HttpServletResponse response) throws IOException {
    response.getWriter().print("hello hcr");
}
```





### 直接回写json格式字符串



```xml
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-core</artifactId>
    <version>2.11.3</version>
</dependency>
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.11.3</version>
</dependency>
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-annotations</artifactId>
    <version>2.11.3</version>
</dependency>
```



```java
@ResponseBody
@RequestMapping(value = "/quick9")
public String save9() throws IOException {
    User user = new User();
    user.setAge(20);
    user.setUsername("hcr");
    //使用json的转换工具将对象转换成json格式的字符串再返回
    ObjectMapper objectMapper = new ObjectMapper();
    String json = objectMapper.writeValueAsString(user);
    return json;
}


@ResponseBody//不要进行页面跳转，直接回写数据
@RequestMapping(value = "/quick8")
public String save8() throws IOException {
    return "{\"username\":\"hcr\",\"age\":18}";
}
```



### 返回对象或集合



**转换的工作，框架封装**







`E:/maven-local-repository/org/springframework/spring-webmvc/5.3.3/spring-webmvc-5.3.3.jar!/org/springframework/web/servlet/DispatcherServlet.properties`

![image-20210202172727948](../picture/SpringMVC%E7%AC%94%E8%AE%B0/image-20210202172727948.png)





set方法来指定一个转换器

```java
public void setMessageConverters(List<HttpMessageConverter<?>> messageConverters) {
    this.messageConverters = messageConverters;
}
```



```java
@ResponseBody//还是要写  代表不是跳转
@RequestMapping(value = "/quick10")
//期望SpringMVC自动将这个对象转换为json格式字符串
public User save10() throws IOException {
    User user = new User();
    user.setAge(21);
    user.setUsername("hcrrr");
    //把要返回的对象或集合直接return
    return user;
}
```



```xml
<!--  配置处理器映射器-->
<bean id="requestMappingHandlerAdapter" class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter">
    <property name="messageConverters" >
        <list>
            <bean class="org.springframework.http.converter.json.MappingJackson2HttpMessageConverter"/>
        </list>
    </property>
</bean>
```



配置后在方法上添加@ResponseBody就可以返回json格式的字符串，但是这样**配置比较麻烦**，配置的代码比较多，因此，我们可以**使用mvc的注解驱动代替上述配置**

```xml
<mvc:annotation-driven/>
```

在 SpringMVC 的各个组件中，**处理器映射器**、**处理器适配器**、**视图解析器**  称为 SpringMVC 的三大组件。

 

使用`<mvc:annotation-driven />`自动加载 RequestMappingHandlerMapping（处理映射器）和

RequestMappingHandlerAdapter（ 处 理 适 配 器 ），可用在Spring-xml.xml配置文件中使用

`<mvc:annotation-driven />`**替代注解处理器和适配器的配置。**

同时使用`<mvc:annotation-driven />`**默认底层就会集成jackson进行对象或集合的json格式字符串的转换（不用手动配置）**





---

要点：

1） 页面跳转

直接返回字符串

通过ModelAndView对象返回

2） 回写数据 

直接返回字符串

HttpServletResponse 对象直接写回数据，HttpServletRequest对象带回数据，Model对象带回数据或者@ResponseBody将字符串数据写回

返回对象或集合 

@ResponseBody+`<mvc:annotation-driven/>   `













# SpringMVC的请求



请求参数类型：

客户端请求参数的格式是：name=value&name=value……

服务器端要获得请求的参数，有时还需要进行**数据的封装**，SpringMVC可以接收如下类型的参数

- 基本类型参数

- POJO类型参数

- 数组类型参数

- 集合类型参数





## 获取各种类型的请求参数

---



**获得基本类型参数：**



Controller中的业务方法的参数名称要与请求参数的name一致，参数值会**自动映射匹配**。并且**能自动做类型转换**；

自动的类型转换是指从**String向其他类型的转换**





![image-20210202202303564](../picture/SpringMVC%E7%AC%94%E8%AE%B0/image-20210202202303564.png)

```java
@RequestMapping("/quick11")
@ResponseBody//代表不进行页面跳转，void代表responsebody响应体是空的
public void save11(String name, int age){
    System.out.println(name);
    System.out.println(age);
}
```



---



**获得pojo类型参数**



Controller中的业务方法的POJO参数的属性名与请求参数的name一致，参数值会自动映射匹配。

SpringMVC也会自动封装这些属性值



![image-20210202202759687](../picture/SpringMVC%E7%AC%94%E8%AE%B0/image-20210202202759687.png)



```java
@RequestMapping("/quick12")
@ResponseBody//代表不进行页面跳转，void代表responsebody响应体是空的
public void save12(User user){
    System.out.println(user);
}
```



---



**获得数组类型参数**



Controller中的业务方法**数组名称**与**请求参数的name一致**，参数值会自动映射匹配。



```java
@RequestMapping("/quick13")
@ResponseBody//代表不进行页面跳转，void代表responsebody响应体是空的
//http://localhost:8088/mvc/quick13?a=111&a=123&a=000&a=666
public void save13(int[] a){
    System.out.println(Arrays.toString(a));
}
```





---



**获得集合类型参数**



获得集合参数时，要**将集合参数包装到一个POJO中**才可以。



需要将集合包装到一个对象VO中





```java
@RequestMapping("/quick14")
@ResponseBody//代表不进行页面跳转，void代表responsebody响应体是空的
//VO{userList=[User{username='李淳罡', age=111}, User{username='徐凤年', age=222}]}
public VO save14(VO vo){
    System.out.println(vo);
    return vo;
}
```



```html
    <form action="${pageContext.request.contextPath}/quick14" method="post">
<%--        List中封装User对象  表明是第几个对象的哪个属性--%>
<%--        [0]代表集合的第一个元素， username--%>
        <input type="text" name="userList[0].username">
        <input type="text" name="userList[0].age">
        <input type="text" name="userList[1].username"><br/>
        <input type="text" name="userList[1].age"><br/>
        <input type="submit" value="提交">
    </form>
```



```java
public class VO {
    @Override
    public String toString() {
        return "VO{" +
                "userList=" + userList +
                '}';
    }
	//装的是List对象
    private List<User> userList;

    public List<User> getUserList() {
        return userList;
    }

    public void setUserList(List<User> userList) {
        this.userList = userList;
    }
}
```



---



当使用ajax提交时，可以指定contentType为json形式，那么在方法参数位置使用**@RequestBody可以直接接收集合数据**，而无需使用POJO进行包装





**静态资源访问权限**

![image-20210202215304261](../picture/SpringMVC%E7%AC%94%E8%AE%B0/image-20210202215304261.png)





```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
    <script src="${pageContext.request.contextPath}/jq/jquery-3.3.1.js"></script>
    <script>
        var userList = new Array();
        userList.push({username:"李淳罡",age:80});
        userList.push({username:"曹长卿",age:60});

        $.ajax({
            type:"POST",
            url:"${pageContext.request.contextPath}/quick15",
            data:JSON.stringify(userList),
            contentType:"application/json;charset=utf-8"
        })
    </script>
</head>
<body>
</body>
</html>
```



```xml
<mvc:resources mapping="/jq/**" location="/jq/"/>
```



```java
@RequestMapping("/quick15")
@ResponseBody//代表不进行页面跳转，void代表responsebody响应体是空的
public void save15(@RequestBody List<User> userList){
    System.out.println(userList);
}
```







## 静态资源访问的开启



`org.springframework.web.servlet.DispatcherServlet.noHandlerFound No mapping found for HTTP request with URI [/webtest/jq/jquery-3.3.1.js] in DispatcherServlet with name 'dispatcherServlet'`

原因在前端控制器dispatcherservlet，

`<url-pattern>/</url-pattern>`

去匹配jquery文件



当有静态资源需要加载时，比如jquery文件，通过谷歌开发者工具抓包发现，没有加载到jquery文件，原因是SpringMVC的前端控制器DispatcherServlet的url-pattern配置的是/,代表对所有的资源都进行过滤操作，我们可以通过以下两种方式指定放行静态资源：

•在spring-mvc.xml配置文件中指定放行的资源

​     `<mvc:resources mapping="/js/**"location="/js/"/> `

•使用`<mvc:default-servlet-handler/>`标签



```xml
<!--开发资源的访问-->
    <!--<mvc:resources mapping="/js/**" location="/js/"/>
    <mvc:resources mapping="/img/**" location="/img/"/>-->

    <mvc:default-servlet-handler/>
```

mapping代表映射地址

location代表哪个目录下的资源是对外开放的









## 参数绑定注解@RequestParam



当**请求的参数名称**与**Controller的业务方法参数名称**不一致时，就需要通过@RequestParam注解显示的绑定





![image-20210203175041440](../picture/SpringMVC%E7%AC%94%E8%AE%B0/image-20210203175041440.png)



```java
@RequestMapping("/quick15")
@ResponseBody
//http://localhost:8088/mvc/quick16?na=abc
public void save15(@RequestBody List<User> userList){
    System.out.println(userList);
}
```

- value：与请求参数名称
- required：在指定的请求参数是否必须包括，**默认是true**，提交时如果没有这个参数，就**会报错**

- defaultValue：当没有指定请求参数时，使用指定的默认值赋值







## 获得Restful风格的参数



Restful是一种软件架构风格、设计风格，而不是标准，只是提供了一组设计原则和约束条件。主要用于客户端和服务器交互类的软件，基于这个风格设计的软件可以更简洁，更有层次，更易于实现缓存机制等。

Restful风格的请求是使用**“url+请求方式”**表示**一次请求目的**的，HTTP 协议里面四个表示操作方式的动词如下：

GET：用于获取资源

POST：用于新建资源

PUT：用于更新资源

DELETE：用于删除资源  



> 例如：
>
> /user/1    GET 		 ：       得到 id = 1 的 user
>
> /user/1   DELETE	 ：  	 删除 id = 1 的 user
>
> /user/1    PUT		  ：       更新 id = 1 的 user
>
> /user       POST		：       新增 user
>



上述url地址/user/1中的1就是**要获得的请求参数**，在SpringMVC中可以使用占位符进行参数绑定。地址/user/1可以写成`/user/{id}`，占位符{id}对应的就是1的值。在业务方法中我们可以使用**`@PathVariable`**注解进行占位符的匹配获取工作。



**1在这里不是作为？后的参数部分，而是url地址**



![image-20210203180452899](../picture/SpringMVC%E7%AC%94%E8%AE%B0/image-20210203180452899.png)

`http://localhost:8088/mvc/quick17/hcr`

```java
@RequestMapping("/quick17/{name}")
@ResponseBody
public void save17(@PathVariable(value = "name") String name){
    System.out.println(name);
}
```





## 自定义类型转换器



SpringMVC 默认已经提供了一些常用的类型转换器，例如客户端提交的**字符串转换成int型进行参数设置**。

但是不是所有的数据类型都提供了转换器，没有提供的就需要自定义转换器，例如：日期类型的数据就需要自定义转换器。



1. 自定义转换器类实现Converter接口
2. 在配置文件中声明转换器
3. 在 `<annotation-driven>`中引用转换器



```java
public class DateConverter implements Converter<String, Date> {
    public Date convert(String dateStr) {
        //将日期字符串转换成日期对象 返回
        SimpleDateFormat format = new SimpleDateFormat("yyyy-MM-dd");
        Date date = null;
        try {
            date = format.parse(dateStr);
        } catch (ParseException e) {
            e.printStackTrace();
        }
        return date;
    }
}
```

![image-20210203181715226](../picture/SpringMVC%E7%AC%94%E8%AE%B0/image-20210203181715226.png)



```xml
<mvc:annotation-driven conversion-service="conversionService2"/>


<bean id="dateConverter" class="com.example.converter.DateConverter"/>
<!--    声明转换器-->
<bean id="conversionService2" class="org.springframework.context.support.ConversionServiceFactoryBean">
    <property name="converters">
        <set>
            <bean class="com.example.converter.DateConverter"/>
        </set>
    </property>
</bean>
```







## 获得servlet相关api



SpringMVC支持使用原始ServletAPI对象作为控制器方法的参数进行注入，常用的对象如下：

HttpServletRequest

HttpServletResponse

HttpSession



```java
@RequestMapping(value="/quick19")
    @ResponseBody
    public void save19(HttpServletRequest request, HttpServletResponse response, HttpSession session) throws IOException {
        System.out.println(request);
        System.out.println(response);
        System.out.println(session);
    }
```



## 获得请求头



使用@RequestHeader可以获得请求头信息，相当于web阶段学习的request.getHeader(name)

@RequestHeader注解的属性如下：

- value：请求头的名称

- required：是否必须携带此请求头



![image-20210203233531483](../picture/SpringMVC%E7%AC%94%E8%AE%B0/image-20210203233531483.png)



```java
@RequestMapping(value="/quick20")
@ResponseBody
public String save20(@RequestHeader(value = "User-Agent") String user_agent) throws IOException {
    return user_agent;
}
```





使用@CookieValue可以直接获得指定Cookie的值

@CookieValue注解的属性如下：

- value：指定cookie的名称

- required：是否必须携带此cookie



```java
@RequestMapping(value="/quick20")
@ResponseBody
public String save20(@CookieValue(value = "JSESSIONID") String jsid) throws IOException {
    return jsid;
}
```

![image-20210203234330937](../picture/SpringMVC%E7%AC%94%E8%AE%B0/image-20210203234330937.png)



























## springmvc的文件上传





### 单文件上传



文件上传客户端表单需要满足：

- 表单项`type=“file”`

- 表单的**提交方式**是`post`

- **表单的enctype属性是多部分表单形式，及`enctype=“multipart/form-data”`**



url编码：键值对形势

![image-20210204151834411](../picture/SpringMVC%E7%AC%94%E8%AE%B0/image-20210204151834411.png)



```html
<form action="${pageContext.request.contextPath}/quick21" method="post" enctype="multipart/form-data">
    名称<input type="text" name="username"><br>
    文件<input type="file" name="upload"><br>
    <input type="submit" value="提交">
</form>
```





![image-20210204152241354](../picture/SpringMVC%E7%AC%94%E8%AE%B0/image-20210204152241354.png)



表单的数据都在http请求体当中，服务端一样也可以获得表单的所有数据

字符串切割？

------

步骤：

导入fileupload和io坐标

```xml
<dependency>
    <groupId>commons-fileupload</groupId>
    <artifactId>commons-fileupload</artifactId>
    <version>1.3.1</version>
</dependency>
<dependency>
    <groupId>commons-io</groupId>
    <artifactId>commons-io</artifactId>
    <version>2.6</version>
</dependency>
```



```java
@RequestMapping(value="/quick21")
@ResponseBody
public void save21(String username,  MultipartFile upload) throws IOException {
    System.out.println(username);
    System.out.println(upload);
    //MultipartFile[field="upload", filename=xxx.pdf, contentType=application/pdf, size=142702]
}
```



---



将文件转移到另一个文件夹中

```java
@RequestMapping(value="/quick21")
@ResponseBody
public void save21(String username,  MultipartFile upload) throws IOException {
    System.out.println(username);
    System.out.println(upload);
    //MultipartFile[field="upload", filename=xxx.pdf, contentType=application/pdf, size=142702]
    //获得上传文件的名称
    String originalFilename = upload.getOriginalFilename();
    //将文件保存
    upload.transferTo(new File("D:/desktop/Code/" + originalFilename));
}
```





### 多文件上传



多文件上传，只需要将页面修改为多个文件上传项，将方法参数MultipartFile类型修改为MultipartFile[]即可





```java
@RequestMapping(value="/quick22")
@ResponseBody
//用数组来接  表单中name都要相等  自动转为数组
public void save22(String username, MultipartFile[] upload) throws IOException {
    System.out.println(username);
    for(MultipartFile multipartFile : upload){
        String originalFilename = multipartFile.getOriginalFilename();
        multipartFile.transferTo(new File("D://desktop//Code/" + originalFilename));
    }
}

@RequestMapping(value="/quick21")
@ResponseBody
public void save21(String username,  MultipartFile upload, MultipartFile upload2) throws IOException {
    System.out.println(username);
    //MultipartFile[field="upload", filename=背包入门.pdf, contentType=application/pdf, size=142702]
    //获得上传文件的名称
    String originalFilename = upload.getOriginalFilename();
    //将文件保存
    upload.transferTo(new File("D:/desktop/Code/" + originalFilename));

    String originalFilename2 = upload2.getOriginalFilename();
    //将文件保存
    upload.transferTo(new File("D:/desktop/Code/" + originalFilename2));
}
```





## SpringMVC的请求-知识要点



![image-20210204172418572](../picture/SpringMVC%E7%AC%94%E8%AE%B0/image-20210204172418572.png)







# springmvc的拦截器



Spring MVC 的拦截器类似于 Servlet  开发中的**过滤器 Filter**，用于**对处理器进行预处理和后处理**。

将拦截器按一定的顺序联结成一条链，这条链称为**拦截器链（InterceptorChain）**。在**访问被拦截的方法或字段**时，拦截器链中的拦截器就会按其之前定义的**顺序**被调用。**拦截器也是AOP思想的具体实现**。





---



## intercepter和filter区别



![image-20210204184524835](../picture/SpringMVC%E7%AC%94%E8%AE%B0/image-20210204184524835.png)



![image-20210204185242235](../picture/SpringMVC%E7%AC%94%E8%AE%B0/image-20210204185242235.png)





 

## 入门应用



> 自定义拦截器很简单，只有如下三步：
>
> ①创建拦截器类实现**`HandlerInterceptor`接口**
>
> ②**配置**拦截器
>
> ③测试拦截器的**拦截效果**



```xml
<!--    手动配置内部资源视图解析器-->
    <bean id="viewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
<!--        设置属性  /jsp/ +  success  + .jsp-->
        <property name="prefix" value="/jsp/"/>
        <property name="suffix" value=".jsp"/>
    </bean>


<!--    配置拦截器-->
    <mvc:interceptors>
        <mvc:interceptor>
<!--            对哪些资源执行拦截操作-->
            <mvc:mapping path="/**"/>
            <bean class="com.example.mvc.interceptor.MyInterceptor1"/>
        </mvc:interceptor>
    </mvc:interceptors>
```



```java
@RequestMapping("target")
public ModelAndView show(){
    System.out.println("目标资源执行。。。。。。");//拦截后这句话没执行  prehandle返回了false
    ModelAndView modelAndView = new ModelAndView();
    modelAndView.addObject("name", "hcr");
    modelAndView.setViewName("success");
    return modelAndView;
}
```



```html
<body>
<h1>Success! ${name}</h1> <!-- 这里一定是在配置好InternalResourceViewResolver后才有效-->
</body>
```



```java
@Override
//在目标方法执行之前执行
public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
    System.out.println("preHandle....");
    //false?
    return true;
}
```



![image-20210204191452345](../picture/SpringMVC%E7%AC%94%E8%AE%B0/image-20210204191452345.png)





```java
public class MyInterceptor1 implements HandlerInterceptor {

    @Override
    //在目标方法执行之前执行
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        System.out.println("preHandle....");
        String param = request.getParameter("param");
        //如果用param.equals，不加param参数时就会出现nullpointerexception
        if("yes".equals(param)){
            return true;
        }else{
            request.getRequestDispatcher("/error.jsp").forward(request, response);
            return false;
        }
        //返回false代表不放行，  可以重定向/转发向其他资源
        //return true;
    }

    @Override
    //在目标方法执行之后，视图对象返回之前执行
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        //可以获取modelandview，进行改变
        //截胡了save()传过来的ModelAndView，并修改了它的参数
        modelAndView.addObject("name", "李淳罡");
        System.out.println("postHandle");
    }

    @Override
    //在整个流程都执行完毕后 执行
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        System.out.println("aftercompletion");
    }
}
```







```xml
<!--    配置拦截器-->
    <mvc:interceptors>
        <mvc:interceptor>
<!--            对哪些资源执行拦截操作-->
            <mvc:mapping path="/**"/>
            <bean class="com.example.mvc.interceptor.MyInterceptor1"/>
        </mvc:interceptor>
        <mvc:interceptor>
            <!--            对哪些资源执行拦截操作-->
            <mvc:mapping path="/**"/>
            <bean class="com.example.mvc.interceptor.MyInterceptor2"/>
        </mvc:interceptor>
    </mvc:interceptors>
```

根据**配置的顺序**，来决定拦截器的执行顺序



> 当拦截器的preHandle方法返回true则会执行目标资源，如果返回false则不执行目标资源
>
> 多个拦截器情况下，配置在前的先执行，配置在后的后执行
>
> 拦截器中的方法执行顺序是：preHandler-------目标资源----postHandle---- afterCompletion



![image-20210204210816083](../picture/SpringMVC%E7%AC%94%E8%AE%B0/image-20210204210816083.png)





## 小结



![image-20210204211228780](../picture/SpringMVC%E7%AC%94%E8%AE%B0/image-20210204211228780.png)







## 用户登录权限控制分析

















# springmvc异常处理机制



在业务层处理异常try/catch的弊端：

- 抓异常的动作跟业务代码耦合
- 很多相同工作，抽取出来处理同一个异常的工作



系统中异常包括两类：预期异常和**运行时异常RuntimeException**，前者通过捕获异常从而**获取异常信息**，后者主要通过规范代码开发、测试等手段减少运行时异常的发生。

系统的Dao、Service、Controller出现都通过`throws Exception`**向上抛出**，最后由`SpringMVC`**前端控制器**交由**异常处理器**进行**异常处理**



![image-20210204215120802](../picture/SpringMVC%E7%AC%94%E8%AE%B0/image-20210204215120802.png)







> 异常处理两种方式：
>
> ① 使用Spring MVC提供的**简单异常处理器**`SimpleMappingExceptionResolver`(异常和跳转页面映射)
>
> ② 实现Spring的**异常处理接口**`HandlerExceptionResolver`—— **自定义**自己的异常处理器



## 简单异常处理器SimpleMappingExceptionResolver



SpringMVC已经定义好了该类型转换器，在使用时可以根据项目情况进行相应异常与视图的映射配置





```xml
<!--    手动配置内部资源视图解析器-->
    <bean id="viewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
<!--        设置属性  /jsp/ +  success  + .jsp-->
        <property name="prefix" value="/jsp/"/>
        <property name="suffix" value=".jsp"/>
    </bean>



<!--    配置异常处理器-->
    <bean id="exceptionResolver" class="org.springframework.web.servlet.handler.SimpleMappingExceptionResolver">
<!--        默认错误视图-->
        <property name="defaultErrorView" value="error"/>
        <property name="exceptionMappings">
            <map>
                <entry key="java.lang.ClassCastException" value="error1"/>
                <entry key="com.example.exception.MyException" value="success"/>
            </map>
        </property>
    </bean>
```









## 自定义异常处理步骤



①创建异常处理器类**实现`HandlerExceptionResolver`**

```java
public class MyExceptionResolver implements HandlerExceptionResolver {
    @Override
    /*
    参数Exception：异常对象
    返回值ModelAndView：跳转到错误视图信息
     */
    public ModelAndView resolveException(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Object o, Exception e) {
        ModelAndView modelAndView = new ModelAndView();
        //可以拿到异常对象
        if(e instanceof MyException){
            modelAndView.addObject("info", "自定义异常");

        }else if(e instanceof ClassCastException){
            modelAndView.addObject("info", "类型转换异常");
        }
        modelAndView.setViewName("error");
        return modelAndView;
    }
}
```



②配置异常处理器

```xml
<!--    自定义异常处理器-->
    <bean id="exceptionResolver" class="com.example.exception.MyExceptionResolver"/>
```



③编写异常页面

```html
<h1>ERROR!!!  ${info}</h1>
```







异常处理方式

> 配置简单异常处理器SimpleMappingExceptionResolver
>
> 自定义异常处理器

自定义异常处理步骤

> ①创建异常处理器类实现HandlerExceptionResolver
>
> ②配置异常处理器
>
> ③编写异常页面
>
> ④测试异常跳转









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











## SpringMVC回写中文字符串数据，出现乱码



SpringMVC 框架可以使用 @RequestBody 和 @ResponseBody 两个注解，分别完成请求到对象和对象到响应的转换，底层这种灵活的响应机制，就是Spring3.X 新引入的 **HttpMessageConverter 即消息转换器机制。该机制默认的编码为 ISO-8859-1**。



```java
//E:/maven-local-repository/org/springframework/spring-web/5.3.3/spring-web-5.3.3.jar!/org/springframework/http/converter/StringHttpMessageConverter.class:101
static {
    DEFAULT_CHARSET = StandardCharsets.ISO_8859_1;
}
```





**1.在 @RequestMapping 里面加入 produces="text/html;charset=UTF-8"**

```java
@ResponseBody
@RequestMapping(value="/logon",produces="text/html; charset=UTF-8")
```

---





**2.设置转换器和属性字符集**

使用HttpMessageConverter接口的相关实现类

```java
//org.springframework.http.converter.AbstractHttpMessageConverter#setSupportedMediaTypes
public void setSupportedMediaTypes(List<MediaType> supportedMediaTypes) {
    Assert.notEmpty(supportedMediaTypes, "MediaType List must not be empty");
    this.supportedMediaTypes = new ArrayList(supportedMediaTypes);
}

//org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter#setMessageConverters
public void setMessageConverters(List<HttpMessageConverter<?>> messageConverters) {
    this.messageConverters = messageConverters;
}
```





```xml
<bean id="requestMappingHandlerAdapter" class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter">
    <property name="messageConverters" >
        <list>
            <bean class="org.springframework.http.converter.json.MappingJackson2HttpMessageConverter"/>
            <bean class="org.springframework.http.converter.StringHttpMessageConverter">
                <property name="supportedMediaTypes">
                    <list>
                        <value>text/plain;charset=utf-8</value>
                        <value>text/html;charset=UTF-8</value>
                    </list>
                </property>
            </bean>
        </list>
    </property>
</bean>
                            
<mvc:annotation-driven/>
```

> **注意：一定要放到<mvc:annotation-driven />的上面，否则不会生效。**









## form表单传输显示出现中文乱码  配置全局乱码过滤器



```xml
<filter>
    <filter-name>characterEncodingFilter</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <init-param>
        <param-name>encoding</param-name>
        <param-value>UTF-8</param-value>
    </init-param>
    <init-param>
        <param-name>forceEncoding</param-name>
        <param-value>true</param-value>
    </init-param>
</filter>
<filter-mapping>
    <filter-name>characterEncodingFilter</filter-name>
    <url-pattern>/*</url-pattern>  <!--  针对所有文件，都是用utf-8编码 -->
</filter-mapping>
```













##  url-pattern 中 / 和 /*的区别



关于web.xml的url映射的小知识:
`< url-pattern>/</url-pattern>`  会匹配到/login这样的**路径型url**，不会匹配到模式为  `*.jsp`这样的**后缀型url**

即：`*.jsp`不会进入spring的 DispatcherServlet类



`< url-pattern>/*</url-pattern>` 会匹配所有url：**路径型**的和**后缀型的url**(包括`/login`,`*.jsp`,`*.js`和`*.html`等)



![image-20210202212918506](../picture/SpringMVC%E7%AC%94%E8%AE%B0/image-20210202212918506.png)





![image-20210202212939982](../picture/SpringMVC%E7%AC%94%E8%AE%B0/image-20210202212939982.png)



当配置相同的情况下，**DispathcherServlet配置成/和/* 的区别**
<一>　/：使用/配置路径，**直接访问到jsp**，**不经springDispatcherServlet**
<二>　/* ：配置/* 路径，不能访问到多视图的jsp



在客户端调用URL：**/user/list**然后**返回user.jsp视图**，

- 当配置的是/：**DispathcherServlet拿到这个请求**然后返回对应的controller，
  然后依据Dispather Type为Forward类型转发到user.jsp视图，即就是请求user.jsp视图(/user/user.jsp)，此时Dispather没有截/user/user.jsp，
  因为此时你配置的是默认的/，就顺利的交给ModleAndView去处理显示了。



- 当配置的是/*：DispathcherServlet拿到这个请求然后返回对应的controller，然后通过Dispather Type通过Forward转发到user.jsp视图，即就是请求user.jsp视图(/user/user.jsp)，此时**Dispather已经拦截/user/user.jsp**，Dispatcher会把他**当作Controller去匹配**，**没有匹配到**就会报404错误。

**no mapping**

![image-20210202213546078](../picture/SpringMVC%E7%AC%94%E8%AE%B0/image-20210202213546078.png)













## 无法更新静态资源 out/target目录不同步更新





![image-20210203003547916](../picture/SpringMVC%E7%AC%94%E8%AE%B0/image-20210203003547916.png)





Build - Rebuild   重新构建项目，可以将静态资源加载到out目录下







```java
<build>    
        <resources>
            <resource>
                <directory>src/main/java</directory>
                <includes>
                    <include>**/*.*</include>
                </includes>
            </resource>
            <resource>
                <directory>src/main/resources</directory>
                <includes>
                    <include>**/*.*</include>
                </includes>
            </resource>
        </resources>
</build>
```

执行`maven  clean`后，启动项目即可更新资源











## 找不到包

将新导入的包加入到目录中，tomcat才能找到这些对应依赖

![image-20210204160014094](../picture/SpringMVC%E7%AC%94%E8%AE%B0/image-20210204160014094.png)





## datasource BUG



原因是在applicationContext.xml  和  `spring-mvc.xml`中**重复扫描了包**



```xml
<context:component-scan base-package="com.example.mvc"/>


<context:component-scan base-package="com.example.mvc.controller"/>
```



---



或者按照网上的解决办法

spring boot会默认加载`org.springframework.boot.autoconfigure.jdbc.DataSourceAutoConfiguration`类使用了@Configuration注解向spring注入了dataSource bean,因为工程中没有关于dataSource相关的配置信息，当spring创建dataSource bean因缺少相关的信息就会报错

```java
@SpringBootApplication(exclude = {DataSourceAutoConfiguration.class})
public class DemoApplication {
    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }

}
```



```
org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'dataSource' defined in class path resource [org/springframework/boot/autoconfigure/jdbc/DataSourceConfiguration$Hikari.class]: Bean instantiation via factory method failed; nested exception is org.springframework.beans.BeanInstantiationException: Failed to instantiate [com.zaxxer.hikari.HikariDataSource]: Factory method 'dataSource' threw exception; nested exception is org.springframework.boot.autoconfigure.jdbc.DataSourceProperties$DataSourceBeanCreationException: Failed to determine a suitable driver class
	at org.springframework.beans.factory.support.ConstructorResolver.instantiate(ConstructorResolver.java:658)
	at org.springframework.beans.factory.support.ConstructorResolver.instantiateUsingFactoryMethod(ConstructorResolver.java:638)
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.instantiateUsingFactoryMethod(AbstractAutowireCapableBeanFactory.java:1336)
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.createBeanInstance(AbstractAutowireCapableBeanFactory.java:1179)
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.doCreateBean(AbstractAutowireCapableBeanFactory.java:571)
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.createBean(AbstractAutowireCapableBeanFactory.java:531)
	at org.springframework.beans.factory.support.AbstractBeanFactory.lambda$doGetBean$0(AbstractBeanFactory.java:335)
	at org.springframework.beans.factory.support.DefaultSingletonBeanRegistry.getSingleton(DefaultSingletonBeanRegistry.java:234)
	at org.springframework.beans.factory.support.AbstractBeanFactory.doGetBean(AbstractBeanFactory.java:333)
	at org.springframework.beans.factory.support.AbstractBeanFactory.getBean(AbstractBeanFactory.java:208)
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.preInstantiateSingletons(DefaultListableBeanFactory.java:944)
	at org.springframework.context.support.AbstractApplicationContext.finishBeanFactoryInitialization(AbstractApplicationContext.java:923)
	at org.springframework.context.support.AbstractApplicationContext.refresh(AbstractApplicationContext.java:588)
	at org.springframework.web.context.ContextLoader.configureAndRefreshWebApplicationContext(ContextLoader.java:401)
	at org.springframework.web.context.ContextLoader.initWebApplicationContext(ContextLoader.java:292)
	at org.springframework.web.context.ContextLoaderListener.contextInitialized(ContextLoaderListener.java:103)
	at org.apache.catalina.core.StandardContext.listenerStart(StandardContext.java:4716)
	at org.apache.catalina.core.StandardContext.startInternal(StandardContext.java:5177)
	at org.apache.catalina.util.LifecycleBase.start(LifecycleBase.java:183)
	at org.apache.catalina.core.ContainerBase.addChildInternal(ContainerBase.java:717)
	at org.apache.catalina.core.ContainerBase.addChild(ContainerBase.java:690)
	at org.apache.catalina.core.StandardHost.addChild(StandardHost.java:706)
	at org.apache.catalina.startup.HostConfig.manageApp(HostConfig.java:1727)
	at jdk.internal.reflect.GeneratedMethodAccessor324.invoke(Unknown Source)
	at java.base/jdk.internal.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
	at java.base/java.lang.reflect.Method.invoke(Method.java:567)
	at org.apache.tomcat.util.modeler.BaseModelMBean.invoke(BaseModelMBean.java:288)
	at java.management/com.sun.jmx.interceptor.DefaultMBeanServerInterceptor.invoke(DefaultMBeanServerInterceptor.java:809)
	at java.management/com.sun.jmx.mbeanserver.JmxMBeanServer.invoke(JmxMBeanServer.java:801)
	at org.apache.catalina.mbeans.MBeanFactory.createStandardContext(MBeanFactory.java:459)
	at org.apache.catalina.mbeans.MBeanFactory.createStandardContext(MBeanFactory.java:408)
	at jdk.internal.reflect.GeneratedMethodAccessor322.invoke(Unknown Source)
	at java.base/jdk.internal.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
	at java.base/java.lang.reflect.Method.invoke(Method.java:567)
	at org.apache.tomcat.util.modeler.BaseModelMBean.invoke(BaseModelMBean.java:288)
	at java.management/com.sun.jmx.interceptor.DefaultMBeanServerInterceptor.invoke(DefaultMBeanServerInterceptor.java:809)
	at java.management/com.sun.jmx.mbeanserver.JmxMBeanServer.invoke(JmxMBeanServer.java:801)
	at java.management/com.sun.jmx.remote.security.MBeanServerAccessController.invoke(MBeanServerAccessController.java:468)
	at java.management.rmi/javax.management.remote.rmi.RMIConnectionImpl.doOperation(RMIConnectionImpl.java:1466)
	at java.management.rmi/javax.management.remote.rmi.RMIConnectionImpl$PrivilegedOperation.run(RMIConnectionImpl.java:1307)
	at java.base/java.security.AccessController.doPrivileged(AccessController.java:689)
	at java.management.rmi/javax.management.remote.rmi.RMIConnectionImpl.doPrivilegedOperation(RMIConnectionImpl.java:1406)
	at java.management.rmi/javax.management.remote.rmi.RMIConnectionImpl.invoke(RMIConnectionImpl.java:827)
	at java.base/jdk.internal.reflect.GeneratedMethodAccessor133.invoke(Unknown Source)
	at java.base/jdk.internal.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
	at java.base/java.lang.reflect.Method.invoke(Method.java:567)
	at java.rmi/sun.rmi.server.UnicastServerRef.dispatch(UnicastServerRef.java:359)
	at java.rmi/sun.rmi.transport.Transport$1.run(Transport.java:200)
	at java.rmi/sun.rmi.transport.Transport$1.run(Transport.java:197)
	at java.base/java.security.AccessController.doPrivileged(AccessController.java:689)
	at java.rmi/sun.rmi.transport.Transport.serviceCall(Transport.java:196)
	at java.rmi/sun.rmi.transport.tcp.TCPTransport.handleMessages(TCPTransport.java:562)
	at java.rmi/sun.rmi.transport.tcp.TCPTransport$ConnectionHandler.run0(TCPTransport.java:796)
	at java.rmi/sun.rmi.transport.tcp.TCPTransport$ConnectionHandler.lambda$run$0(TCPTransport.java:677)
	at java.base/java.security.AccessController.doPrivileged(AccessController.java:389)
	at java.rmi/sun.rmi.transport.tcp.TCPTransport$ConnectionHandler.run(TCPTransport.java:676)
	at java.base/java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1128)
	at java.base/java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:628)
	at java.base/java.lang.Thread.run(Thread.java:835)
Caused by: org.springframework.beans.BeanInstantiationException: Failed to instantiate [com.zaxxer.hikari.HikariDataSource]: Factory method 'dataSource' threw exception; nested exception is org.springframework.boot.autoconfigure.jdbc.DataSourceProperties$DataSourceBeanCreationException: Failed to determine a suitable driver class
	at org.springframework.beans.factory.support.SimpleInstantiationStrategy.instantiate(SimpleInstantiationStrategy.java:185)
	at org.springframework.beans.factory.support.ConstructorResolver.instantiate(ConstructorResolver.java:653)
	... 58 common frames omitted
Caused by: org.springframework.boot.autoconfigure.jdbc.DataSourceProperties$DataSourceBeanCreationException: Failed to determine a suitable driver class
	at org.springframework.boot.autoconfigure.jdbc.DataSourceProperties.determineDriverClassName(DataSourceProperties.java:235)
	at org.springframework.boot.autoconfigure.jdbc.DataSourceProperties.initializeDataSourceBuilder(DataSourceProperties.java:176)
	at org.springframework.boot.autoconfigure.jdbc.DataSourceConfiguration.createDataSource(DataSourceConfiguration.java:48)
	at org.springframework.boot.autoconfigure.jdbc.DataSourceConfiguration$Hikari.dataSource(DataSourceConfiguration.java:90)
	at java.base/jdk.internal.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
	at java.base/jdk.internal.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
	at java.base/jdk.internal.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
	at java.base/java.lang.reflect.Method.invoke(Method.java:567)
	at org.springframework.beans.factory.support.SimpleInstantiationStrategy.instantiate(SimpleInstantiationStrategy.java:154)
	... 59 common frames omitted
04-Feb-2021 22:10:57.553 严重 [RMI TCP Connection(121)-127.0.0.1] org.apache.catalina.core.StandardContext.startInternal 一个或多个listeners启动失败，更多详细信息查看对应的容器日志文件
04-Feb-2021 22:10:57.554 严重 [RMI TCP Connection(121)-127.0.0.1] org.apache.catalina.core.StandardContext.startInternal 由于之前的错误，Context[/mvc]启动失败
[2021-02-04 10:10:58,078] Artifact mvcdemo1:war exploded: Error during artifact deployment. See server log for details.

```













