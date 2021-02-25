# Mybatis简介

原始jdbc操作：

查询数据

![image-20210206154922829](../picture/Mybatis%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0/image-20210206154922829.png)



插入数据：

![image-20210206154958015](../picture/Mybatis%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0/image-20210206154958015.png)





**原始jdbc操作的分析**

原始jdbc开发存在的问题如下：

①数据库连接**创建、释放频繁**造成系统资源浪费从而影响系统性能

②sql 语句在代码中**硬编码**，造成代码不易维护，实际应用 sql 变化的可能较大，**sql 变动需要改变java代码(耦合)**。

③查询操作时，需要手动将结果集中的数据手动**封装到实体**中。插入操作时，需要手动**将实体的数据设置到sql语句的占位符位置**



> 应对上述问题给出的解决方案：
>
> ①使用数据库**连接池**初始化连接资源
>
> ②将**sql语句抽取到xml配置文件**中 
>
> ③使用**反射、内省**等底层技术，自动将**实体与表**进行**属性与字段的自动映射**

---



mybatis 是一个优秀的基于java的**持久层框架**，它内部**封装了jdbc**，使开发者只需要**关注sql语句**本身，而不需要花费精力去处理加载驱动、创建连接、创建statement等繁杂的过程。

mybatis通过xml或注解的方式将要执行的各种 statement**配置**起来，并通过**java对象**和statement中**sql**的动态参数进行**映射**生成最终执行的sql语句。

最后mybatis框架执行sql并**将结果映射为java对象**并返回。采用`ORM思想`解决了**实体和数据库映射**的问题，对jdbc 进行了封装，屏蔽了jdbc api 底层访问细节，使我们不用与jdbc api 打交道，就可以完成对数据库的持久化操作。





`ORM思想ObjectRelationMapping对象(与数据表之间的)关系映射`





# 快速入门练习



**MyBatis开发步骤：**

①添加MyBatis的坐标



```xml
<dependency>
  <groupId>mysql</groupId>
  <artifactId>mysql-connector-java</artifactId>
  <version>8.0.13</version>
</dependency>
<dependency>
  <groupId>org.mybatis</groupId>
  <artifactId>mybatis</artifactId>
  <version>3.5.6</version>
</dependency>
```



②创建user数据表

③编写User实体类 

④编写映射文件UserMapper.xml



**命名空间+id  组成了调用的名字 `userMapper.findAll`**

**结果类型User，封装到其中**

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="userMapper">
    <select id="findAll" resultType="com.mybatisDemo.domain.User">
        select * from sys_user
    </select>
    
</mapper>
```



⑤编写核心文件SqlMapConfig.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "http://mybatis.org/dtd/mybatis-3-config.dtd">

<configuration>

    <!--数据源环境-->
    <environments default="development">
        <environment id="development">
<!--            事务管理器-->
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/test?useSSL=false&amp;serverTimezone=GMT%2B8"/>
                <property name="username" value="root"/>
                <property name="password" value="huangchenrui20"/>
            </dataSource>
        </environment>
    </environments>
    
<!--    加载映射文件-->
    <mappers>
        <mapper resource="com/mybatisDemo/mapper/UserMapper.xml"/>
    </mappers>
    
</configuration>
```



![image-20210206193809273](../picture/Mybatis%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0/image-20210206193809273.png)





⑥编写测试类



```java
public class MyBatisTest {
    @Test
    public void test1() throws IOException {
        //获得核心配置文件 数据源配置和映射文件
        InputStream resourceAsStream = Resources.getResourceAsStream("sqlMapConfig.xml");

        //获得session工厂对象
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);

        //获得session会话对象
        SqlSession sqlSession = sqlSessionFactory.openSession();

        //执行操作  参数：namespace+id
        List<User> userList = sqlSession.selectList("userMapper.findAll");

        System.out.println(userList);

        //释放资源
        sqlSession.close();
    }
}
```





# MyBatis映射文件





![image-20210206183215338](../picture/Mybatis%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0/image-20210206183215338.png)





# Mybatis的增删改查操作



## 插入数据

**插入操作注意问题**: 

• 插入语句使用insert标签

• 在映射文件中使用parameterType属性指定**要插入的数据类型**

•Sql语句中使用`#{实体属性名}`方式**引用实体中的属性值**

•插入操作使用的API是`sqlSession.insert(“命名空间.id”,实体对象);`

•插入操作涉及数据库数据变化，所以要使用sqlSession对象显示的提交事务，即`sqlSession.commit()` 







```xml
<!--    查询操作-->
    <select id="findAll" resultType="com.mybatisDemo.domain.User">
        select * from user
    </select>
```



？插入失败

**事务**

Mybatis默认事务是不提交的

**查询不需要事务提交，增删改需要提交事务，更新数据到磁盘**

```java
@Test
public void test2() throws IOException {
    //模拟user对象
    User user = new User(2, "hcr", "321");

    //获得核心配置文件
    InputStream resourceAsStream = Resources.getResourceAsStream("sqlMapConfig.xml");

    //获得session工厂对象
    SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);

    //获得session会话对象
    SqlSession sqlSession = sqlSessionFactory.openSession();

    sqlSession.insert("userMapper.save", user);

    //Mybatis执行更新操作  需要提交事务
    sqlSession.commit();

    sqlSession.close();
}
```



## 修改

• 修改语句使用update标签

• 修改操作使用的API是sqlSession.update(“命名空间.id”,实体对象);



```xml
<update id="update" parameterType="com.mybatisDemo.domain.User">
    update user set username=#{username}, password=#{password} where id=#{id}
</update>
```



```java
    @Test
    public void test3() throws IOException {
        //模拟user对象
        User user = new User(2, "huangcr", "123123");

        //获得核心配置文件
        InputStream resourceAsStream = Resources.getResourceAsStream("sqlMapConfig.xml");

        //获得session工厂对象
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);

        //获得session会话对象
        SqlSession sqlSession = sqlSessionFactory.openSession();

        sqlSession.update("userMapper.update", user);

        //Mybatis执行更新操作  需要提交事务
        sqlSession.commit();

        sqlSession.close();
    }
}
```





