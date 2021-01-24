# HTML & CSS



![image-20210122220805487](../picture/JavaWeb%E7%AC%94%E8%AE%B0/image-20210122220805487.png)





![image-20210122222141152](../picture/JavaWeb%E7%AC%94%E8%AE%B0/image-20210122222141152.png)





## html标签介绍



```html
<!DOCTYPE html><!-- 约束,声明 -->
<!-- html标签表示html的开始   lang="zh_CN"表示中文    html标签中一般分为两部分,分别是:head和body    -->
<head><!-- 表示头部信息,一般包含三部分内容,title标签,css样式,js代码 -->
    <meta charset="UTF-8"><!-- 表示当前页面使用UTF-8字符集 -->
    <title>某东</title><!--表示标题-->
</head>
```



标签拥有自己的属性（包括body标签 bgcolor）



- 基本属性
- 事件属性



- 单标签`<br/> <hr/><img/>`
- 双标签





### 标签语法

- 哪怕是 `<br/>` **也一定要正确关闭 /**
- **注释不能嵌套**
- 属性必须有**值，属性值必须加引号**





## html 实体

**HTML 中的预留字符必须被替换为字符实体**



![image-20210122224213978](../picture/JavaWeb%E7%AC%94%E8%AE%B0/image-20210122224213978.png)





## 常用标签

- 标题标签

`<h1>  ---   <h6>`

align = left ...



- 超链接  `<a href=""     target="">`

![image-20210122224734200](../picture/JavaWeb%E7%AC%94%E8%AE%B0/image-20210122224734200.png)



- 无序列表 `<ul type="" 可以修改列表前面的符号 兼容问题> <li></li></ul>`
- 有序列表 `<ol> <li></li></ol>`

- 图片`<img src="../10-10.jpg" height="200" width="400" border=  alt=/>`

![image-20210122230340452](../picture/JavaWeb%E7%AC%94%E8%AE%B0/image-20210122230340452.png)

- 表格 `<table align="center" border="1" width="300" height="300" cellspacing="0"> `  **0表示两个单元格相加紧贴  还有一种是完全重合**
  - tr 行标签
  - th 表头标签
  - td 单元格标签   align
  - **跨行跨列**  colspan跨列   rowspan跨行



- iframe框架标签（**内嵌窗口**）`<iframe src="http://www.baidu.com"></iframe>`

> ifram标签和<a>标签组合使用，在a标签的target属性上设置ifram的name属性值



```html
<!--ifarme 标签可以在页面上开辟一个小区域显示一个单独的页面
ifarme 和 a 标签组合使用的步骤:
1 在 iframe 标签中使用 name 属性定义一个名称
2 在 a 标签的 target 属性上设置 iframe 的 name 的属性值
-->
<iframe src="3.标题标签.html" width="500" height="400" name="abc"></iframe>
<br/>

<ul>
<li><a href="0-标签语法.html" target="abc">0-标签语法.html</a></li>
<li><a href="1.font 标签.html" target="abc">1.font 标签.html</a></li>
<li><a href="2.特殊字符.html" target="abc">2.特殊字符.html</a></li>
</ul>
```



- div   **默认独占一行**
- span   它的长度是**封装数据的长度**
- p    默认会在段落的上方或下方各空出一行来（如果已有就不再空）



## 表单标签



用来收集用户信息的**所有元素集合**.然后把这些**信息发送给服务器**  



```html
<!--
form 标签就是表单
input type=text 是文件输入框 value 设置默认显示内容
input type=password 是密码输入框 value 设置默认显示内容
input type=radio 是单选框 name 属性可以对其进行分组 checked="checked"表示默认选中input type=checkbox 是复选框 checked="checked"表示默认选中
input type=reset 是重置按钮 value 属性修改按钮上的文本
input type=submit 是提交按钮 value 属性修改按钮上的文本
input type=button 是按钮 value 属性修改按钮上的文本
input type=file 是文件上传域
input type=hidden 是隐藏域 当我们要发送某些信息， 而这些信息， 不需要用户参与， 就可以使用隐藏域（提交的
时候同时发送给服务器）
select 标签是下拉列表框
option 标签是下拉列表框中的选项 selected="selected"设置默认选中
textarea 表示多行文本输入框 （起始标签和结束标签中的内容是默认值）
rows 属性设置可以显示几行的高度
cols 属性设置每行可以显示几个字符宽度
-->
<form>
用户名称： <input type="text" value="默认值"/><br/>
用户密码： <input type="password" value="abc"/><br/>
确认密码： <input type="password" value="abc"/><br/>
性别： <input type="radio" name="sex"/>男<input type="radio" name="sex" checked="checked" />女<br/>
兴趣爱好： <input type="checkbox" checked="checked" />Java<input type="checkbox" />JavaScript<input
type="checkbox" />C++<br/>
国籍： <select>
<option>--请选择国籍--</option>
<option selected="selected">中国</option>
<option>美国</option>
<option>小日本</option>
</select><br/>
自我评价： <textarea rows="10" cols="20">我才是默认值</textarea><br/>
<input type="reset" value="abc" />
<input type="submit"/>
<input type="file">   <!-- 文件上传 -->
</form>
```

---



**表单格式化**



将表单信息都放到table中，就会很整齐



---



**表单提交**



form标签



http://localhost:8680/?action=login&username=hcr&sex=on



**method 属性**

> 浏览器使用 method 属性设置的方法将表单中的数据传送给服务器进行处理。共有两种方法：POST 方法和 GET 方法。
>
> 如果采用 POST 方法，浏览器将会按照下面两步来发送数据。首先，浏览器将与 action  属性中指定的表单处理服务器建立联系，一旦建立连接之后，浏览器就会按分段传输的方法将数据发送给服务器。
>
> 在服务器端，一旦 POST  样式的应用程序开始执行时，就应该从一个标志位置读取参数，而一旦读到参数，在应用程序能够使用这些表单值以前，必须对这些参数进行解码。用户特定的服务器会明确指定应用程序应该如何接受这些参数。
>
> 另一种情况是采用 GET 方法，这时浏览器会与表单处理服务器建立连接，然后直接在一个传输步骤中发送所有的表单数据：浏览器会将数据直接附在表单的  action URL 之后。这两者之间用问号进行分隔。
>
> 一般浏览器通过上述任何一种方法都可以传输表单信息，而有些服务器只接受其中一种方法提供的数据。可以在 <form> 标签的 method  （方法）属性中指明表单处理服务器要用方法来处理数据，使 POST 还是 GET。



form 标签是表单标签
action 属性设置提交的服务器地址
method 属性设置提交的方式 GET(默认值)或 POST

表单提交的时候， 数据没有发送给服务器的三种情况：
1、 表单项没有 name 属性值
2、 单选、 复选（下拉列表中的 option 标签） 都需要添加 value 属性， 以便发送给服务器
3、 表单项不在提交的 form 标签中

---

GET 请求的特点是：**获取信息**
1、 浏览器地址栏中的**地址**是： action 属性[+?+请求参数]
**请求参数的格式是： name=value&name=value**
2、 不安全
3、 它有**数据长度的限制**



POST 请求的特点是：
1、 浏览器地址栏中只有 action 属性值
2、 相对于 GET 请求要安全

3、 理论上**没有数据长度的限制**

```html
<form action="http://localhost:8080" method="post">
<input type="hidden" name="action" value="login" />
```





















# XML



**xml是可扩展（标签可以自己定义）的标记性语言**



xml 的主要作用有：
1、 用来**保存数据**， 而且这些数据具有***自我描述性***
2、 它还可以做为**项目或者模块**的**配置文件**
3、 还可以做为**网络传输数据**的格式（现在都是以 **JSON** 为主）  





## xml语法



**语法出错浏览器无法解析**





### 文档声明



```xml
<?xml version="1.0" encoding="UTF-8"?> xml 声明。
<!-- xml 声明 version 是版本的意思 encoding 是编码 -->
```

而且这个<?xml 要连在一起写， 否则会有报错



属性
version 								是版本号
encoding 							是 xml 的文件编码
standalone="yes/no" 		表示这个 xml 文件是否是独立的 xml 文件





### 元素



![image-20210123011502329](../picture/JavaWeb%E7%AC%94%E8%AE%B0/image-20210123011502329.png)



**命名规则：**

- 名称可以含字母、 数字以及其他的字符  
- 名称不能以数字或者标点符号开始  
- 名称不能包含空格  
- xml 中的元素（ 标签） 也 分成 单标签和双标签：  



### xml属性



用来提供元素的额外信息

每个属性的值**必须**使用 **引号** 引起来。  





### 语法规则



- 所有 XML 元素都须有关闭标签（也就是闭合）  
- XML 标签对大小写敏感  
- XML 必须正确地嵌套  
- XML 文档必须有根元素  （**顶级元素 -> 没有父标签   唯一一个**） `multiple root tags`
- XML 中的特殊字符  
- 文本区域（ CDATA 区）  

CDATA 语法可以告诉 xml 解析器， **CDATA 里的文本内容， 只是纯文本， 不需要 xml 语法解析**  

`<![CDATA[ 这里可以把你输入的字符原样显示， 不会解析 xml ]]>  `





## XML的解析



不管是 html 文件还是 xml 文件它们都是**标记型文档**， 都可以使用 w3c 组织制定的 **dom 技术来解析**。  

![image-20210123013059720](../picture/JavaWeb%E7%AC%94%E8%AE%B0/image-20210123013059720.png)

标签都会被解析为dom

document对象表示的是整个文档



> dom 解析技术是 W3C 组织制定的， 而所有的编程语言都对这个解析技术使用了自己语言的特点进行实现。
> Java 对 dom 技术解析标记也做了实现。
> sun 公司在 JDK5 版本对 dom 解析技术进行升级： SAX（ Simple API for XML ）
> SAX 解析， 它跟 W3C 制定的解析不太一样。 它是以类似事件机制通过回调告诉用户当前正在解析的内容。
> 它是**一行一行的读取** xml 文件进行解析的。 不会创建大量的 dom 对象。
> 所以它在解析 xml 的时候， 在内存的使用上和性能上。 都优于 Dom 解析。  



第三方的解析：
jdom **在 *dom* 基础上进行了封装** 
**dom4j 又对 jdom 进行了封装。**
pull 主要用在 Android 手机开发， 是在跟 sax 非常类似都是事件机制解析 xml 文件。
这个 Dom4j 它是第三方的解析技术。 我们需要使用第三方给我们提供好的**类库**才可以解析 xml 文件。  





## dom4j解析技术



```java
public void readXML() throws DocumentException {
    // 需要分四步操作:

    // 第一步， 通过创建 SAXReader 对象。 来读取 xml 文件， 获取 Document 对象
    SAXReader reader = new SAXReader();
    Document document = reader.read("src/books.xml");
    // 第二步， 通过 Document 对象。 拿到 XML 的根元素对象
    Element root = document.getRootElement();
    // 打印测试
    // Element.asXML() 它将当前元素转换成为 String 对象
    // System.out.println( root.asXML() );
    // 第三步， 通过根元素对象。 获取所有的 book 标签对象
    // Element.elements(标签名)它可以拿到当前元素下的指定的子元素的集合
    List<Element> books = root.elements("book");
    // 第四步， 遍历每个 book 标签对象。 然后获取到 book 标签对象内的每一个元素，
    for (Element book : books) {
        // 测试
        // System.out.println(book.asXML());
        // 拿到 book 下面的 name 元素对象
        Element nameElement = book.element("name");
        // 拿到 book 下面的 price 元素对象
        Element priceElement = book.element("price");
        // 拿到 book 下面的 author 元素对象
        Element authorElement = book.element("author");
        // 再通过 getText() 方法拿到起始标签和结束标签之间的文本内容
        System.out.println("书名" + nameElement.getText() + " , 价格:"
        + priceElement.getText() + ", 作者： " + authorElement.getText());
    }
}
```



# Tomcat服务器



JavaWeb 是指所有通过 Java 语言编写可以**通过浏览器访问**的程序的总称， 叫 JavaWeb。

JavaWeb 是基于请求和响应来开发的。  



Request：客户端给服务器发送数据

Response：服务器给客户端回传数据

**成对出现**





web资源分为：

- 静态资源：html、 css、 js、 txt、 mp4 视频 ,、jpg 图片  
- 动态资源：jsp 页面、 Servlet 程序  



## web服务器



Tomcat： 由 Apache 组织提供的一种 Web 服务器， 提供**对 jsp 和 Servlet 的支持**。 它是一种轻量级的 javaWeb 容器（服务
器） ， 也是当前**应用最广的 JavaWeb 服务器**（免费） 。  



Jboss： 是一个遵从 JavaEE 规范的、 开放源代码的、 纯 Java 的 EJB 服务器， 它支持所有的 JavaEE 规范（免费） 。



GlassFish： 由 Oracle 公司开发的一款 JavaWeb 服务器， 是一款强健的商业服务器， 达到产品级质量（应用很少） 。



Resin： 是 CAUCHO 公司的产品， 是一个非常流行的服务器， 对 servlet 和 JSP 提供了良好的支持，性能也比较优良， resin 自身采用 JAVA 语言开发（收费， 应用比较多） 。



WebLogic： 是 Oracle 公司的产品， 是目前应用最广泛的 Web 服务器， 支持 JavaEE 规范，而且不断的完善以适应新的开发要求， 适合大型项目（收费， 用的不多， 适合大公司） 。  





## Tomcat



Servlet 程序从 2.5 版本是现在世面使用最多的版本（xml 配置）
到了 Servlet3.0 之后。 就是注解版本的 Servlet 使用  



> tomcat9.0.41  没法使用jdk8   可以使用jdk12



work是 Tomcat 工作时的目录， 用来存放 Tomcat 运行时 jsp 翻译为 Servlet 的源码， 和 Session 钝化的目录。



启动：

- `catalina run`
- 双击`startup.bat`



## 部署方法



1. 放到webapps目录下

2. 找到 Tomcat 下的 conf 目录\Catalina\localhost\ 下,创建如下的配置文件：  

   ```xml
   <!-- 
   Context 表示一个工程上下文
   path 表示工程的访问路径:/dt
   docBase 表示你的工程目录在哪里
   -->
   <Context path="/dt" docBase="D:\desktop" />
   ```

   **重要的是：！！！xml文件的名字应该就是这个path的名字**

![image-20210123043730324](../picture/JavaWeb%E7%AC%94%E8%AE%B0/image-20210123043730324.png)





---

手托 html 页面到浏览器和在浏览器中输入 http://ip:端口号/工程名/访问的区别  



![image-20210123161406829](../picture/JavaWeb%E7%AC%94%E8%AE%B0/image-20210123161406829.png)

![image-20210123161418025](../picture/JavaWeb%E7%AC%94%E8%AE%B0/image-20210123161418025.png)



使用的是 `file://协议`



`http://192.168.0.106:8088/dt/ClassTableTry.jsp`  则使用的是http协议

![image-20210123161549618](../picture/JavaWeb%E7%AC%94%E8%AE%B0/image-20210123161549618.png)







## 创建web工程



![image-20210123162754345](../picture/JavaWeb%E7%AC%94%E8%AE%B0/image-20210123162754345.png)





![image-20210123162846122](../picture/JavaWeb%E7%AC%94%E8%AE%B0/image-20210123162846122.png)



![image-20210123164511246](../picture/JavaWeb%E7%AC%94%E8%AE%B0/image-20210123164511246.png)



**启动后默认访问的地址**

![image-20210123164536569](../picture/JavaWeb%E7%AC%94%E8%AE%B0/image-20210123164536569.png)



启动服务器，自动打开index页面

![image-20210123170039275](../picture/JavaWeb%E7%AC%94%E8%AE%B0/image-20210123170039275.png)





**有修改时热部署**    在程序发生变化时，部署在tomcat上的web项目也同时更新  刷新页面即可产生变化

![image-20210123170641679](../picture/JavaWeb%E7%AC%94%E8%AE%B0/image-20210123170641679.png)



也可以修改端口号

![image-20210123170535339](../picture/JavaWeb%E7%AC%94%E8%AE%B0/image-20210123170535339.png)









# Servlet

1、 Servlet 是 JavaEE 规范之一。 **规范就是接口**
2、 Servlet 就 JavaWeb **三大组件**之一。 三大组件分别是： Servlet 程序、 Filter 过滤器、 Listener 监听器。
3、 Servlet 是运行在**服务器上的一个 java 小程序**， 它可以**接收客户端发送过来的请求， 并*响应* 数据*给客户端***  



## servlet基础





### 手动实现servlet程序



- 编写一个类去实现这个接口
- 实现 service 方法， 处理请求， 并响应数据  
- 到 web.xml 中去**配置 servlet 程序的访问地址**  





```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee
http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
version="4.0">
    
<!-- servlet 标签给 Tomcat 配置 Servlet 程序 -->
<servlet>
    <!--servlet-name 标签 Servlet 程序起一个别名（一般是类名） -->
    <servlet-name>HelloServlet</servlet-name>
    <!--servlet-class 是 Servlet 程序的全类名-->
    <servlet-class>webDir1.HelloServlet</servlet-class>
</servlet>
    
<!--servlet-mapping 标签给 servlet 程序配置访问地址-->
<servlet-mapping>
    <!--servlet-name 标签的作用是告诉服务器， 我当前配置的地址给哪个 Servlet 程序使用-->
    <servlet-name>HelloServlet</servlet-name>
    
    <!--url-pattern 标签配置访问地址 <br/>
    / 斜杠在服务器解析的时候， 表示地址为： http://ip:port/工程路径 <br/>
    /hello 表示地址为： http://ip:port/工程路径/hello <br/>
    -->
    <url-pattern>/hello</url-pattern>
</servlet-mapping>
</web-app>
```

**斜杠在服务器解析的时候， 表示地址为： `http://ip:port/`工程路径**





### 常见错误



url-pattern 中配置的路径没有**以斜杠打头**。  

如果不以  `/hello`   **斜杠打头**，

![image-20210123184506120](../picture/JavaWeb%E7%AC%94%E8%AE%B0/image-20210123184506120.png)



---



servlet-name 配置的值不存在  

**servlet-mapping中的name值**要和**servlet中的name值**一样

![image-20210123184524916](../picture/JavaWeb%E7%AC%94%E8%AE%B0/image-20210123184524916.png)





---



servlet-class 标签的全类名配置错误



 ### url地址到servlet程序的访问

![image-20210123193859939](../picture/JavaWeb%E7%AC%94%E8%AE%B0/image-20210123193859939.png)





### servlet的生命周期



1、 执行 Servlet 构造器方法
2、 执行 init 初始化方法
		第一、 二步， 是在**第一次访问**， 的时候**创建 Servlet 程序会调用**。
3、 执行 service 方法
		第三步， 每次访问都会调用。
4、 执行 destroy 销毁方法
		第四步， 在 web 工程停止的时候调用。  

![image-20210123194707411](../picture/JavaWeb%E7%AC%94%E8%AE%B0/image-20210123194707411.png)





### get/post请求的分发处理

**来有效区分post和get请求**





![image-20210123195341370](../picture/JavaWeb%E7%AC%94%E8%AE%B0/image-20210123195341370.png)







```java
HttpServletRequest httpServletRequest = (HttpServletRequest) servletRequest;
//获取请求的方式
String method = httpServletRequest.getMethod();
System.out.println(method);
```

![image-20210123195708170](../picture/JavaWeb%E7%AC%94%E8%AE%B0/image-20210123195708170.png)







```java
if ("GET".equals(method)) {
	doGet();
} else if ("POST".equals(method)) {
	doPost();
}

/**
* 做 get 请求的操作
*/
public void doGet(){
    System.out.println("get 请求");
    System.out.println("get 请求");
} 
/**
* 做 post 请求的操作
*/
public void doPost(){
    System.out.println("post 请求");
    System.out.println("post 请求");
}
```





### 通过继承 HttpServlet 实现 Servlet 程序  





> 要实现此接口，可以编写一个扩展 `javax.servlet.GenericServlet` 的一般  servlet，或者编写一个扩展 `javax.servlet.http.HttpServlet` 的 HTTP servlet。 

一般在实际项目开发中， 都是使用继承 HttpServlet 类的方式去实现 Servlet 程序。  



1、 编写一个类去继承 HttpServlet 类
2、 **根据业务**需要**重写 doGet 或 doPost 方法**
3、 到 web.xml 中的配置 Servlet 程序的访问地址  





```java
public class HelloServlet2 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("helloservlet2  doget!!!");
    }
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("helloservlet2  dopost!!!");
    }
}
```







### 使用idea创建servlet程序



直接创建servlet程序

![image-20210123214536896](../picture/JavaWeb%E7%AC%94%E8%AE%B0/image-20210123214536896.png)





直接使用注解配置name和url路径

```java
@WebServlet(name = "demo1", urlPatterns = {"/demo"})
public class ServletDemo1 extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        System.out.println("OHHHHHHHHHH post");
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        System.out.println("OHHHHHHHHHH get");
    }
}
```





> 如果不勾选  使用注解配置：会在web.xml中自动写入servlet标签  再添加一个serlvet-mapping就行







## Servlet类的继承体系



```java
public interface Servlet 
public interface ServletConfig 
public abstract class GenericServlet implements Servlet, ServletConfig, Serializable
public abstract class HttpServlet extends GenericServlet
public class ServletDemo1 extends HttpServlet
```



![image-20210123222328437](../picture/JavaWeb%E7%AC%94%E8%AE%B0/image-20210123222328437.png)



如果不进行重写do方法，那么在接收到get/post请求时   就**会抛异常**

![image-20210123223325383](../picture/JavaWeb%E7%AC%94%E8%AE%B0/image-20210123223325383.png)





## ServletConfig类



servlet程序的配置信息类



Servlet 程序和 ServletConfig 对象都是**由 Tomcat 负责创建**， 我们负责使用。
Servlet 程序默认是第一次访问的时候创建， **ServletConfig 是每个 Servlet 程序创建时**， 就创建一个**对应的 ServletConfig 对象。**

**每一个servletconfig对应的是自己servlet类所对应的config对象**



**作用**：

1、 可以获取 Servlet 程序的别名 servlet-name 的值
2、 获取初始化参数 init-param
3、 获取 ServletContext 对象  

```java
    @Override
    public void init(ServletConfig servletConfig) throws ServletException {
        System.out.println("init初始化方法");
        System.out.println(servletConfig.getServletName());
        System.out.println(servletConfig.getInitParameter("username"));
        System.out.println(servletConfig.getServletContext());
    }
