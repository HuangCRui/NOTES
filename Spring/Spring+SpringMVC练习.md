# spring+springmvc练习



## 搭建环境



![image-20210205044441110](../picture/Spring+SpringMVC%E7%BB%83%E4%B9%A0/image-20210205044441110.png)



![image-20210205044510286](../picture/Spring+SpringMVC%E7%BB%83%E4%B9%A0/image-20210205044510286.png)





![image-20210205044414774](../picture/Spring+SpringMVC%E7%BB%83%E4%B9%A0/image-20210205044414774.png)





applicationContext

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
http://www.springframework.org/schema/context
http://www.springframework.org/schema/context/spring-context.xsd
http://www.springframework.org/schema/mvc
http://www.springframework.org/schema/mvc/spring-mvc.xsd
">


<!--    数据源-->
    <context:property-placeholder location="classpath:jdbc.properties"/>

<!--    配置数据源对象-->
    <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
        <property name="driverClass" value="${jdbc.Driver}"/>
        <property name="jdbcUrl" value="${jdbc.url}"/>
        <property name="user" value="${jdbc.user}"/>
        <property name="password" value="${jdbc.password}"/>
    </bean>

<!--    jdbc模板对象-->
    <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
        <property name="dataSource" ref="dataSource"/>
    </bean>

</beans>
```



spring-mvc.xml

```xml
<!--    mvc注解驱动-->
    <mvc:annotation-driven/>

<!--    内部资源视图解析器-->
    <bean id="viewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/pages/"/>
        <property name="suffix" value=".jsp"/>
    </bean>

<!--    静态资源权限开放-->
    <mvc:default-servlet-handler/>
```



web.xml

```xml
<!--  配置监听器-->
  <listener>
    <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
  </listener>

<!--  全局初始化参数-->
  <context-param>
    <param-name>contextConfigLocation</param-name>
    <param-value>classpath:applicationContext.xml</param-value>
  </context-param>


<!--  springmvc的前端控制器-->
  <servlet>
    <servlet-name>dispatcherServlet</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <init-param>
      <param-name>contextConfigLocation</param-name>
      <param-value>classpath:spring-mvc.xml</param-value>
    </init-param>
    <load-on-startup>1</load-on-startup>
  </servlet>
  <servlet-mapping>
    <servlet-name>dispatcherServlet</servlet-name>
    <url-pattern>/</url-pattern>
  </servlet-mapping>
```







## 用户表和角色表的分析





每个用户都要有角色，角色不一样，权限不一样



都是多对多的关系：

- 一个角色有多个用户
- 一个用户可以对应多个角色



**用户(主表)——角色(主表)  中间表，维护用户id和角色id**



![image-20210205160051466](../picture/Spring+SpringMVC%E7%BB%83%E4%B9%A0/image-20210205160051466.png)



![image-20210205160248709](../picture/Spring+SpringMVC%E7%BB%83%E4%B9%A0/image-20210205160248709.png)







## 角色列表展示





![image-20210205160805306](../picture/Spring+SpringMVC%E7%BB%83%E4%B9%A0/image-20210205160805306.png)



> 完成该功能的思路和步骤为：
>
> ①点击角色管理菜单发送请求到服务器端（修改角色管理菜单的url地址）
>
> ②创建RoleController和list()方法
>
> ③创建RoleService和list()方法
>
> ④创建RoleDao和findAll()方法
>
> ⑤使用JdbcTemplate完成查询操作
>
> ⑥将查询数据存储到modelAndView中
>
> ⑦转发到role-list.jsp页面进行展示

修改链接地址，指向controller对应方法的url地址

![image-20210205181430118](../picture/Spring+SpringMVC%E7%BB%83%E4%B9%A0/image-20210205181430118.png)



Controller层

```java
@RequestMapping("/role")
@Controller
public class RoleController {

    @Autowired
    private RoleService roleService;

    @RequestMapping("/list")
    public ModelAndView list(){
        ModelAndView modelAndView = new ModelAndView();
        List<Role> roleList = roleService.list();
        //设置模型对象
        modelAndView.addObject("roleList", roleList);
        //设置视图
        modelAndView.setViewName("role-list");

        return modelAndView;
    }
}
```



service层



```java
@Service
public class RoleServiceImpl implements RoleService {

    @Autowired
    private RoleDao roleDao;

    @Override
    public List<Role> list() {
        List<Role> roleList = roleDao.findAdd();
        return roleList;
    }
}
```



dao层



```java
@Repository
public class RoleDaoImpl implements RoleDao {

    @Autowired
    private JdbcTemplate template;