## 删除

**删除操作注意问题**

• 删除语句使用delete标签

• Sql语句中使用#{**任意字符串**}方式引用传递的**单个参数**

• 删除操作使用的API是`sqlSession.delete(“命名空间.id”,Object);`



**如果多个条件username=? password=?  还是用一个实体，取出实体的参数**



```xml
<!--    当只传递一个参数，写什么都行-->
    <delete id="delete" parameterType="java.lang.Integer">
        delete from user where id=#{id}
    </delete>
```





```java
    @Test
    public void test4() throws IOException {

        //获得核心配置文件
        InputStream resourceAsStream = Resources.getResourceAsStream("sqlMapConfig.xml");

        //获得session工厂对象
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);

        //获得session会话对象
        SqlSession sqlSession = sqlSessionFactory.openSession();

        sqlSession.delete("userMapper.delete", 3);

        //Mybatis执行更新操作  需要提交事务
        sqlSession.commit();

        sqlSession.close();
    }
}
```





## 小结



```xml
增删改查映射配置与API：
查询数据： List<User> userList = sqlSession.selectList("userMapper.findAll");
    <select id="findAll" resultType="com.itheima.domain.User"  可能还会有个parameterType作为参数>
        select * from User
    </select>
添加数据： sqlSession.insert("userMapper.add", user);
    <insert id="add" parameterType="com.itheima.domain.User">
        insert into user values(#{id},#{username},#{password})
    </insert>
修改数据： sqlSession.update("userMapper.update", user);
    <update id="update" parameterType="com.itheima.domain.User">
        update user set username=#{username},password=#{password} where id=#{id}
    </update>
删除数据：sqlSession.delete("userMapper.delete",3);
    <delete id="delete" parameterType="java.lang.Integer">
        delete from user where id=#{id}
    </delete>
```









# MyBatis核心配置文件概述



![image-20210206193548085](../picture/Mybatis%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0/image-20210206193548085.png)



## environments标签



数据库环境的配置，支持多环境配置

**每个environment可以配置一个数据源环境**

![image-20210206211509533](../picture/Mybatis%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0/image-20210206211509533.png)





其中，**事务管理器（transactionManager）**类型有两种：

- JDBC：这个配置就是直接**使用了JDBC 的提交和回滚**设置，它依赖于从数据源得到的**连接connection**来管理事务作用域。

- MANAGED：这个配置几乎没做什么。它从来不提交或回滚一个连接，而是让容器来管理事务的整个生命周期（比如 JEE 应用服务器的上下文）。 默认情况下它会关闭连接，然而一些容器并不希望这样，因此需要将 closeConnection 属性设置为 false 来阻止它默认的关闭行为。



其中，数据源（dataSource）类型有三种：

- UNPOOLED：这个数据源的实现只是**每次被请求时打开和关闭连接**。

- POOLED：这种数据源的实现利用**“池”**的概念将 JDBC 连接(connection)对象组织起来。

- JNDI：这个数据源的实现是为了能在如 EJB 或应用服务器这类容器中使用，容器可以集中或在外部配置数据源，然后放置一个 JNDI 上下文的引用。





## mapper标签



```xml
<!--    加载映射文件-->
    <mappers>
        <mapper resource="com/mybatisDemo/mapper/UserMapper.xml"/>
    </mappers>
```



该标签的作用是**加载映射**的，加载方式有如下几种：

- 使用相对于类路径的**资源引用**，例如：

`<mapper resource="org/mybatis/builder/AuthorMapper.xml"/>`

- 使用完全限定资源定位符（URL），例如：

`<mapper url="file:///var/mappers/AuthorMapper.xml"/>`

- 使用**映射器接口**实现类的完全限定类名，例如：

`<mapper class="org.mybatis.builder.AuthorMapper"/>`

- 将包内的映射器接口实现全部注册为映射器，例如：

`<package name="org.mybatis.builder"/>`





## properties标签



实际开发中，习惯将**数据源的配置信息**单独抽取成一个properties文件，该标签可以**加载额外配置的properties文件**



![image-20210206220546154](../picture/Mybatis%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0/image-20210206220546154.png)





```xml
<!--    通过properties标签来加载外部properties文件-->
    <properties resource="jdbc.properties"/>
    <!--数据源环境-->
    <environments default="development">
        <environment id="development">
<!--            事务管理器-->
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="${jdbc.driver}"/>
                <property name="url" value="${jdbc.url}"/>
                <property name="username" value="${jdbc.user}"/>
                <property name="password" value="${jdbc.password}"/>
            </dataSource>
        </environment>
    </environments>
```



## typeAliases标签



类型别名是为Java 类型设置一个短的名字。原来的类型名称配置如下

![image-20210206221824714](../picture/Mybatis%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0/image-20210206221824714.png)





mybatis框架已经为我们设置好的一些常用的类型的别名

![image-20210206221725053](../picture/Mybatis%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0/image-20210206221725053.png)





(要按标签的顺序写)



```xml
<!--    定义别名-->
<typeAliases>
    <typeAlias type="com.mybatisDemo.domain.User" alias="user"/>
</typeAliases>

<!--    查询操作-->
    <select id="findAll" resultType="user">
        select * from user
    </select>
```





# MyBatis相应API



## SqlSession工厂构建器SqlSessionFactoryBuilder

常用API：`SqlSessionFactory  build(InputStream inputStream)`

通过加载mybatis的核心文件的输入流的形式构建一个`SqlSessionFactory`对象

**构建  一个工厂**



```java
String resource = "org/mybatis/builder/mybatis-config.xml"; 
InputStream inputStream = Resources.getResourceAsStream(resource); 
SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder(); 
//获得工厂
SqlSessionFactory factory = builder.build(inputStream);
```

其中， Resources 工具类，这个类在 org.apache.ibatis.io 包中。Resources 类帮助你从类路径下、文件系统或一个 web URL 中加载资源文件。





## SqlSession工厂对象SqlSessionFactory



SqlSessionFactory 有多个个方法创建SqlSession 实例。常用的有如下两个：

**使用工厂来造一个session对象**

![image-20210206223521764](../picture/Mybatis%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0/image-20210206223521764.png)