/*
HelloSer     这个是在xml文件中配置的那个name，并非类名
crhuang
org.apache.catalina.core.ApplicationContextFacade@20fa3314
*/

```

```xml
<servlet>
        <servlet-name>HelloSer</servlet-name>
        <servlet-class>webDir1.HelloServlet</servlet-class>
        <init-param>
            <param-name>username</param-name>
            <param-value>crhuang</param-value>
        </init-param>
        <init-param>
            <param-name>psw</param-name>
            <param-value>123456</param-value>
        </init-param>
    </servlet>
```





## ServletContext



1、 ServletContext 是一个接口， 它表示 Servlet **上下文**对象
2、 一个 web 工程， **只有一个 ServletContext 对象实例**。
3、 ServletContext 对象是一个**域对象**。
4、 ServletContext 是在 web 工程部署启动的时候创建。 在 web 工程停止的时候销毁。  



**什么是域对象**

可以像Map一样存取数据的对象。

这里的域，指的是存取数据的操作范围，整个 web 工程。  

- 存数据：setAttribute()
- 取数据：getAttribute()
- 删除数据：removeAttribute()



### ServletContext作用



1、 获取 web.xml 中配置的**上下文参数 context-param**
2、 获取当前的工程路径， 格式: /工程路径
3、 获取工程部署后在服务器硬盘上的绝对路径
4、 像 Map一样存取数据  







```java
ServletContext context = getServletConfig().getServletContext();
//不能得到servletconfig中配置的属性信息，二者无关
System.out.println(context.getInitParameter("username"));//HuangCR