    @Override
    public List<Role> findAdd() {
        List<Role> roleList = template.query("select * from sys_role", new BeanPropertyRowMapper<Role>(Role.class));
        return roleList;
    }
}
```



**配置组件扫描**



在role-list.jsp中将数据取出来并展示

```html
<c:forEach items="${roleList}" var="role">
    <tr>
        <td><input name="ids" type="checkbox"></td>
        <td>${role.id}</td>
        <td>${role.roleName}</td>
        <td>${role.roleDesc}</td>
        <td class="text-center">
            <a href="javascript:void(0);" class="btn bg-olive btn-xs">删除</a>
        </td>
    </tr>
</c:forEach>
```









## 角色添加





![image-20210205182254346](../picture/Spring+SpringMVC%E7%BB%83%E4%B9%A0/image-20210205182254346.png)



> 操作步骤如下：
>
> ①点击列表页面新建按钮跳转到角色添加页面
>
> ②输入角色信息，点击保存按钮，表单数据提交服务器
>
> ③编写RoleController的save()方法
>
> ④编写RoleService的save()方法
>
> ⑤编写RoleDao的save()方法
>
> ⑥使用JdbcTemplate保存Role数据到sys_role
>
> ⑦跳转回角色列表页面



**根据name自动封装到pojo对象中**

![image-20210205183106839](../picture/Spring+SpringMVC%E7%BB%83%E4%B9%A0/image-20210205183106839.png)





controller：

```java
@RequestMapping("/save")
public String save(Role role){
    roleService.save(role);
    return "redirect:/role/list";
}
```



service:

```java
@Override
public void save(Role role) {
    roleDao.sace(role);
}
```



dao:

```java
@Override
public void save(Role role) {
    template.update("insert into sys_role values(?,?,?)", null, role.getRoleName(), role.getRoleDesc());
}
```



---



出现乱码？

**没有配置filter，表单读取到的中文不是utf-8格式的字符，变成乱码**

![image-20210205183904396](../picture/Spring+SpringMVC%E7%BB%83%E4%B9%A0/image-20210205183904396.png)





post方法有乱码问题

![image-20210205184035057](../picture/Spring+SpringMVC%E7%BB%83%E4%B9%A0/image-20210205184035057.png)





使用get：

![image-20210205184140205](../picture/Spring+SpringMVC%E7%BB%83%E4%B9%A0/image-20210205184140205.png)





![image-20210205184149600](../picture/Spring+SpringMVC%E7%BB%83%E4%B9%A0/image-20210205184149600.png)





```xml
<filter>
  <filter-name>characterEncodingFilter</filter-name>
  <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
  <init-param>
    <param-name>encoding</param-name>
    <param-value>UTF-8</param-value>
  </init-param>