​	true：自动提交事务





## SqlSession会话对象



SqlSession 实例在 MyBatis 中是非常强大的一个类。在这里你会看到所有**执行语句、提交或回滚事务和获取映射器实例**的方法。

执行语句的方法主要有：

```java
//查一条数据
<T> T selectOne(String statement, Object parameter) 
<E> List<E> selectList(String statement, Object parameter) 
    
int insert(String statement, Object parameter) 
int update(String statement, Object parameter) 
int delete(String statement, Object parameter)

```

**操作事务**的方法主要有：

```java
void commit()  
void rollback() 
```







# mybatis的Dao层实现



## 传统开发方式



UserMapperImpl实现

```java
public class UserMapperImpl implements UserMapper {
    @Override
    public List<User> findAll() throws IOException {
        InputStream resourceAsStream = Resources.getResourceAsStream("sqlMapConfig.xml");
        SqlSessionFactory sqlSessionFactory =  new SqlSessionFactoryBuilder().build(resourceAsStream);
        SqlSession sqlSession = sqlSessionFactory.openSession();
        List<User> userList  = sqlSession.selectList("userMapper.findAll");
        return userList;
    }
}
```



```java
public class ServiceDemo {
    public static void main(String[] args) throws IOException {
        //创建dao层对象  当前dao层实现是手动编写的
        UserMapper userMapper = new UserMapperImpl();
        System.out.println(userMapper.findAll());
    }
}
```







## 代理开发方式

**`dao实现类`  大部分都是模板代码**



采用 Mybatis 的代理开发方式实现 DAO 层的开发，这种方式是我们后面进入企业的主流。

Mapper 接口开发方法只需要程序员编写**Mapper 接口**（**相当于Dao 接口**），由Mybatis 框架根据接口定义创建接口的动态代理对象，代理对象的方法体同上边Dao接口实现类方法。



Mapper 接口开发需要遵循以下规范：

**1) Mapper.xml文件中的`namespace`与`mapper接口`的全限定名相同**

**2) Mapper接口方法名和Mapper.xml中定义的每个statement的id相同**

**3) Mapper接口方法的输入参数类型和mapper.xml中定义的每个sql的parameterType的类型相同**

**4) Mapper接口方法的输出参数类型和mapper.xml中定义的每个sql的resultType的类型相同**



![image-20210207161716795](../picture/Mybatis%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0/image-20210207161716795.png)





```java
public static void main(String[] args) throws IOException {

    InputStream resourceAsStream = Resources.getResourceAsStream("sqlMapConfig.xml");
    SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);
    SqlSession sqlSession = sqlSessionFactory.openSession();

    //没有实现，mybatis帮助实现的
    UserMapper mapper = sqlSession.getMapper(UserMapper.class);
    System.out.println(mapper.findAll());
}
```

---



带参数：

```xml
<!--    根据id进行查询-->
    <select id="findById" parameterType="int" resultType="user">
        select * from user where id=#{id}
    </select>
```



```java
public static void main(String[] args) throws IOException {

    InputStream resourceAsStream = Resources.getResourceAsStream("sqlMapConfig.xml");
    SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);
    SqlSession sqlSession = sqlSessionFactory.openSession();

    UserMapper mapper = sqlSession.getMapper(UserMapper.class);
    System.out.println(mapper.findById(1));
}
```





# MyBatis映射文件深入



## 动态sql语句



Mybatis 的映射文件中，前面我们的 SQL 都是比较简单的，有些时候业务逻辑复杂时，我们的 SQL是**动态变化**的，此时在前面的学习中 SQL 就不能满足要求了。



参考的官方文档，描述如下：

![image-20210207170051151](../picture/Mybatis%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0/image-20210207170051151.png)





### if



![image-20210207171006397](../picture/Mybatis%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0/image-20210207171006397.png)



根据实体类的不同取值，使用不同的 SQL语句来进行查询。比如在 id如果不为空时可以根据id查询，如果username 不同空时还要加入用户名作为条件。这种情况在我们的多条件组合查询中经常会碰到。



根据是否有条件，来看是否要加where标签

```xml
<select id="findByCondition" parameterType="user" resultType="user">
    select * from user
    <where>
        <if test="id!=0">
            and id=#{id}
        </if>
        <if test="username!=null">
            and username=#{username}
        </if>
    </where>
</select>
```







### foreach



查询id=1  /  id=2  /   id=3

`select * from user where id=1 or id=2  /  id in (1,2,3)`

**所有要查询的id**



> foreach标签的属性含义如下：
>
> - `<foreach>`标签用于遍历集合，它的属性：
>
> - collection：代表要遍历的集合元素类型(array...)，注意编写时不要写#{}
>
> - open：代表语句的开始部分
>
> - close：代表结束部分
>
> - item：代表遍历集合的每个元素，生成的变量名
>
> - sperator：代表分隔符



```xml
<select id="findByIds" parameterType="list" resultType="user">
    select * from user
    <where>
        <foreach collection="list" open="id in(" close=")" item="id" separator=",">
            #{id}
        </foreach>
    </where>
</select>
```





![image-20210207175704493](../picture/Mybatis%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0/image-20210207175704493.png)





## sql片段抽取





```xml
<!--    对相同的语句进行抽取-->
    <sql id="selectUser">select * from user</sql>


<select id="findByIds" parameterType="list" resultType="user">
    <include refid="selectUser"></include>
    <where>
        <foreach collection="list" open="id in(" close=")" item="id" separator=",">
            #{id}
        </foreach>
    </where>
</select>
```







# MyBatis核心配置文件深入





## typeHandlers标签



无论是 MyBatis 在预处理语句（PreparedStatement）中设置一个参数时，还是从结果集中取出一个值时， 都会用类型处理器将获取的值以合适的方式转换成 Java 类型。下表描述了一些默认的类型处理器（截取部分）。

![image-20210207203426517](../picture/Mybatis%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0/image-20210207203426517.png)



可以重写类型处理器或**创建你自己的类型处理器**来处理**不支持的或非标准的类型**。具体做法为：实现 org.apache.ibatis.type.TypeHandler 接口， 或继承一个很便利的类 `org.apache.ibatis.type.BaseTypeHandler`， 然后可以选择性地将它映射到一个JDBC类型。