System.out.println(context.getContextPath());// 工程路径： /webexer

//获取工程部署后在服务器硬盘上的 绝对 路径
/**
*      / 斜杠被服务器解析地址为:http://ip:port/工程名/ 映射到 IDEA 代码的 web 目录
*/

//D:\desktop\Code\Exercise\webexer\target\webexer-1.0-SNAPSHOT\
System.out.println(context.getRealPath("/"));
//D:\desktop\Code\Exercise\webexer\target\webexer-1.0-SNAPSHOT\css
System.out.println(context.getRealPath("/css"));
```

![image-20210124021621874](../picture/JavaWeb%E7%AC%94%E8%AE%B0/image-20210124021621874.png)



```xml
<!--  context-param 是上下文参数(它属于整个 web 工程)-->
<context-param>
    <param-name>username</param-name>
    <param-value>context</param-value>
</context-param>

    <context-param>
    <param-name>password</param-name>
    <param-value>root</param-value>
</context-param>
```





这是一个web工程模块

![image-20210124022142507](../picture/JavaWeb%E7%AC%94%E8%AE%B0/image-20210124022142507.png)







---

ServletContext 像 Map 一样存取数据  



一个 web 工程， **只有一个 ServletContext 对象实例**。



**ServletContext对象在工程部署启动的时候创建。只要*部署后*，*往里面存放了数据*，在哪个程序中/哪个位置都可以取出来（包括刷新后，也可以）**



> 这里的域，指的是存取数据的操作**范围**，**整个 web 工程。**  

```java
// 获取 ServletContext 对象
ServletContext context = getServletContext();
System.out.println(context);
System.out.println("保存之前: Context1 获取 key1 的值是:"+ context.getAttribute("key1"));
context.setAttribute("key1", "value1");
System.out.println("Context1 中获取域数据 key1 的值是:"+ context.getAttribute("key1"));


