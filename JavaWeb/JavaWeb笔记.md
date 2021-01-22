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



# JavaWeb



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