需求：一个Java中的Date数据类型，我想将之存到数据库的时候存成一个1970年至今的毫秒数，取出来时**转换成java的Date**，即**java的Date与数据库的varchar毫秒值之间转换**。



> 开发步骤：
>
> ①定义转换类继承类`BaseTypeHandler<T>`
>
> ②覆盖4个未实现的方法，其中`setNonNullParameter`为java程序设置数据到数据库的回调方法，`getNullableResult`为查询时 mysql的字符串类型转换成 java的Type类型的方法
>
> ③在MyBatis核心配置文件中进行注册



![image-20210207205014205](../picture/Mybatis%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0/image-20210207205014205.png)







```java
public class DateTypeHandler extends BaseTypeHandler<Date> {
    @Override
    public void setNonNullParameter(PreparedStatement preparedStatement, int i, Date date, JdbcType jdbcType) throws SQLException {
        //负责将java类型转换成数据库需要的类型
        long time = date.getTime();
        preparedStatement.setLong(i, time);
    }


    //将数据库中类型转换成java类型
    @Override
    public Date getNullableResult(ResultSet resultSet, String s) throws SQLException {
        //String参数是数据库中要转换的字段名称
        //获取结果集中需要的数据(Long)  将其转换成Date类型  返回
        long aLong = resultSet.getLong(s);
        Date date = new Date(aLong);
        return date;
    }

    @Override
    public Date getNullableResult(ResultSet resultSet, int i) throws SQLException {
        long aLong = resultSet.getLong(i);
        Date date = new Date(aLong);
        return date;
    }

    @Override
    public Date getNullableResult(CallableStatement callableStatement, int i) throws SQLException {
        long aLong = callableStatement.getLong(i);
        Date date = new Date(aLong);
        return date;
    }
}
```



```xml
<!--    当只传递一个参数，写什么都行-->
    <delete id="delete" parameterType="java.lang.Integer">
        delete from user where id=#{id}
    </delete>
```







## plugins标签



MyBatis可以使用**第三方的插件**来对功能进行扩展，分页助手PageHelper是将**分页**的复杂操作进行封装，使用简单的方式即可获得分页的相关数据



开发步骤：

①导入通用PageHelper的坐标

②在mybatis核心配置文件中配置PageHelper插件

③测试分页数据获取



`class com.github.pagehelper.PageHelper cannot be cast to class org.apache.ibatis.plugin.Interceptor`

> **在PageHelper5版本中MyBatis的插件指定不为PageHelper**，解决办法：
>
> 1. 将Maven依赖改为版本4
> 2. 将插件配置改为`<plugin interceptor="com.github.pagehelper.PageInterceptor"/>`

```xml
<dependency>
  <groupId>com.github.pagehelper</groupId>
  <artifactId>pagehelper</artifactId>
  <version>4.1.6</version>
</dependency>
```



```xml
<!--    配置分页助手插件-->
    <plugins>
        <plugin interceptor="com.github.pagehelper.PageHelper">
            <property name="dialect" value="mysql"/>
        </plugin>
    </plugins>
```



```java
public static void main(String[] args) throws IOException {

    InputStream resourceAsStream = Resources.getResourceAsStream("sqlMapConfig.xml");
    SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);
    SqlSession sqlSession = sqlSessionFactory.openSession();

    //设置分页相关参数   当前页  +  每页显示的条数
    PageHelper.startPage(2, 3);

    UserMapper mapper = sqlSession.getMapper(UserMapper.class);
    List<User> userList = mapper.findAll();
    for(User user : userList){
        System.out.println(user);
    }
    sqlSession.close();
}
```



---



直接打印userList:

`Page{count=true, pageNum=2, pageSize=3, startRow=3, endRow=6, total=7, pages=3, countSignal=false, orderBy='null', orderByOnly=false, reasonable=false, pageSizeZero=false}`

**显示了pageInfo的各种信息**

```java
//其他分页的数据
PageInfo<User> pageInfo = new PageInfo<User>(userList);
System.out.println("总条数："+pageInfo.getTotal());
System.out.println("总页数："+pageInfo.getPages());
System.out.println("当前页："+pageInfo.getPageNum());
System.out.println("每页显示长度："+pageInfo.getPageSize());
System.out.println("是否第一页："+pageInfo.isIsFirstPage());
System.out.println("是否最后一页："+pageInfo.isIsLastPage());
```





---



MyBatis核心配置文件常用标签：

1、`properties`标签：该标签可以加载外部的properties文件

2、`typeAliases`标签：设置类型别名

3、`environments`标签：数据源环境配置标签

4、`typeHandlers`标签：配置自定义类型处理器

5、`plugins`标签：配置MyBatis的插件









# MyBatis多表查询



## 一对一查询



用户表和订单表的关系为，一个用户有多个订单，一个订单只从属于一个用户



**一对一查询**的需求：查询一个订单，与此同时查询出该订单所属的用户



![image-20210208015226299](../picture/Mybatis%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0/image-20210208015226299.png)





---



一对一查询的语句：



```java
public interface OrderMapper {
    public List<Order> findAll();
}
```



![image-20210208025810254](../picture/Mybatis%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0/image-20210208025810254.png)





![image-20210208025013491](../picture/Mybatis%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0/image-20210208025013491.png)





![image-20210208025748156](../picture/Mybatis%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0/image-20210208025748156.png)



虽然在查询的结果中显示了User，但需要告诉mybatis：username属性是User类内部的属性，才能进行封装



**order中没有 username  password字段，不能放到order中**

![image-20210208025942553](../picture/Mybatis%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0/image-20210208025942553.png)