//在另一个servlet程序中：
System.out.println(context);//只有一个ServletContext对象
System.out.println("Context2 中获取域数据 key1 的值是:"+ context.getAttribute("key1"));
```



## HTTP协议



客户端和服务器之间通信时，发送的数据，需要遵守的规则



请求分为get  post请求





### 请求的http格式



#### get请求

1. 请求行

   请求的方式 GET
   请求的资源路径[+?+请求参数]
   请求的协议的版本号 HTTP/1.1  

2. 请求头

   key : value 组成 不同的键值对， 表示不同的含义。  

![image-20210124024511473](../picture/JavaWeb%E7%AC%94%E8%AE%B0/image-20210124024511473.png)



**发送给服务器的数据是直接写在请求行中的**

![image-20210124030040116](../picture/JavaWeb%E7%AC%94%E8%AE%B0/image-20210124030040116.png)



![image-20210124030119067](../picture/JavaWeb%E7%AC%94%E8%AE%B0/image-20210124030119067.png)



![image-20210124024547235](../picture/JavaWeb%E7%AC%94%E8%AE%B0/image-20210124024547235.png)





#### post请求

1. 请求行

   请求的方式 POST
   请求的资源路径[+?+请求参数]
   请求的协议的版本号 HTTP/1.1  

2. 请求头

   key : value 组成 不同的键值对， 表示不同的含义。 

   **空行**

3. 请求体  -> 就是**发送给服务器的数据**  

 

![image-20210124025724440](../picture/JavaWeb%E7%AC%94%E8%AE%B0/image-20210124025724440.png)



![image-20210124025905831](../picture/JavaWeb%E7%AC%94%E8%AE%B0/image-20210124025905831.png)





### 常用请求头



Accept: 表示客户端可以接收的数据类型
Accpet-Languege: 表示客户端可以接收的语言类型
User-Agent: 表示客户端浏览器的信息
Host： 表示请求时的服务器 ip 和端口号  



### 哪些是get/post请求



GET 请求有哪些：
1、 form 标签 method=get
2、 a 标签
3、 link 标签引入 css
4、 Script 标签引入 js 文件
5、 img 标签引入图片
6、 iframe 引入 html 页面
7、 **在浏览器地址栏中输入地址后敲回车**



POST 请求有哪些：
8、 form **表单**标签 **method=post**  









### 响应的http格式



1、 响应行
		(1) 响应的协议和版本号
		(2) 响应状态码
		(3) 响应状态描述符
2、 响应头

​		key : value	   不同的响应头， 有其不同含义

3、 响应体 ---->>> 就是回传给客户端的数据



![image-20210124034258466](../picture/JavaWeb%E7%AC%94%E8%AE%B0/image-20210124034258466.png)



![image-20210124034949243](../picture/JavaWeb%E7%AC%94%E8%AE%B0/image-20210124034949243.png)





### 常用的响应码



200 表示请求成功
302 表示请求**重定向**（明天讲）
404 表示请求服务器已经收到了， 但是你要的**数据不存在**（请求**地址错误**）
500 表示服务器已经收到请求， 但是服务器**内部错误**（代码错误）  

![image-20210124035150028](../picture/JavaWeb%E7%AC%94%E8%AE%B0/image-20210124035150028.png)





### MIME类型说明



MIME 是 HTTP 协议中**数据类型**。
MIME 的英文全称是"Multipurpose Internet Mail Extensions" **多功能 Internet 邮件扩充服务**。 MIME 类型的格式是“**大类型/小类型**” ， 并与某一种文件的**扩展名**相对应。  



![image-20210124041705155](../picture/JavaWeb%E7%AC%94%E8%AE%B0/image-20210124041705155.png)

![image-20210124041715164](../picture/JavaWeb%E7%AC%94%E8%AE%B0/image-20210124041715164.png)





## HttpServletRequest类







每次**只要有请求进入 Tomcat 服务器**， Tomcat 服务器就会把请求过来的 HTTP 协议信息**解析好封装到 Request 对象**中。
然后**传递到 service 方法**（ doGet 和 doPost） 中给我们使用。 我们可以**通过 HttpServletRequest 对象， 获取到所有请求的信息。**  





### 获取请求参数

i. getRequestURI() 				获取请求的**资源路径**

ii. getRequestURL() 			  获取请求的统一资源定位符（**绝对路径**）

iii. getRemoteHost() 			获取客户端的 ip 地址

iv. getHeader() 					获取请求头

v. getParameter() 				获取请求的参数
vi. getParameterValues() 	获取请求的参数**（多个值的时候使用）**
vii. getMethod() 					获取请求的方式 GET 或 POST
viii. setAttribute(key, value); 设置域数据
ix. getAttribute(key); 			获取域数据
x. getRequestDispatcher()   获取请求转发对象





```java
System.out.println("get");
System.out.println(request.getHeader("Connection"));
System.out.println(request.getRequestURI());//   /webexer/demo
System.out.println(request.getRequestURL());//   http://localhost:8088/webexer/demo
System.out.println(request.getRemoteHost());//   0:0:0:0:0:0:0:1  使用真实ip访问： 192.168.0.106
//使用手机访问：192.168.0.102