</filter>
<filter-mapping>
  <filter-name>characterEncodingFilter</filter-name>
  <url-pattern>/*</url-pattern>
</filter-mapping>
```







## 用户列表展示



![image-20210205185110294](../picture/Spring+SpringMVC%E7%BB%83%E4%B9%A0/image-20210205185110294.png)



**每一个用户还具备一个/多个角色**





![image-20210205190251534](../picture/Spring+SpringMVC%E7%BB%83%E4%B9%A0/image-20210205190251534.png)







![image-20210205191642383](../picture/Spring+SpringMVC%E7%BB%83%E4%B9%A0/image-20210205191642383.png)





> **通过userId  查询到所有的roleId，**









```html
<c:forEach items="${userList}" var="user">
   <tr>
      <td><input name="ids" type="checkbox"></td>
      <td>${user.id}</td>
      <td>${user.username}</td>
      <td>${user.email}</td>
      <td>${user.phoneNum}</td>
      <td class="text-center">
         <c:forEach items="${user.roles}" var="role">
            &nbsp;&nbsp;${role.roleName}
         </c:forEach>
      </td>
      <td class="text-center">
         <a href="javascript:void(0);" onclick="delUser('${user.id}')" class="btn bg-olive btn-xs">删除</a>
      </td>
   </tr>
</c:forEach>
```





controller：



```java
@Controller
@RequestMapping("/user")
public class UserController {

    @Autowired
    private UserService userService;

    @RequestMapping("/list")
    public ModelAndView list(){
        List<User> userList = userService.list();
        ModelAndView modelAndView = new ModelAndView();
        modelAndView.addObject("userList", userList);
        modelAndView.setViewName("user-list");
        return modelAndView;
    }
}
```



service:



```java
@Service
public class UserServiceImpl implements UserService {
    @Autowired
    private UserDao userDao;

    @Autowired
    private RoleDao roleDao;


    @Override
    public List<User> list() {
        List<User> userList = userDao.list();
        
        //还不具备roles的数据
        //封装userList中的每一个User的roles数据
        for(User user : userList){
            //获得user的id
            Long id = user.getId();
            //将id作为参数，查询当前userId对应的 Role集合数据
            //每一个userid对应一个Role集合，并且都是一个Role对象的参数
            List<Role> roles = roleDao.findRoleByUserId(id);
            user.setRoles(roles);
        }
        return userList;
    }
}
```





dao:



```java
@Repository
public class UserDaoImpl implements UserDao {
    @Autowired
    private JdbcTemplate jdbcTemplate;

    @Override
    public List<User> list() {
        List<User> userList = jdbcTemplate.query("select * from sys_user", new BeanPropertyRowMapper<User>(User.class));
        return userList;
    }
}
```



```java
@Override
public List<Role> findRoleByUserId(Long id) {
    List<Role> roles = template.query("select * from sys_user_role ur, sys_role r where ur.roleId=r.id and ur.userId=?",
            new BeanPropertyRowMapper<Role>(Role.class), id);
    return roles;
}
```





## 用户添加





![image-20210205211034633](../picture/Spring+SpringMVC%E7%BB%83%E4%B9%A0/image-20210205211034633.png)‘





新建用户时，点击新建按钮先去到添加用户的页面user-add.jsp,在添加用户页面需要展示可供选择的角色信息，因此来到添加页面时需要查询所有的角色信息并展示



**跳转到保存的页面**

![image-20210205211148069](../picture/Spring+SpringMVC%E7%BB%83%E4%B9%A0/image-20210205211148069.png)







**数据库最终保存的是id，展示的是roleName**

```html
<div class="col-md-2 title">用户角色</div>
<div class="col-md-10 data">
   <c:forEach items="${roleList}" var="role">
      <input class="" type="checkbox" name="roleIds" value="${role.id}">${role.roleName}
   </c:forEach>
</div>
```



![image-20210205212120558](../picture/Spring+SpringMVC%E7%BB%83%E4%B9%A0/image-20210205212120558.png)





**去到user-add.jsp页面时先查询所有角色信息的controller代码**



```java
//查询所有角色信息的service层和dao层代码在之前角色列表展示功能的时候已经写了，因此只需调用即可
@RequestMapping("/saveUI")
public ModelAndView saceUI(){
    ModelAndView modelAndView = new ModelAndView();
    //页面需要当前所有的角色数据
    List<Role> roleList = roleService.list();
    modelAndView.addObject("roleList", roleList);
    //跳转到添加的页面
    modelAndView.setViewName("user-add");
    return modelAndView;
}
```



----



**提交表单**

![image-20210205212338944](../picture/Spring+SpringMVC%E7%BB%83%E4%B9%A0/image-20210205212338944.png)







![image-20210205215750077](../picture/Spring+SpringMVC%E7%BB%83%E4%B9%A0/image-20210205215750077.png)



user中的id为空？

id是在数据库中自增数据，**数据库自动生成**。

如何拿到数据库自动生成的id





![image-20210205220410504](../picture/Spring+SpringMVC%E7%BB%83%E4%B9%A0/image-20210205220410504.png)



dao层：

```java
 @Override
    public Long save(User user) {
//        jdbcTemplate.update("insert into sys_user values(?,?,?,?,?)",
//                null, user.getUsername(), user.getEmail(), user.getPassword(), user.getPhoneNum());

        //返回当前保存的用户的id，该id是数据库自动生成的
        //创建PreparedStatementCreator
        PreparedStatementCreator preparedStatementCreator = new PreparedStatementCreator() {
            @Override
            public PreparedStatement createPreparedStatement(Connection connection) throws SQLException {
                //使用原始的jdbc PreparedStatement
                //指定生成主键
                PreparedStatement preparedStatement = connection.prepareStatement("insert into sys_user values(?,?,?,?,?)", PreparedStatement.RETURN_GENERATED_KEYS);
                preparedStatement.setObject(1, null);
                preparedStatement.setObject(2, user.getUsername());
                preparedStatement.setObject(3, user.getEmail());
                preparedStatement.setObject(4, user.getPassword());
                preparedStatement.setObject(5, user.getPhoneNum());
                return preparedStatement;
            }
        };

        //创建keyHolder
        GeneratedKeyHolder keyHolder = new GeneratedKeyHolder();
        //keyHolder中存放生成的主键
        jdbcTemplate.update(preparedStatementCreator, keyHolder);
        //获得生成的主键
        long userId = keyHolder.getKey().longValue();
        return userId;
    }

    @Override
    public void saveUserRoleRel(Long userId, Long[] roleIds) {
        for(long roleId : roleIds){
            jdbcTemplate.update("insert into sys_user_role values(?,?)", userId, roleId);
        }
    }
```



service层：

```java
@Override
public void save(User user, Long[] roleIds) {
    //第一步，向sys_user表中存储数据
    //得到自动生成的主键值
    Long userId = userDao.save(user);

    //第二步，想sys_user_role关系表中存储多条数据
    //一个用户具备多个角色
    userDao.saveUserRoleRel(userId, roleIds);
}
```



controller层：

```java
@RequestMapping("/save")
public String save(User user, Long[] roleIds){
    userService.save(user, roleIds);

    return "redirect:/user/list";
}
```





## 删除用户操作





![image-20210205235002730](../picture/Spring+SpringMVC%E7%BB%83%E4%B9%A0/image-20210205235002730.png)



要删除两张表中的userId数据

先删除从表（**关系表**）**存在外键约束**

再删除主表







controller:

```java
//restful风格
@RequestMapping("/del/{userId}")
public String del(@PathVariable("userId") Long userId){
    userService.del(userId);
    return "redirect:/user/list";
}
```







service:

```java
@Override
public void del(Long userId) {
    //1.删除sys_user_role关系表
    userDao.delUserRoleRel(userId);

    //2.删除sys_user表
    userDao.delUser(userId);
}
```







dao:不仅要删除用户表数据，同时需要将用户和角色的关联表数据进行删除

```java
@Override
public void delUserRoleRel(Long userId) {
    jdbcTemplate.update("delete from sys_user_role where userId = ?", userId);
}

@Override
public void delUser(Long userId) {
    jdbcTemplate.update("delete from sys_user where id = ?", userId);
}
```



---



一套操作，应该都具备**事务性**







## 用户登录权限控制



**用户没有登录**的情况下，**不能对后台菜单进行访问操作**，点击菜单跳转到登录页面，只有用户登录成功后才能进行后台功能的操作



![image-20210206000547790](../picture/Spring+SpringMVC%E7%BB%83%E4%B9%A0/image-20210206000547790.png)



---



判断用户是否登录  本质：判断session中有没有user，如果没有登陆则先去登陆，如果已经登陆则直接放行访问目标资源





```java
public class PrivilegeInterceptor implements HandlerInterceptor {
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        //逻辑：判断用户是否登录
        //本质：判断session中有没有user
        HttpSession session = request.getSession();
        User user = (User) session.getAttribute("user");
        if(user == null){
            //没有登录
            response.sendRedirect(request.getContextPath()+"/login.jsp");
            return false;
        }
        //放行  访问目标资源
        return true;
    }
}
```

















![image-20210206001957307](../picture/Spring+SpringMVC%E7%BB%83%E4%B9%A0/image-20210206001957307.png)









输完密码，发现根本就过不去。。。

寻找资源/login

**配置的`/**`  对任何请求都执行过滤操作**

有的请求需要**放行**



```xml
<!--    配置权限拦截器-->
    <mvc:interceptors>
        <mvc:interceptor>
<!--            配置对哪些资源执行拦截操作-->
            <mvc:mapping path="/**"/>
<!--            配置哪些资源排除拦截操作  也可以配置多个-->
            <mvc:exclude-mapping path="/user/login"/>
            <bean class="com.ssdemo.interceptor.PrivilegeInterceptor"/>
        </mvc:interceptor>
    </mvc:interceptors>
```



controller:

```java
@RequestMapping("/login")
public String login(String username, String password, HttpSession httpSession){
    User user = userService.login(username, password);
    if(user != null){
        //说明登录成功  将user存储到session中
        httpSession.setAttribute("user", user);
        return "redirect:/index.jsp";
    }
    return "redirect:/login.jsp";
}
```



service:

```java
@Override
public User login(String username, String password) {
    User user = userDao.findByUsernameAndPassword(username, password);
    return user;
}
```





dao:

```java
@Override
public User findByUsernameAndPassword(String username, String password) {
    //要么有  要么没有
    User user = jdbcTemplate.queryForObject("select * from sys_user where username=? and password=?",
            new BeanPropertyRowMapper<User>(User.class), username, password);
    return user;
}
```







---





登录失败：



![image-20210206003635832](../picture/Spring+SpringMVC%E7%BB%83%E4%B9%A0/image-20210206003635832.png)





`queryForObject()`如果查找失败，会报异常`EmptyResultDataAccessException`





```java
@Override
public User login(String username, String password) {
    try{
        User user = userDao.findByUsernameAndPassword(username, password);
        return user;
    }catch (EmptyResultDataAccessException e ){
        return null;
    }
```



**把异常抛出去**

```java
@Override
public User findByUsernameAndPassword(String username, String password) throws EmptyResultDataAccessException {
    //要么有  要么没有
    User user = jdbcTemplate.queryForObject("select * from sys_user where username=? and password=?",
            new BeanPropertyRowMapper<User>(User.class), username, password);
    return user;
}
```





# tips



## web.xml配置问题



![image-20210205053452765](../picture/Spring+SpringMVC%E7%BB%83%E4%B9%A0/image-20210205053452765.png)

