```xml
    <resultMap id="orderMap" type="order">
<!--        手动去指定字段与实体属性的映射关系-->
<!--   主键id     column:数据表的字段名称
                  property：实体的属性名称        -->
<!--        将查询出的oid属性与id映射-->
        <id column="oid" property="id"/>
        <result column="ordertime" property="ordertime"/>
        <result column="total" property="total"/>
        <result column="ordertime" property="ordertime"/>
<!--        将uid往user类内部的属性封装-->
        <result column="uid" property="user.id"/>
        <result column="username" property="user.username"/>
        <result column="password" property="user.password"/>
        <result column="birthday" property="user.birthday"/>

    </resultMap>

<!--    查询操作-->
    <select id="findAll" resultMap="orderMap">
        SELECT *,o.id oid FROM orders o, user u where o.uid=u.id
    </select>
```







![image-20210208030756624](../picture/Mybatis%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0/image-20210208030756624.png)









```xml
    <resultMap id="orderMap" type="order">
<!--        手动去指定字段与实体属性的映射关系-->
<!--   主键id     column:数据表的字段名称
                  property：实体的属性名称        -->
<!--        将查询出的oid属性与id映射-->
        <id column="oid" property="id"/>
        <result column="ordertime" property="ordertime"/>
        <result column="total" property="total"/>
        <result column="ordertime" property="ordertime"/>
<!--        将uid往user类内部的属性封装-->
<!--        <result column="uid" property="user.id"/>-->
<!--        <result column="username" property="user.username"/>-->
<!--        <result column="password" property="user.password"/>-->
<!--        <result column="birthday" property="user.birthday"/>-->

<!--        匹配  property：当前实体Order中的属性名称
                  javaType：当前实体order中的属性的类型  定义别名-->
        <association property="user" javaType="user">
            <id column="uid" property="id"/>
            <result column="username" property="username"/>
            <result column="password" property="password"/>
            <result column="birthday" property="birthday"/>
        </association>
    </resultMap>
```







## 一对多查询



一对多查询的需求：查询一个用户，与此同时查询出**该用户具有的订单**



![image-20210208164453204](../picture/Mybatis%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0/image-20210208164453204.png)



sql语句：

`select *,o.id oid from user u left join orders o on u.id=o.uid;`

![image-20210208164438996](../picture/Mybatis%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0/image-20210208164438996.png)







```xml
<resultMap id="userMap" type="user">
        <id column="uid" property="id"/>
        <result column="username" property="username"/>
        <result column="passowrd" property="password"/>
        <result column="birthday" property="birthday"/>
<!--        配置集合信息-->
<!--        property: 集合名称
            ofType:当前集合中的数据类型       -->
        <collection property="orderList" ofType="order">
<!--            封装order的数据-->
            <id column="oid" property="id"/>
            <result column="ordertime" property="ordertime"/>
            <result column="total" property="total"/>
        </collection>
    </resultMap>

<!--    查询操作-->
    <select id="findAll" resultMap="userMap">
        select *,o.id oid from user u, orders o where u.id=o.uid
    </select>
```



```java
@Test
public void test4() throws IOException {
    InputStream resourceAsStream = Resources.getResourceAsStream("sqlMapConfig.xml");
    SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);
    SqlSession sqlSession = sqlSessionFactory.openSession();

    UserMapper mapper = sqlSession.getMapper(UserMapper.class);

    List<User> userList = mapper.findAll();
    for(User user : userList){
        System.out.println(user);
    }
    sqlSession.close();
}
```





![image-20210208165740876](../picture/Mybatis%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0/image-20210208165740876.png)











## 多对多查询



用户表和角色表的关系为，一个用户有多个角色，一个角色被多个用户使用

多对多查询的需求：查询用户同时查询出该用户的所有角色



![image-20210208192248725](../picture/Mybatis%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0/image-20210208192248725.png)





![image-20210208212603500](../picture/Mybatis%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0/image-20210208212603500.png)



`select * from user u, sys_role r, sys_user_role ur where u.id=ur.userId and ur.roleId=r.id`



![image-20210208195807976](../picture/Mybatis%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0/image-20210208195807976.png)



![image-20210208195754952](../picture/Mybatis%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0/image-20210208195754952.png)





```xml
    <resultMap id="userRoleMap" type="user">
<!--        user的信息-->
        <id column="userid" property="id"/>
        <result column="username" property="username"/>
        <result column="passowrd" property="password"/>
        <result column="birthday" property="birthday"/>
<!--         user内部的roleList信息   -->
        <collection property="roleList" ofType="role">
            <id column="roleid" property="id"/>
            <result column="rolename" property="rolename"/>
            <result column="roleDesc" property="roleDesc"/>
        </collection>
    </resultMap>

    <select id="findUserAndRoleAll" resultMap="userRoleMap">
        select * from user u, sys_role r, sys_user_role ur where u.id=ur.userId and ur.roleId=r.id
    </select>
```



```java
@Test
public void test5() throws IOException {
    InputStream resourceAsStream = Resources.getResourceAsStream("sqlMapConfig.xml");
    SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);
    SqlSession sqlSession = sqlSessionFactory.openSession();

    UserMapper mapper = sqlSession.getMapper(UserMapper.class);

    List<User> userList = mapper.findUserAndRoleAll();
    for(User user : userList){
        System.out.println(user);
    }
    sqlSession.close();
}
```



-----



小结：

MyBatis多表配置方式：

**一对一配置：使用`<resultMap>`还可以加  `<association>` 做配置**

**一对多配置：使用`<resultMap>`+`<collection>`做配置**

**多对多配置：使用`<resultMap>`+`<collection>`做配置**











# MyBatis的注解开发



注解开发越来越流行，Mybatis也可以使用注解开发方式，就可以减少编写Mapper映射文件了。

> @Insert：实现新增
>
> @Update：实现更新
>
> @Delete：实现删除
>
> @Select：实现查询
>
> @Result：实现结果集封装
>
> @Results：可以与@Result 一起使用，封装多个结果集
>
> @One：实现一对一结果集封装
>
> @Many：实现一对多结果集封装





## 增删改查



```xml
<!--    加载映射关系  通过注解配置-->
    <mappers>
<!--        指定扫描接口所在的包 -->
        <package name="com.mybatisDemo.dao"/>
    </mappers>
```