/*
192.168.0.102
神魔恋
手机也可以连接这个服务器
 */
System.out.println(request.getParameter("username"));//获得表单提交的参数
```



### post请求中文乱码



doGet 请求的**中文乱码**解决：  



```java
// 获取请求参数
String username = req.getParameter("username");
//1 先以 iso8859-1 进行编码
//2 再以 utf-8 进行解码
username = new String(username.getBytes("iso-8859-1"), "UTF-8");
```

----

POST 请求的中文乱码解决  





```java
// 设置请求体的字符集为 UTF-8， 从而解决 post 请求的中文乱码问题   不设置会出现中文乱码
req.setCharacterEncoding("UTF-8");
System.out.println("-------------doPost------------");
// 获取请求参数
String username = req.getParameter("username");
String password = req.getParameter("password");
String[] hobby = req.getParameterValues("hobby");
System.out.println("用户名： " + username);
System.out.println("密码： " + password);
System.out.println("兴趣爱好： " + Arrays.asList(hobby));
```







### 请求的转发



请求转发是指， 服务器收到请求后， 从一个资源跳转到另一个资源的操作叫请求转发  



![image-20210124194220872](../picture/JavaWeb%E7%AC%94%E8%AE%B0/image-20210124194220872.png)



**浏览器地址栏没有变化**

**一次请求**

 2还要经过1，然后回到客户端

**可以跳转到web-inf目录中的资源**



**给的url地址是从web工程下开始寻找路径，所以不能访问工程以外的资源**



```java
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    System.out.println("demo1  doget");
    String username = request.getParameter("username");
    System.out.println("username in demo1 " + username);

    //给材料改一个章，并传递到servletdemo2中
    request.setAttribute("key", "柜台1的章");

    //问路？
    RequestDispatcher requestDispatcher = request.getRequestDispatcher("/demo2");

    //走向demo2
    requestDispatcher.forward(request, response);
}


protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    System.out.println("demo2 doget");
    String username = request.getParameter("username");
    System.out.println("username in demo2 " + username);

    Object key = request.getAttribute("key");
    System.out.println("demo1的盖章??" + key);

    System.out.println("demo2 do something");
}
```



### base标签



正常的跳转href：

![image-20210124210952852](../picture/JavaWeb%E7%AC%94%E8%AE%B0/image-20210124210952852.png)









![image-20210124210754827](../picture/JavaWeb%E7%AC%94%E8%AE%B0/image-20210124210754827.png)



这是请求转发后进入的页面，***地址不变***

![image-20210124210857184](../picture/JavaWeb%E7%AC%94%E8%AE%B0/image-20210124210857184.png)



**相对于当前浏览器地址！！**

![image-20210124211022928](../picture/JavaWeb%E7%AC%94%E8%AE%B0/image-20210124211022928.png)





```html
<base href="http://192.168.0.106:8088/webexer/css/a/b/">
<base href="/webexer/css/a/b/">   这个也可以。需要从工程名开始
```

> 最后一个/一定要有，否则会认为b是一个资源



**之所以跳不回去，是因为参照的地址没有发生变化**



base标签使它参照的地址不根据浏览器地址而变化 ，**设置页面  *相对路径* 工作时参照的地址**







## HttpServletResponse类



HttpServletResponse 类和 HttpServletRequest 类一样。 每次请求进来， Tomcat 服务器都会创建一个 Response 对象传
递给 Servlet 程序去使用。 HttpServletRequest 表示请求过来的信息， HttpServletResponse 表示所有响应的信息



我们如果需要设置**返回给客户端的信息**， 都可以**通过 HttpServletResponse 对象来进行设置**  



字节流			getOutPutStream()			常用于下载（传递二进制数据)

字符流			getWriter()							常用于回传字符串（常用）



两个流同时**只能使用一个**。
使用了字节流， 就不能再使用字符流， 反之亦然， 否则就会报错。  

同时使用了两个响应流

![image-20210124224738805](../picture/JavaWeb%E7%AC%94%E8%AE%B0/image-20210124224738805.png)





### 往客户端回传数据



```java
PrintWriter printWriter = response.getWriter();
printWriter.write("response content!!");//显示在浏览器页面上，返回的数据
```





### 解决响应中文乱码







还要调整浏览器的字符集格式   Chrome默认GBK

![image-20210124225518303](../picture/JavaWeb%E7%AC%94%E8%AE%B0/image-20210124225518303.png)





```java
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    System.out.println("demo1  doget");

    System.out.println(response.getCharacterEncoding());//ISO-8859-1  不支持中文
    response.setCharacterEncoding("UTF-8");//鏉庡湪骞茬榄�!!

    //通过响应头，设置浏览器也使用utf-8字符集
    response.setHeader("Content-Type", "text/html;charset=UTF-8");

    PrintWriter printWriter = response.getWriter();
    printWriter.write("李在干神魔!!");
}
```





![image-20210124231008824](../picture/JavaWeb%E7%AC%94%E8%AE%B0/image-20210124231008824.png)



---



```java
//同时设置服务器和客户端都使用utf-8，还设置了响应头
//此方法一定要在获取流对象之前调用  才有效
response.setContentType("text/html;charset=UTF-8");

