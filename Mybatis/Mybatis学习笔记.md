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































































