```java
public interface UserMapper {

    @Insert("insert into user values(#{id},#{username},#{password},#{birthday})")
    public void save(User user);

    @Update("update user set username=#{username}, password=#{password} where id=#{id}")
    void update(User user);

    @Delete("delete from user where id=#{id}")
    void delete(int i);

    @Select("select * from user where id=#{id}")
    User findById(int id);

    @Select("select * from user")
    List<User> findAll();
}
```





```java
public class MyBatisTest {
    private UserMapper mapper;

    @Before
    public void test1() throws IOException {
        InputStream resourceAsStream = Resources.getResourceAsStream("sqlMapConfig.xml");
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);
        SqlSession sqlSession = sqlSessionFactory.openSession(true);
        mapper = sqlSession.getMapper(UserMapper.class);
    }

    @Test
    public void testSave(){
        User user = new User();
        user.setUsername("crh");
        user.setPassword("123");
        user.setBirthday(new Date());
        mapper.save(user);
    }

    @Test
    public void testUpdate(){
        User user = new User();
        user.setId(4);
        user.setUsername("rch");
        user.setPassword("123");
        mapper.update(user);
    }

    @Test
    public void testDelete(){
        mapper.delete(3);
    }

    @Test
    public void testFindById(){
        System.out.println(mapper.findById(5));
    }

    @Test
    public void testFindAll() throws IOException {
        System.out.println(mapper.findAll());
    }
```









## 复杂映射开发



实现复杂关系映射之前我们可以在映射文件中通过配置`<resultMap>`来实现，使用注解开发后，我们可以使用`@Results`注解，`@Result`注解，`@One`注解，`@Many`注解组合完成复杂关系的配置





![image-20210208231206156](../picture/Mybatis%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0/image-20210208231206156.png)





![image-20210208231221029](../picture/Mybatis%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0/image-20210208231221029.png)







## 一对一查询



```xml
<!--    加载映射关系  通过注解配置-->
    <mappers>
<!--        指定接口所在的包 -->
        <package name="com.mybatisDemo.dao"/>
    </mappers>
```



```java
public interface OrderMapper {

    @Select("select *, o.id oid from orders o, user u where o.uid=u.id")
    @Results({
        		//将每个查询到的结果封装进实体对象
            @Result(column = "oid", property = "id"),
            @Result(column = "ordertime", property = "ordertime"),
            @Result(column = "total", property = "total"),
            @Result(column = "uid", property = "user.id"),
            @Result(column = "username", property = "user.username"),
            @Result(column = "password", property = "user.password")
    })
    public List<Order> findAll();
}
```





```java
public class MyBatisTest2 {
    private OrderMapper mapper;

    @Before
    public void test1() throws IOException {
        InputStream resourceAsStream = Resources.getResourceAsStream("sqlMapConfig.xml");
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);
        SqlSession sqlSession = sqlSessionFactory.openSession(true);
        mapper = sqlSession.getMapper(OrderMapper.class);
    }



    @Test
    public void testFindAll() throws IOException {
        System.out.println(mapper.findAll());
    }

}
```







-----



```java
public interface OrderMapper {
    
    @Select("select * from orders")
    @Results({
            @Result(column = "id", property = "id"),
            @Result(column = "ordertime", property = "ordertime"),
            @Result(column = "total", property = "total"),
            @Result(
                
                //这里将user属性当成一个整体  通过子查询来封装数据进user对象
                   property = "user", //要封装的属性名称
                   column = "uid",   //根据哪个字段去查询user表的数据
                   javaType = User.class,  //要封装的实体类型
                    
                   //select属性：查询哪个接口的方法去获得数据
                   one = @One(select = "com.mybatisDemo.dao.UserMapper.findById")
            )
    })
    public List<Order> findAll();
}
```











## 一对多查询



一对多查询的需求：查询一个用户，与此同时查询出该用户具有的订单

![image-20210209013454884](../picture/Mybatis%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0/image-20210209013454884.png)







![image-20210209013538616](../picture/Mybatis%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0/image-20210209013538616.png)

**一对多：使用List来接收user对象集合**



UserMapper

```java
@Select("select * from user")
@Results({
        @Result(column = "id", property = "id"),
        @Result(column = "username", property = "username"),
        @Result(column = "password", property = "password"),
        @Result(
                property = "orderList",
                column = "id",
                javaType = List.class,
                many = @Many(select = "com.mybatisDemo.dao.OrderMapper.findByUid")
        )
}
)
public List<User> findUserAndOrderAll();
```



OrderMapper

**使用OrderMapper中的接口方法   来根据uid查询当前用户的所有订单**

```java
public interface OrderMapper {
    @Select("select * from orders where uid=#{uid}")
    List<Order> findByUid(int uid);
}
```







## 多对多查询





多对多查询的需求：查询用户同时查询出该用户的所有角色



![image-20210209023048095](../picture/Mybatis%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0/image-20210209023048095.png)





**保存用户的角色信息**

![image-20210209023149267](../picture/Mybatis%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0/image-20210209023149267.png)





![image-20210209023525670](../picture/Mybatis%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0/image-20210209023525670.png)





`select * from sys_user_role ur, sys_role r where ur.roleId=r.id and ur.userId=#{uid}`



![image-20210209040124406](../picture/Mybatis%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0/image-20210209040124406.png)









userMapper

```java
//多对多查询，需要三张表
@Select("select * from user")
@Results({
        @Result(column = "id", property = "id"),
        @Result(column = "username", property = "username"),
        @Result(column = "password", property = "password"),
        @Result(column = "birthday", property = "birthday"),

        @Result(
                property = "roleList",
                column = "id",
                javaType = List.class, //封装到集合中
                //查出来的属性自动匹配进Role对象中
                many = @Many(select = "com.mybatisDemo.dao.RoleMapper.findByUid")
        )
})
public List<User> findUserAndRoleAll();
```



```java
public interface RoleMapper {

    @Select("select * from sys_user_role ur, sys_role r where ur.roleId=r.id and ur.userId=#{uid}")
    //因为要查的是user对象，role为user对象的参数
    //根据每个唯一的uid作为查找对象，  在中间表中找到uid对应的rid   再通过rid匹配role表中id
    //最终找出uid对应的所有role对象
    public List<Role> findByUid(int uid);
}
```



**对每一个id都去查询所有对应的role**