System.out.println(response.getCharacterEncoding());//UTF-8
PrintWriter printWriter = response.getWriter();
printWriter.write("李在干神魔!!");
```







### 请求重定向





请求重定向， 是指**客户端给服务器发请求**， 然后服务器告诉客户端说： 我给你一些地址， **你去新地址访问**。 叫请求重定向（因为**之前的地址可能已经被废弃**） 。  



![image-20210124232603299](../picture/JavaWeb%E7%AC%94%E8%AE%B0/image-20210124232603299.png)







```java
System.out.println("response demo1  here!!");
//设置响应状态码302，表示重定向
response.setStatus(302);

//不共享request域中的数据
request.setAttribute("key1", "v1");

//设置响应头  说明新的地址
response.setHeader("Location", "/webexer/demo2");//或者http://localhost:8088/webexer/demo2  或者  demo2  或者/webexer/demo2

//这里的 /斜杠，是交给浏览器来解析：http://localhost:8080，所以需要写出工程名
```





请求demo？返回302 重定向到demo2？返回成功！200

![image-20210125004941661](../picture/JavaWeb%E7%AC%94%E8%AE%B0/image-20210125004941661.png)

![image-20210125005058797](../picture/JavaWeb%E7%AC%94%E8%AE%B0/image-20210125005058797.png)





> 特点：
>
> - 浏览器地址栏会发生变化
> - 是两次请求（请求转发是一次）
> - 不共享request域中的数据（`requestDispatcher.forward(request, response);`，和请求转发不一样，不传request和response对象，也就不共享数据域）
> - **不能访问WEB-INF下的资源**：浏览器得到重定向地址，归根结底，还是 **浏览器发的请求，受保护，还是无法进入web-inf目录**
>   - **请求转发是  服务器发的请求，所以可以进入web-inf目录**
> - **可以访问工程外的资源**（一定要加http://....）





---

请求重定向第二种方法



```java
response.sendRedirect("/webexer/css/a/b/a.html");
```

















# Tips



## idea-tomcat控制台中文乱码问题

![image-20210123184005079](../picture/JavaWeb%E7%AC%94%E8%AE%B0/image-20210123184005079.png)

`-Dfile.encoding=UTF-8`



并要保证setting-file encoding中设置均为UTF-8编码





## service和doget/post关系

如果一个httpservlet的子类同时override了service和dopost/get方法，那么**只会调用service方法**



> 因为在HttpServlet中 service()方法 **实现了请求的分发处理**
>
> ```java
> String method = req.getMethod();
> ```





## httpservlet继承类 重写init方法



```java
//javax.servlet.GenericServlet#config
private transient ServletConfig config;