![image-20210209050123058](../picture/Mybatis%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0/image-20210209050123058.png)















# SSM框架整合





## 原始方式整合





实体类

```java
public class Account {
    private int id;
    private String name;
    private double money;
    //省略getter和setter方法
}
```





mapper

```java
public interface AccountMapper {
    //保存账户数据
    void save(Account account);
    //查询账户数据
    List<Account> findAll();
}
```





pom.xml

```xml
<dependency>
  <groupId>org.springframework</groupId>
  <artifactId>spring-context</artifactId>
  <version>5.0.5.RELEASE</version>
</dependency>
<dependency>
  <groupId>org.aspectj</groupId>
  <artifactId>aspectjweaver</artifactId>
  <version>1.8.7</version>
</dependency>
<dependency>
  <groupId>org.springframework</groupId>
  <artifactId>spring-jdbc</artifactId>
  <version>5.0.5.RELEASE</version>
</dependency>
<dependency>
  <groupId>org.springframework</groupId>
  <artifactId>spring-tx</artifactId>
  <version>5.0.5.RELEASE</version>
</dependency>
<dependency>
  <groupId>org.springframework</groupId>
  <artifactId>spring-test</artifactId>
  <version>5.0.5.RELEASE</version>
</dependency>
<dependency>
  <groupId>org.springframework</groupId>
  <artifactId>spring-webmvc</artifactId>
  <version>5.0.5.RELEASE</version>
</dependency>

<!--servlet和jsp-->
<dependency>
  <groupId>javax.servlet</groupId>
  <artifactId>servlet-api</artifactId>
  <version>2.5</version>
</dependency>
<dependency>
  <groupId>javax.servlet.jsp</groupId>
  <artifactId>jsp-api</artifactId>
  <version>2.0</version>
</dependency>

<!--mybatis相关-->
<dependency>
  <groupId>org.mybatis</groupId>
  <artifactId>mybatis</artifactId>
  <version>3.4.5</version>
</dependency>
<dependency>
  <groupId>org.mybatis</groupId>
  <artifactId>mybatis-spring</artifactId>
  <version>1.3.1</version>
</dependency>
<dependency>
  <groupId>mysql</groupId>
  <artifactId>mysql-connector-java</artifactId>
  <version>8.0.13</version>
</dependency>
<dependency>
  <groupId>c3p0</groupId>
  <artifactId>c3p0</artifactId>
  <version>0.9.1.2</version>
</dependency>

<dependency>
  <groupId>junit</groupId>
  <artifactId>junit</artifactId>
  <version>4.12</version>
</dependency>
<dependency>
  <groupId>javax.servlet</groupId>
  <artifactId>jstl</artifactId>
  <version>1.2</version>
</dependency>

<dependency>
  <groupId>jstl</groupId>
  <artifactId>jstl</artifactId>
  <version>1.2</version>
</dependency>
```



save页面

```html
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
    <h1>保存账户信息表单</h1>
    <form action="${pageContext.request.contextPath}/save.action" method="post">
        用户名称<input type="text" name="name"><br/>
        账户金额<input type="text" name="money"><br/>
        <input type="submit" value="保存"><br/>
    </form>
</body>
</html>
```



accountList.jsp

```html
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<html>
<head>
    <title>Title</title>
</head>
<body>

    <h1>展示账户数据列表</h1>

    <table>
        <tr>
            <th>账户id</th>
            <th>账户名称</th>
            <th>账户金额</th>
        </tr>

        <c:forEach items="${accountList}" var="account">
            <tr>
                <td>${account.id}</td>
                <td>${account.name}</td>
                <td>${account.money}</td>
            </tr>
        </c:forEach>

    </table>

</body>
</html>
```







controller

```java
@Controller
@RequestMapping("/account")
public class AccountController {
    @Autowired
    private AccountService accountService;

    //保存
    @RequestMapping(value = "/save", produces = "text/html;charset=utf-8")
    @ResponseBody//直接展示字符串
    public String save(Account account) throws IOException {
        accountService.save(account);
        return "保存成功";
    }

    //查询
    @RequestMapping("/findAll")
    public ModelAndView findAll() throws IOException {
        ModelAndView modelAndView = new ModelAndView();
        List<Account> accountList = accountService.findAll();
        modelAndView.addObject("accountList", accountList);
        modelAndView.setViewName("accountList");
        return modelAndView;
    }
}
```





service

```java
@Service("accountService")
public class AccountServiceImpl implements AccountService {


    @Override
    public void save(Account account) throws IOException {
        InputStream resourceAsStream = Resources.getResourceAsStream("sqlMapConfig.xml");
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);
        SqlSession sqlSession = sqlSessionFactory.openSession();
        AccountMapper mapper = sqlSession.getMapper(AccountMapper.class);
        mapper.save(account);
        sqlSession.commit();
        sqlSession.close();
    }

    @Override
    public List<Account> findAll() throws IOException {
        InputStream resourceAsStream = Resources.getResourceAsStream("sqlMapConfig.xml");
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);
        SqlSession sqlSession = sqlSessionFactory.openSession();
        AccountMapper mapper = sqlSession.getMapper(AccountMapper.class);
        List<Account> accountList = mapper.findAll();
        sqlSession.close();
        return accountList;
    }
}
```



accountMapper

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.ssmDemo.mapper.AccountMapper">
    <insert id="save" parameterType="account">
        insert into account values(#{id}, #{name}, #{money})
    </insert>
    
    <select id="findAll" resultType="account">
        select * from account
    </select>
</mapper>
```





sqlMapConfig

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "http://mybatis.org/dtd/mybatis-3-config.dtd">


<configuration>

<!--    加载properties文件-->
    <properties resource="jdbc.properties"/>

    <typeAliases>
<!--        <typeAlias type="com.ssmDemo.domain.Account" alias="account"/>-->
        <package name="com.ssmDemo.domain"/>
    </typeAliases>

    <!--数据源环境-->
    <environments default="development">
        <environment id="development">
            <!--事务管理器-->
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="${jdbc.driver}"/>
                <property name="url" value="${jdbc.url}"/>
                <property name="username" value="${jdbc.user}"/>
                <property name="password" value="${jdbc.password}"/>
            </dataSource>
        </environment>
    </environments>
    
    
    <mappers>
<!--        <mapper resource="com/ssmDemo/mapper/AccountMapper.xml"/>-->
        <package name="com.ssmDemo.mapper"/>
    </mappers>

</configuration>
```











![image-20210210002450003](../picture/Mybatis%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0/image-20210210002450003.png)

中文字符显示乱码

> 乱码过滤器设置的是请求数据时获取的编码

```java
//保存
@RequestMapping(value = "/save", produces = "text/html;charset=utf-8")
@ResponseBody//直接展示字符串
public String save(Account account) throws IOException {
    accountService.save(account);
    return "保存成功";
}
```









## spring整合mybatis





弊端：

service层每次都需要加载mybatis配置文件，手动提交



**spring注入 accountMapper，直接使用**



![image-20210210025235328](../picture/Mybatis%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0/image-20210210025235328.png)







---

**将SqlSessionFactory配置到Spring容器中**



mybatis中使用pooled，即mybatis配置的连接池

![image-20210210032702557](../picture/Mybatis%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0/image-20210210032702557.png)







mybatis-spring 中，配置dataSource和配置文件

`sqlSessionFactory`是一个接口， `sqlSessionFactoryBean`是它的实现类，配置到spring容器中

![image-20210210031357996](../picture/Mybatis%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0/image-20210210031357996.png)



**其中dataSource使用c3p0连接池**







```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
http://www.springframework.org/schema/context
http://www.springframework.org/schema/context/spring-context.xsd
http://www.springframework.org/schema/mvc
http://www.springframework.org/schema/mvc/spring-mvc.xsd
http://www.springframework.org/schema/tx
http://www.springframework.org/schema/tx/spring-tx.xsd
http://www.springframework.org/schema/aop
http://www.springframework.org/schema/aop/spring-aop.xsd
">


    <context:component-scan base-package="com.ssmDemo">
        <context:exclude-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
    </context:component-scan>

    
<!--加载配置文件-->
    <context:property-placeholder location="classpath:jdbc.properties"/>

    <!--    配置数据源对象-->
    <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
        <property name="driverClass" value="${jdbc.driver}"/>
        <property name="jdbcUrl" value="${jdbc.url}"/>
        <property name="user" value="${jdbc.user}"/>
        <property name="password" value="${jdbc.password}"/>
    </bean>

<!--    配置sessionFactory-->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
<!--        connection连接源于datasource-->
        <property name="dataSource" ref="dataSource"/>
<!--        mybatis配置信息/映射/...-->
        <property name="configLocation" value="classpath:sqlMapConfig.xml"/>
    </bean>

    
<!--    扫描mapper所在的包 为mapper创建实现类-->
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <property name="basePackage" value="com.ssmDemo.mapper"/>
    </bean>

    
    
<!--    声明式事务控制-->
<!--    平台事务管理器-->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource"/>
    </bean>

<!--     通知 事务的增强  平台事务管理器-->
    <tx:advice id="txAdvice">
        <tx:attributes>
            <tx:method name="*"/>
        </tx:attributes>
    </tx:advice>

<!--    事务的aop织入-->
    <aop:config>
        <aop:advisor advice-ref="txAdvice" pointcut="execution(* com.ssmDemo.service.impl.*.*(..))"/>
    </aop:config>

    
</beans>
```





springMapConfig.xml

```xml
    <typeAliases>
<!--        <typeAlias type="com.ssmDemo.domain.Account" alias="account"/>-->
        <package name="com.ssmDemo.domain"/>
    </typeAliases>
```







```java
@Service("accountService")
public class AccountServiceImpl implements AccountService {

    //扫描完mapper接口后  spring会自动创建接口的实现，并存放到容器中，直接注入即可
    @Autowired
    private AccountMapper accountMapper;

    @Override
    public void save(Account account) throws IOException {
        accountMapper.save(account);

    }

    @Override
    public List<Account> findAll() throws IOException {
        return accountMapper.findAll();

    }
}
```



























































































































# TIP



## 将数据库结果封装到对象中



使用的是`set`方法，根据**set后面的字段值**来匹配**数据库中的属性值**，而不是pojo对象中的 **参数名称**



![image-20210209043412163](../picture/Mybatis%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0/image-20210209043412163.png)



```java
public void setRolena(String rolename) {
    this.rolena = rolename;
}
//[Role{id=3, rolename='null', roleDesc='写代码'}]
```



```java
public void setRolename(String rolena) {
    this.rolena = rolena;
}
//[Role{id=3, rolename='programmer', roleDesc='写代码'}]
```



---





另一个例子：

```java
private String roleDess;

public void setRoleDesc(String roleDes) {
    this.roleDess = roleDes;
}
//[Role{id=3, rolename='programmer', roleDesc='写代码'}]
```

**充分说明mybatis就只是根据set方法的名称来判断匹配数据库的哪一个属性的**



**并且也不区分大小写**





```java
private String username;

public void setUserna(String username) {
    this.username = username;
}
```



![image-20210209050237449](../picture/Mybatis%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0/image-20210209050237449.png)

**只能匹配到set方法，也可以实现封装到对象中的功能**







----



![image-20210209045127963](../picture/Mybatis%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0/image-20210209045127963.png)

这里的property是实体类中的属性名称，

- 如果没有setter方法，则会在匹配上参数名称后自动生成setter方法
- 如果属性名称无法对应，但又名字能对应的setter方法





![image-20210209045533025](../picture/Mybatis%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0/image-20210209045533025.png)

















## jstl包



导入失败？



![image-20210210003614158](../picture/Mybatis%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0/image-20210210003614158.png)



这是一个Jar包的问题，我原来引入的jar是：

```xml
<dependency>
  <groupId>jstl</groupId>
  <artifactId>jstl</artifactId>
  <version>1.2</version>
</dependency>
```

**改成这样就行：**

```xml
<dependency>
  <groupId>javax.servlet</groupId>
  <artifactId>jstl</artifactId>
  <version>1.2</version>
</dependency>
```





