//javax.servlet.GenericServlet#init(javax.servlet.ServletConfig)
public void init(ServletConfig config) throws ServletException {
    this.config = config;
    this.init();
}

@Override
public void init(ServletConfig config) throws ServletException {
    super.init(config);
}

//config对象是通过init进行初始化
//javax.servlet.GenericServlet#getServletConfig
public ServletConfig getServletConfig() {
    return this.config;
}
```

**需要调用super.init(config)方法**

如果对init进行重写，就会导致ServletConfig**没有进行初始化**







## 路径名称



![image-20210124214947836](../picture/JavaWeb%E7%AC%94%E8%AE%B0/image-20210124214947836.png)

一定要加http://才能正确定位





## web中 / 斜杠的不同意义



/ 斜杠           

- 如果被**浏览器解析**， 得到的地址是： http://ip:port/       就只是在当前服务器port：8088下来进行跳转，所以默认就是`http://localhost:8088`

  ```html
  <base href="/webexer/css/a/b/">   <!--在html文件中，由浏览器解析-->
  ```

- 如果被**服务器解析 => 就是在java文件中（因为java文件是交给服务器来运行的，服务器知道当前的工程目录）**， 得到的地址是： http://ip:port/**工程路径**  

  1、 `<url-pattern>/servlet1</url-pattern>`
  2、 `servletContext.getRealPath(“/”)`
  3、 `request.getRequestDispatcher(“/”)`  





特殊情况： response.sendRediect(“/”); （**请求重定向**）把  **斜杠发送给浏览器解析**。 得到 http://ip:port/  





---



示例1:

```html
<a href="/css/a/b/a.html">进去</a>
```

因为这里的 **/斜杠**  代表的就是`http://ip:0888`

去掉  /   或者使用 `<a href="./css/a/b/a.html">进去</a>`     success~ 

![image-20210125004837621](../picture/JavaWeb%E7%AC%94%E8%AE%B0/image-20210125004837621.png)

---



示例2



```java
response.setHeader("Location", "www.baidu.com");//这个不行  一定要加http://
response.setHeader("Location", "demo2");//这个可以

//一旦加了“/demo2”，那么浏览器就会从localhost:8080开始进入/demo2  如果不加，就默认为在本工程下面寻找的demo2文件，就可以找到映射的servlet
```