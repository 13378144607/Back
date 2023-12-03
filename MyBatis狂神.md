# MyBatis学习

# 1.简介

## 1.1什么是mybatis

- MyBatis 是一款优秀的**持久层框架**
- 它支持自定义 SQL、存储过程以及高级映射。
- MyBatis 免除了几乎所有的 JDBC 代码以及设置参数和获取结果集的工作。
- MyBatis 可以通过简单的 XML 或注解来配置和映射原始类型、接口和 Java POJO（Plain Old Java Objects，普通老式 Java 对象）为数据库中的记录。

如何获得Mybatis

- maven仓库：

  ```xml
  <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis</artifactId>
      <version>3.5.13</version>
  </dependency>
  ```

- Github

- Mybatis官方文档：[mybatis – MyBatis 3 | 简介](https://mybatis.org/mybatis-3/zh/index.html)

## 1.2持久化

数据持久化

- 持久化就是将程序的数据在持久状态和瞬时状态转化的过程
- 内存：**断电即失**
- 数据库（jdbc），io文件持久化

## 1.3持久层

Dao层，Service层，Controller层......

- 完成持久化工作的代码块。
- 界限十分明显。

## 1.4、为什么需要Mybatis？

- 方便
- 传统的JDBC代码太复杂、简化、框架、自动化
- 帮助程序员将数据存入到数据库中
- 优点
  - 简单易学
  - 灵活
  - sql和代码分离，提高可维护性
  - 提供映射 标签，支持对象与数据库的orm字段关系映射
  - 提供对象关系映射标签，支持对象关系组件维护
  - 提供xml标签，支持编写动态sql

# 2.第一个Mybatis程序

思路：搭建环境--->导入Mybatis--->编写代码--->测试

## 2.1、搭建环境

搭建数据库

新建项目

1. 新建一个普通的maven项目

2. 删除src目录

3. 导入maven依赖

   ```xml
   <dependencies>
       <dependency>
           <groupId>mysql</groupId>
           <artifactId>mysql-connector-java</artifactId>
           <version>5.1.46</version>
       </dependency>
       <dependency>
           <groupId>org.mybatis</groupId>
           <artifactId>mybatis</artifactId>
           <version>3.5.5</version>
       </dependency>
       <dependency>
           <groupId>junit</groupId>
           <artifactId>junit</artifactId>
           <version>4.13</version>
           <scope>test</scope>
       </dependency>
   </dependencies>
   ```

   

## 2.2创建一个模块

- 编写mybatis的核心配置文件,注意其中url的配置
  ```xml
  <?xml version="1.0" encoding="UTF-8" ?>
  <!DOCTYPE configuration
          PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
          "https://mybatis.org/dtd/mybatis-3-config.dtd">
  <!--核心配置文件-->
  <configuration>
      <environments default="development">
          <environment id="development">
              <transactionManager type="JDBC"/>
              <dataSource type="POOLED">
                  <property name="driver" value="com.mysql.jdbc.Driver"/>
                  <property name="url" value="jdbc:mysql://localhost:3306/mybatis?useSSL=false&amp;useUnicode=true&amp;characterEncoding=utf8"/>
                  <property name="username" value="root"/>
                  <property name="password" value="040615"/>
              </dataSource>
          </environment>
      </environments>
  
  </configuration>
  ```
  
- 编写mybatis工具类 
  ```java
  package com.xuan.utils;
  
  import org.apache.ibatis.io.Resources;
  import org.apache.ibatis.session.SqlSession;
  import org.apache.ibatis.session.SqlSessionFactory;
  import org.apache.ibatis.session.SqlSessionFactoryBuilder;
  
  import java.io.IOException;
  import java.io.InputStream;
  
  //sqlSessionFactory ---> sqlSession
  public class MybatisUtils {
      private static SqlSessionFactory  sqlSessionFactory;
  
      static {
          try {
              //第一步：
              //使用Mybatis获取sqlSessionFactory对象
              String resource = "mybatis-config.xml";
              InputStream inputStream = Resources.getResourceAsStream(resource);
              sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);//使用了匿名内部类
          } catch (IOException e) {
              throw new RuntimeException(e);
          }
      }
  
      //既然有了 SqlSessionFactory，顾名思义，我们可以从中获得 SqlSession 的实例
      // SqlSession 提供了在数据库执行 SQL 命令所需的所有方法。你可以通过 SqlSession 实例来直接执行已映射的 SQL 语句。
      public static SqlSession getSqlSession(){
          return sqlSessionFactory.openSession();
      }
  }
  
  ```

  

## 2.3、编写代码

- 实体类
  ```java
  package com.xuan.pojo;
  
  //实体类
  public class User {
      private int id;
      private String name;
      private String pwd;
  
      public int getId() {
          return id;
      }
  
      public void setId(int id) {
          this.id = id;
      }
  
      @Override
      public String toString() {
          return "User{" +
                  "id=" + id +
                  ", name='" + name + '\'' +
                  ", pwd='" + pwd + '\'' +
                  '}';
      }
  
      public String getName() {
          return name;
      }
  
      public void setName(String name) {
          this.name = name;
      }
  
      public String getPwd() {
          return pwd;
      }
  
      public void setPwd(String pwd) {
          this.pwd = pwd;
      }
  
      public User(int id, String name, String pwd) {
          this.id = id;
          this.name = name;
          this.pwd = pwd;
      }
  
      public User() {
      }
  }
  
  ```

  

- Mapper接口
  ```java
  package com.xuan.dao;
  
  import com.xuan.pojo.User;
  
  import java.util.List;
  
  public interface UserMapper {//实现数据库的增删改查方法
      List<User> getUserList();
  }
  
  ```

  

- 接口实现类,由原来的UserDaoImpl转变成一个Mapper配置文件
  ```xml
  <?xml version="1.0" encoding="UTF-8" ?>
  <!DOCTYPE mapper
          PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
          "https://mybatis.org/dtd/mybatis-3-mapper.dtd">
  <!--namespace=绑定 一个对应的Dao/Mapper-->
  <mapper namespace="com.xuan.dao.UserMapper">
      <!--id是接口中的抽象方法名，resultType是这个方法的返回值类型-->
      <select id="getUserList" resultType="com.xuan.pojo.User">
          select * from mybatis.user;
      </select>
  </mapper>
  ```

## 2.4、测试

注意点：org.apache.ibatis.binding.BindingException: Type interface com.xuan.dao.UserDao is not known to the MapperRegistry.（这个报错就是在Mybatis的核心配置文件中没有注册Mapper导致的，此配置文件用来实现接口中的抽象方法）

**MapperRegistry是什么?**

核心配置文件中注册mappers

- junit测试
  ```java
  package com.xuan.dao;
  
  import com.xuan.pojo.User;
  import com.xuan.utils.MybatisUtils;
  import org.apache.ibatis.session.SqlSession;
  import org.junit.Test;
  
  import java.util.List;
  
  public class UserDaoTest {
      @Test
      public void test(){
          //第一步：获得SqlSession对象
          SqlSession sqlSession = MybatisUtils.getSqlSession();
          //执行sql方式一：getMapper()
          UserDao userDao = sqlSession.getMapper(UserMapper.class);
          List<User> userList = userDao.getUserList();
  
          for (User user : userList) {
              System.out.println(user);
          }
          //关闭Session
          sqlSession.close();
  
      }
  }
  
  ```

  可能遇到的问题

  1. 配置文件没有注册
  2. 绑定接口错误
  3. 方法名不对
  4. 返回类型不对
  5. **Maven导不出资源问题**（需要再pom文件中添加相应的build进行配置）
  6. **sql配置文件中的url的useSSL属性不能为true应该是false**（属性useSSL的取值取决于你导入的MySQL包的版本。在较旧的MySQL版本中，useSSL设置为false表示不使用SSL加密连接，而设置为true表示使用SSL加密连接。而在较新的MySQL版本中，useSSL设置为false表示使用不安全的连接，而设置为true表示使用安全的连接。**5.xx版本推荐为默认的false，如果配置了SSL证书则可填true；8.xx版本推荐为true**）

## 2.5、总结

**七个步骤完成**

![image-20231007134242265](C:\Users\LRX\AppData\Roaming\Typora\typora-user-images\image-20231007134242265.png)

# 3.CRUD

## 3.1、namespace

namespace中的包名要和Dao/mapper接口的包名一致

## 3.2、select 

选择，查询语句：

- id：就是对应的namespace中的方法名
- resultType：sql语句执行的返回值类型
- parameterType：参数类型（对应的方法的参数列表中的参数的类型，如：int，String，如果是一个自定义类的话就是一个全路径）

1. ​	编写接口

   ```java
   public interface UserMapper {
       //查询全部用户
       List<User> getUserList();
       //根据ID查询用户
       User getUserById(int id);
       //insert一个用户
       int insertUser(User user);//int是影响行数
       //修改用户
       int updateUser(User user);
       //删除用户
       int deleteUser(int id);
   
   }
   ```

2. 编写对应的mapper中的sql语句

   ```xml
   <mapper namespace="com.xuan.dao.UserMapper">
   <!--  去java文件中找对应的方法及返回类型，id必须和dao中的方法名一样  -->
       <select id="getUserList" resultType="com.xuan.pojo.User">
           select * from mybatis.user;
       </select>
       <select id="getUserById" parameterType="int" resultType="com.xuan.pojo.User">
           select * from mybatis.user where id = #{id}
       </select>
   <!--  对象中的属性能够直接取出来  -->
       <insert id="insertUser" parameterType="com.xuan.pojo.User">
           insert into mybatis.user (id,name,pwd) values (#{id},#{name},#{pwd});
       </insert>
       <update id="updateUser" parameterType="com.xuan.pojo.User">
           update mybatis.user set name=#{name},pwd=#{pwd} where id = #{id};
       </update>
       <delete id="deleteUser" parameterType="int">
           delete from mybatis.user where id = #{id};
       </delete>
   </mapper>
   ```

3. 测试：特别注意增删改需要提交事务 

   ```java
   @Test
       public void getUserById(){
           //1.获取Session对象
           SqlSession sqlSession = MybatisUtils.getSqlSession();
   
           //2.获得接口
           UserMapper mapper = sqlSession.getMapper(UserMapper.class);
           User user = mapper.getUserById(1);
           System.out.println(user);
           
           //如果是增删改则必须在此处进行事务提交
           //sqlSession.commit();
   
           //3.关闭资源
           sqlSession.close();
       }
   ```

   

## 3.3、Insert

```xml
<insert id="insertUser" parameterType="com.xuan.pojo.User">
        insert into mybatis.user (id,name,pwd) values (#{id},#{name},#{pwd});
    </insert>
```



## 3.4、update

```xml
<update id="updateUser" parameterType="com.xuan.pojo.User">
        update mybatis.user set name=#{name},pwd=#{pwd} where id = #{id};
    </update>
```



## 3.5、delete

```xml
<delete id="deleteUser" parameterType="int">
        delete from mybatis.user where id = #{id};
    </delete>
```

**注意点：增删改需要提交事务**

## 3.6、易错点

- 标签不要匹配错
- resource绑定mapper，需要使用路径/
- 程序配置文件必须符合规范
- NullPointerException，没有注册到资源
- 输出的xml文件中存在中文乱码问题
- Maven资源没有导出问题

## 3.7、万能的Map

假设，我们的实体类，或者数据库中的表，字段或者参数过多，我们应该考虑使用Map

注意：map中的key应该和#()中的值名字相对应

```java
//万能的map进行插入
    int insertUser2(Map<String,Object> map);
```

```java
 @Test
    public void insertUser2(){
        //1.获取SqlSession对象
        SqlSession sqlSession = MybatisUtils.getSqlSession();
        //2.获取接口
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);

        HashMap<String, Object> map = new HashMap<String, Object>();
        map.put("userId",5);
        map.put("userName","小六");
        map.put("userPwd","123456");

        int i = mapper.insertUser2(map);
        if(i > 0){
            System.out.println("成功");
        }
        //4.提交事务
        sqlSession.commit();

        //5.关闭资源
        sqlSession.close();
    }
```

```xml
<!--  使用map进行插入  -->
    <insert id="insertUser2" parameterType="map">
        insert into mybatis.user (id,name,pwd) values (#{userId},#{userName},#{userPwd});
    </insert>
```

Map传递参数，直接在sql中取出key即可，**Map传参的话sql语句中的属性可以自定义名字**

对象传递参数，直接在sql中取对象的属性即可

只有一个基本类型参数的情况下，可以直接在sql中取到

多个参数用Map，或者注解

## 3.8、思考题

模糊查询怎么写？

1. Java代码执行的时候，传递通配符% %

   ```java
           List<User> userList = mapper.getUserLike("%李%");
   
   ```

2. 在sql拼接中 使用通配符

   ```xml
   select * from mybatis.user where name like "%"#{value}"%"
   ```

   

# 4.配置解析##

## 4.1、核心配置文件

- mybatis-config.xml

- MyBatis的配置文件包含了会深深影响Mybatis行为的设置和属性信息

  ```xml
  configuration（配置）
  properties（属性）
  settings（设置）
  typeAliases（类型别名）
  typeHandlers（类型处理器）
  objectFactory（对象工厂）
  plugins（插件）
  environments（环境配置）
  environment（环境变量）
  transactionManager（事务管理器）
  dataSource（数据源）
  databaseIdProvider（数据库厂商标识）
  mappers（映射器）
  ```

## 4.2、配置环境（environments）

Mybatis可以配置成适应多种环境

**不过要记住：尽管可以配置多个环境，但每个SqlSessionFactory实例只能选择一种环境**

学会使用配置多套运行环境！

Mybatis默认的事务管理器是JDBC（但是事务管理器不止JDBC，还有一个MANAGED），连接池：POOLED

## 4.3、属性（properties）

我们可以通过properties属性来实现引用文件

编写一个配置文件

db.properties

```properties
driver="com.mysql.jdbc.Driver"
url=""jdbc:mysql://localhost:3306/mybatis?useSSL=false&useUnicode=true&characterEncoding=utf8""
username=root
password=040615
```

在核心配置文件中引入，程序运行时最先读取的，所以一定要放最上面

在核心配置文件中 引入

```xml
<configuration>
	<properties resource="db.properties"/>
    ......
</configuration>


```

- 可以直接引入外部文件
- 可以在其中增加一些属性配置
- 如果连个文件有同一个字段，优先使用外部配置文件(约定大于配置)

## 4.4、类型别名（typeAliases）

类型别名是为java类型 设置一个短名字

存在的意义仅在于用来减少类似完全限定名的冗余

1. 1自定义别名

```xml
<!--可以给实体类取别名-->
<typeAliases>
    <typeAlias type="com.xuan.pojo.User" alias="User"/>
</typeAliases>
```

```java
<select id="getUserList" resultType="User">
        select * from mybatis.user;
    </select>
```

1. 2、也可以指定一个包名，Mybatis会在指定的包下面搜索需要的JavaBean，比如：

每一个在包 `domain.blog` 中的 Java Bean，在没有注解的情况下，会使用 Bean 的首字母小写的非限定类名来作为它的别名。 比如 `domain.blog.Author` 的别名为 `author`；若有注解，则别名为其注解值。见下面的例子：

```xml
<typeAliases>
    <package name="com.xuan.pojo.User"/>
</typeAliases>
```

```java
<select id="getUserList" resultType="user">
        select * from mybatis.user;
    </select>
```

- 在实体类比较少的时候 ，使用第一种
- 实体类十分多，则使用第二种
- 区别：第一种可以自定义别名；第二种只能使用默认的，如果非要改则需要在实体上增加注解：（使用注解要先进行包扫描）

```java
@Alias("xxx")
public class User{......}
```

```java
<select id="getUserList" resultType="xxx">
        select * from mybatis.user;
    </select>
```

## 4.5、设置

## 4.6其他配置

- [typeHandlers（类型处理器）](https://mybatis.org/mybatis-3/zh/configuration.html#typeHandlers)
- [objectFactory（对象工厂）](https://mybatis.org/mybatis-3/zh/configuration.html#objectFactory)
- [plugins（插件）](https://mybatis.org/mybatis-3/zh/configuration.html#plugins)

## 4.7、映射器（mappers）

MapperRegister：注册绑定我们的Mapper文件；

方式一：默认方式

使用此方法就必须要在dao层下创建一个Mapper.xml文件来配置sql语句和一些格式

```xml
<mappers>
	<mapper resource="com.xuan.dao.UserMapper.xml"/>
</mappers>
```

方式二：使用class文件绑定注册

使用此方法可以使用**注解编程**

```xml
<mappers>
    <mapper class="com.xuan.dao.UserMapper"/>
</mappers>
```

注意点：

- 接口和它的Mapper配置文件必须同名
- 接口和它的Mapper配置文件必须在同一包下

方式三：使用扫描包进行注入绑定

```xml
<mappers>
    <package name="com.xuan.dao"/>
</mappers>
```

注意点：

- 接口和它的Mapper配置文件必须同名
- 接口和它的Mapper配置文件必须在同一包下

## 4.8、生命周期和作用域

生命周期，作用域都是至关重要的，因为错误的使用会导致非常严重的**并发性问题**

**SqlSessionFactoryBuilder：**

- 一旦创建了 SqlSessionFactory，就不再需要它了
- 局部变量

**SqlSessionFactory：**

- 可以想象为数据库连接池
- SqlSessionFactory一旦被创建就应该在应用的运行期间一直存在，**没有任何理由丢弃它嚯重新创建另一个实例。**
- 最简单的就是使用单例模式或者静态单例模式。保证全局只有一个变量

**SqlSession：**

- 连接到连接池的一个请求 
- SqlSession的实例不是线程安全的，因此是不能被共享的，所以它的最佳作用域是请求或者方法作用域
- 用完之后需要赶紧关闭，否则资源被占用

这里面的每一个Mapper，就代表一个业务

# 5.解决属性名和字段名不一致的问题

## 5.1、问题

数据库中的字段![image-20231008111331490](C:\Users\LRX\AppData\Roaming\Typora\typora-user-images\image-20231008111331490.png)

新建一个项目，拷贝之前的，测试实体类字段不一致情况

```java
public class User{
    private int id;
    private String name;
    private String password;
}
```

测试出现问题

![image-20231008131912768](C:\Users\LRX\AppData\Roaming\Typora\typora-user-images\image-20231008131912768.png)

```java
//select * from mybatis.user where id = #{id}
//类型处理器
//select id,name,pwd from mybatis.user where id = #{id}
```

解决 方法：

- 起别名

  ```xml
  <select id="getUserById" resultType="com.xuan.pojo.User">
  	selec id ,name pwd as password form mybatis.user where id = #{id}
  </select>
  ```

## 5.2、resultMap

结果集映射

```
id  name pwd
id  name password
```

```xml
<mapper namespace="com.xuan.dao.UserMapper">
    <!--  结果集映射  -->
    <resultMap id="UserMap" type="User">
    <!--    column数据库中的字段，property实体类中的属性    -->
        <result column="id" property="id"/>
        <result column="name" property="name"/>
        <result column="pwd" property="password"/>
    </resultMap>

    <select id="getUserById" parameterType="int" resultMap="UserMap">
        select * from mybatis.user where id = #{id}
    </select>
</mapper>
```

- resultMap元素是MyBatis中最重要最强大的元素 
- ResultMap的设计 思想是，对于简单的语句根本不需要配置显示的结果集映射，而对于复杂的语句只需要描述他们的关系就行
- ResultMap最优秀的地方在于，虽然你已经对它相当了解了，但是根本就不需要显式地用它们

# 6.日志

## 6.1、日志工厂

如果一个数据库操作，出现了异常，我们需要排错，日志就是最好的助手。

曾经：sout、debug

现在：日志工厂

![image-20231008140552975](C:\Users\LRX\AppData\Roaming\Typora\typora-user-images\image-20231008140552975.png)

- SLF4J 
-  LOG4J（3.5.9 起废弃） 【掌握】
-  LOG4J2 
-  JDK_LOGGING |
- COMMONS_LOGGING 
- STDOUT_LOGGING 【掌握】
-  NO_LOGGING

在MyBatis中具体使用哪一个日志实现，在设置中设定

**STDOUT_LOGGING标准日志输出**

在mybatis核心配置文件中，配置我们的日志

```xml
<configuration>
	...
    <settings>
        <setting name="logImpl" value="STDOUT_LOGGING"/>
    </settings>
    ...
</configuration>
```

## 6.2、LOG4J 

什么是Log4j？

- Log4j是Apache的一个开源项目，通过使用Log4j，我们可以控制 日志信息输出的目的地是控制台、文件、GUI组件
- 我们也可以控制每一条日志的输出格式
- 通过定一个每一条日志信息 的级别，我们能够更加细致地控制日志的生成过程
- 通过一个配置文件来 灵活地进行配置，而不需要修改代码

1. 先导入Log4j的包
   ```xml
   <dependencies>
       <!-- https://mvnrepository.com/artifact/log4j/log4j -->
       <dependency>
           <groupId>log4j</groupId>
           <artifactId>log4j</artifactId>
           <version>1.2.12</version>
       </dependency>
   </dependencies>
   ```

2. log4j.properties配置文件

   ```properties
   #将等级为DEBUG的日志信息输出到console和file这两个目的地，console和file的定义在下面的代码
   log4j.rootLogger=DEBUG,console,file
   
   #控制台输出的相关设置
   log4j.appender.console = org.apache.log4j.ConsoleAppender
   log4j.appender.console.Target = System.out
   log4j.appender.console.Threshold=DEBUG
   log4j.appender.console.layout = org.apache.log4j.PatternLayout
   log4j.appender.console.layout.ConversionPattern={%c}-%m%n
   
   #文件输出的相关设置
   log4j.appender.file = org.apache.log4j.RollingFileAppender
   log4j.appender.file.File=./log/xuan.log
   log4j.appender.file.MaxFileSize=10mb
   log4j.appender.file.Threshold=DEBUG
   log4j.appender.file.layout=org.apache.log4j.PatternLayout
   log4j.appender.file.layout.ConversionPattern={%p}{%d{yy-MM-dd}}{%c}%m%n
   
   #日志输出级别
   log4j.logger.org.mybatis=DEBUG
   log4j.logger.java.sql=DEBUG
   log4j.logger.java.sql.Statement=DEBUG
   log4j.logger.java.sql.ResultSet=DEBUG
   log4j.logger.java.sql.PreparedStatement=DEBUG
   ```

3. 配置 log4j为日志的实现

   ```xml
   <configuration>
   	...
       <settings>
           <setting name"logImpl" value="LOG4J"
       </settings>
       ...
   </configuration>
   ```

4. log4j的使用，直接测试运行刚才的查询 ![image-20231008144421978](C:\Users\LRX\AppData\Roaming\Typora\typora-user-images\image-20231008144421978.png)

**简单使用**

1. 在要使用Log4j的类中，导入包import org.apache.log4j.Logger;

2. 日志对象，参数为当前类的class

   ```java
   static Logger logger = Logger.getLogger(UserDaoTest.class);
   ```

3. 日志级别

   ```java
   logger.info("info：进入了testlog4j");
   logger.debug("debug：进入了testlog4j");
   logger.error("error：进入了testlog4j");
   ```

# 7.分页

## 7.1**使用LIMIT分页**

```sql
select * from user limit startIndex,pageSize;
select * from user limit 3; #[0,n]
```

使用mybatis实现分页，核心SQL

1. 接口

   ```java
   //分页    
   List<User> getUserByLimit(Map<String,Object> map);
   
   ```

   

2. Mapper.xml

   ```xml
   <!--  分页  -->
       <select id="getUserByLimit" parameterType="map" resultMap="UserMap" >
           select * from mybatis.user limit #{startIndex},#{pageSize};
       </select>
   ```

   

3. 测试

   ```java
    @Test
       public void getUserByLimit(){
           //获取Session
           SqlSession sqlSession = MybatisUtils.getSqlSession();
   
           //获得接口
           UserMapper mapper = sqlSession.getMapper(UserMapper.class);
   
           HashMap<String, Object> map = new HashMap<String, Object>();
           map.put("startIndex",0);
           map.put("pageSize",2);
   
           List<User> userList = mapper.getUserByLimit(map);
           for (User user : userList) {
               System.out.println(user);
           }
   
           //关闭资源
           sqlSession.close();
       }
   ```

## 7.2、RowBounds分页

## 7.3、分页插件

# 8.注解开发

注意：仅使用在单一语句上，复杂语句不推荐使用

## 8.1、使用注解开发

1. 注解在接口上实现

   ```java
   @Select("select * from user") List<user> getUsers();
   ```

2. 需要在核心配置文件中绑定接口

   ```xml
   <!--  绑定接口  -->
       <mappers>
           <mapper class="com.xuan.dao.UserMapper"/>
       </mappers>
   ```

3. 测试

本质：反射机制实现

底层：动态代理

## 8.2、CRUD

MyBatisUtils类中的

```java
    public static SqlSession getSqlSession(){
        return sqlSessionFactory.openSession(true);//相当于是事务自动提交，就不用在增删改中添加commit了
    }
```

编写接口，增加注解，省去了写Mapper.xml，前提是Mybatis.xml文件中进行绑定

```java
package com.xuan.dao;


import com.xuan.pojo.User;
import org.apache.ibatis.annotations.*;

import java.util.List;
import java.util.Map;

public interface UserMapper {
    @Select("select * from user")
    List<User> getUsers();

    @Select("select * from user where id = #{id}")
    User getUserById(@Param("id") int id);//方法如果存在多个参数则所有参数前面必须加上@Param("")注解，编译器默认是优先先从@Param中取值

    @Insert("insert into user (id,name,pwd) values (#{id},#{name},#{password})")
    int insertUser(User user);

    @Update("update user set name = #{name},pwd = #{password} where id = #{id}")
    int updateUser(User user);

    @Delete("delete from user where id = #{id}")
    int deleteUser(@Param("id") int id);
}


```

测试类：略

【注意：我们必须要将接口注册绑定到注册文件中】

## 8.3、关于`@Param("")`注解

- 基本类型的参数或者String类型，需要加上
- 引用类型不需要加
- 如果只有一个基本类型，可以忽略，但是建议加上，加上的话就默认优先使用@Param()中的参数
- 我们在SQL中引用的就是我们这里的@Param()中设定的属性名

# 9.Lombok（有手的话不建议使用）

```
Lombok项目是一个java库，它可以自动插入到编辑器和构建工具中，增强java的性能。不需要再写getter、setter或equals方法，只要有一个注解，就有一个功能齐全的构建器、自动记录变量等等。
```

使用步骤：

1. 在IDEA中安装Lombok插件（不可缺失）

2. 在项目中导入Lombok的jar包（不可缺失）

   ```xml
   <dependency>
       <groupId>org.projectlombok</groupId>
       <artifactId>lombok</artifactId>
       <version>1.18.24</version>
   </dependency>
   ```

   

3. 在实体类上加注解即可

   掌握：

   ```
   @Data：无参构造、getter、setter、tostring、hashcode，equals等等
   {
   @AllArgsConstructor：有参构造
   @NoArgsConstructor：无参构造
   这两个写了其中一个的话就没有另一个，想要都有则要写两个
   }
   @Getter and @Setter：放在类上面就生成类中所有的，放在起其中一个属性上面就生成其中一个属性的
   @ToString：toString
   ```

   

4. 总体 

   ```java
   @Getter and @Setter
   @FieldNameConstants
   @ToString
   @EqualsAndHashCode
   @AllArgsConstructor, @RequiredArgsConstructor and @NoArgsConstructor
   @Log, @Log4j, @Log4j2, @Slf4j, @XSlf4j, @CommonsLog, @JBossLog, @Flogger, @CustomLog
   @Data
   @Builder
   @SuperBuilder
   @Singular
   @Delegate
   @Value
   @Accessors
   @Wither
   @With
   @SneakyThrows
   @StandardException
   @val
   @var
   experimental @var
   @UtilityClass
   ```

   **优点：**

   1. 减少样板代码：Lombok可以通过注解自动生成Java类中的常见方法，例如Getter和Setter方法、构造函数等，从而减少重复的样板代码，提高开发效率。
   2. 简化属性访问：通过使用注解，Lombok可以自动生成Getter和Setter方法，使得属性的访问更加简洁和易读。
   3. 代码可读性：Lombok生成的代码非常简洁，不仅减少了冗余代码的量，还提高了代码的可读性和可维护性。
   4. 可扩展性：Lombok提供了许多注解，可以根据需要扩展自定义功能，并且可以与其他框架和库无缝集成。

   **缺点：**

   1. 学习成本：虽然Lombok可以减少样板代码，但需要掌握其特定的注解和用法才能充分利用其功能，这可能需要花费一些时间学习。
   2. IDE支持：虽然大多数主流的Java IDE（如Eclipse和IntelliJ IDEA）都支持Lombok，但在某些情况下，IDE可能无法正确解析Lombok生成的代码，导致一些问题或调试困难。
   3. 依赖性：在使用Lombok的项目中，必须包含Lombok的依赖库，并且开发团队成员都需要了解和使用Lombok，这可能会对项目的依赖管理和团队合作造成一定影响。
   4. 使用Lombok出来的构造器不能进行重载，但是可以手写重载

# 10.多对一处理（association）

- 多个学生，对应一个老师			

## 测试环境搭建

1. 导入Lombok
2. 新建实体类Teacher，Student
3. 建立Mapper接口
4. 建立Mapper.xml文件
5. 在核心配置文件中绑定我们的 Mapper接口或者文件【方式很多】
6. 测试查询是否能够成功

## 按照查询嵌套处理

```xml
<mapper namespace="com.xuan.dao.StudentMapper">
<!--
    1.查询所有学生信息
    2.根据查询出来的学生tid，查询对应的老师
  -->

    <select id="getStudent" resultMap="StudentTeacher">
        select * from student
    </select>

    <resultMap id="StudentTeacher" type="Student">
        <result property="id" column="id"/>
        <result property="name" column="name"/>
        <!--    复杂的属性需要单独处理 对象：association集合：collection -->
        <association property="teacher" column="tid" javaType="Teacher" select="getTeacher"/>
    </resultMap>

    <select id="getTeacher" resultType="Teacher">
        select * from teacher where id = #{id}
    </select>
</mapper>
```

## 按照结果嵌套处理

```xml
<mapper>
    <select id="getStudent2" resultMap="StudentTeacher2">
        select s.id sid,s.name sname,t.name tname
        from student s,teacher t
        where s.tid = t.id
    </select>

    <resultMap id="StudentTeacher2" type="Student">
        <result property="id" column="sid"/><!--sql语句中取了别名就要用别名名字-->
        <result property="name" column="sname"/>
        <association property="teacher" javaType="Teacher"><!--相当于直接跳到了Teacher表中-->
            <result property="name" column="tname"/>
        </association><!--按结果来直接写死类型为Teacher-->
    </resultMap>
</mapper>
```

回顾Mysql多对一查询方式

- 子查询【按照查询嵌套】
- 联表查询【按照结果嵌套】

# 11.一对多处理（collection）

比如：一个老师拥有多个学生

对于老师而言，就是一对多的关系

## 环境搭建

1. 环境搭建，和上一个一样

**实体类**

```java
package com.xuan.pojo;

import lombok.Data;

@Data
public class Student {
    private int id;
    private String name;
    private int tid;
}

```

```java
package com.xuan.pojo;

import lombok.Data;

import java.util.List;

@Data
public class Teacher {
    private int id;
    private String name;

    //一个老师拥有多个学生
    private List<Student> students;
}

```

## 按照结果嵌套查询处理

```xml
<select id="getTeacherCollection" resultMap="TeacherMap">
        select s.id as sid,s.name as sname,t.name as tname,t.id tid
        from student s,teacher t
        where s.tid = t.id and t.id = #{tid};
</select>

<resultMap id="TeacherMap" type="Teacher">
    <result column="tid" property="id"/>
    <result column="tname" property="name"/>
    <!--     javaType="" 指定属性的类型
             集合中的泛型信息，我们使用ofType获取
             -->
    <collection property="students" ofType="Student">
        <result property="id" column="sid"/>
        <result property="name" column="sname"/>
        <result property="tid" column="tid"/>
    </collection>
</resultMap>
```



## 按照查询嵌套处理

```xml
<select id="getTeacherCollection2" resultMap="TeacherMap2">
        select * from mybatis.teacher where id = #{tid};
</select>

<resultMap id="TeacherMap2" type="Teacher">
    <result property="id" column="id"/>
    <result property="name" column="name"/>
    <collection property="students" column="id" javaType="ArrayList" ofType="Student" select="getStudentByTeacherId"/>
</resultMap>

<select id="getStudentByTeacherId" resultType="Student">
    select * from student where tid = #{tid}
</select>
```

## 小结

1. 关联-association【多对一】
2. 集合-collection【一对多】
3. javaType & ofType
   - JavaType用来指定实体类中属性的类型
   - ofType用来指定映射到List或者集合中的pojo类型，泛型中的约束类型

注意点：

- 保证SQL的可读性，尽量保证通俗易懂
- 注意一对多和多对一中，属性名和字段的问题
- 如果问题不好排查错误，可以使用日志，建议使用log4j

# 12.动态SQL

**什么是动态SQL：根据不同的条件生成不同的SQL语句**

```xml
如果你之前用过 JSTL 或任何基于类 XML 语言的文本处理器，你对动态 SQL 元素可能会感觉似曾相识。在 MyBatis 之前的版本中，需要花时间了解大量的元素。借助功能强大的基于 OGNL 的表达式，MyBatis 3 替换了之前的大部分元素，大大精简了元素种类，现在要学习的元素种类比原来的一半还要少。

if
choose (when, otherwise)
trim (where, set)
foreach
```

## 搭建环境

```sql
CREATE TABLE `blog`(
`id` VARCHAR(50) NOT NULL COMMENT '博客id',
`title` VARCHAR(100) NOT NULL COMMENT '博客标题',
`author` VARCHAR(30) NOT NULL COMMENT '博客作者',
`create_time` DATETIME NOT NULL COMMENT '创建时间',
`views` INT(30) NOT NULL COMMENT '浏览量'
)ENGINE=INNODB DEFAULT CHARSET=utf8
```

创建一个基础工程

1. 导包

2. 编写配置文件

3. 编写实体类

   ```java
   @Data
   public class Blog {
       private String id;
       private String title;
       private String author;
       private Date createTime;
       private int views;
   }
   ```

   

4. 编写实体类对应Mapper接口和Mapper.xml文件 

## IF

```xml
<select id="queryBlogIF" parameterType="map" resultType="Blog">
    select * from blog
	<where>
        <if test="title != null">
            title = #{title}
        </if>
        <if test="author != null">
            and author = #{author}
        </if>
    </where>
</select>
```

注意：*where* 元素只会在子元素返回任何内容的情况下才插入 “WHERE” 子句。而且，若子句的开头为 “AND” 或 “OR”，*where* 元素也会将它们去除。

```xml
<select id="queryBlogIF" parameterType="map" resultType="Blog">
    select * from blog where
        <if test="title != null">
        	and title = #{title}
        </if>
        <if test="author != null">
            and author = #{author}
        </if>
</select>
```



## Choose

```xml
<select id="queryBlogChoose" parameterType="map" resultMap="Blog">
    select * from mybatis.blog
    <where>
        <choose>
            <when test="title != null">
                title = #{title}
            </when>
            <when test="author != null">
                and author = #{author}
            </when>
            <otherwise>
                and views = #{views}
            </otherwise>
        </choose>
    </where>
</select>
```

解释：choose中的when 和otherwise好比Switch语句中的case和default只能执行成功一个case或者一个default

## set

```xml
<update id="updateBlog" parameterType="map">
    update blog
    <set>
        <if test="title != null">
            title = #{title},
        </if>
        <if test="author != null">
            author = #{author}
        </if>
    </set>
    where id = #{id};
</update>
```

注意：*set* 元素会动态地在行首插入 SET 关键字，并会删掉额外的逗号（这些逗号是在使用条件语句给列赋值时引入的）。

## where的问题![image-20231010191727633](C:\Users\LRX\AppData\Roaming\Typora\typora-user-images\image-20231010191727633.png)

![image-20231010191642228](C:\Users\LRX\AppData\Roaming\Typora\typora-user-images\image-20231010191642228.png)

![image-20231010191709242](C:\Users\LRX\AppData\Roaming\Typora\typora-user-images\image-20231010191709242.png)

**理解**：如果发现你的where语句后面的第一个子语句没有添加进去那么后面的带and的第二个子语句的and就会被去掉作为第一个子语句，如果发现where后面没有子语句则会将where自动删除

## SQL片段

有的时候，我们可能会将一些公共的部分抽取出来方便复用

1. 使用sql标签抽取公共部分

   ```xml
   <mapper namespace="">
       ........
       <sql id="if-title-author">
           <if test="title != null">
               title = #{title}
           </if>
           <if test="author != null">
               and author = #{author}
           </if>
   	</sql>
       
       
       ........
   </mapper>
   
   ```

   

2. 在需要使用的地方使用include标签引用即可

```xml
<mapper>
	<select id="queryBlogIF" parameterType="map" resultType="Blog">
        select * from blog
        <where>
            <include refid="if-title-author"></include>
        </where>
    </select>
</mapper>
```

注意：

- 最好基于单表定义sql片段
- 不要在sql标签中存在where标签

## Foreach

```sql
select * from user where 1 = 1 and (id = 1 or id = 2 or id = 3)

<foreach item="id" collection="ids"
	open="(" separator="or" close=")">
	#{id}
</foreach>
```

![image-20231011183752790](C:\Users\LRX\AppData\Roaming\Typora\typora-user-images\image-20231011183752790.png)

==动态SQL就是在拼接SQL语句，我们只要保证SQL的正确性，按照SQL的格式去排列组合就可以了==

建议：

- 先在MySQL中写出完整的SQL，再对应的去修改成为我们的动态SQL实现即可

# 13.缓存

## 13.1、简介

1. 什么是缓存[Cache]
   - 存在内存中的临时数据
   - 将用户经常查询的数据放在缓存（内存）中 ,用户去查询数据就不用从磁盘上查询，从缓存中查询，从而提高查询效率，解决了高并发系统的性能问题
2. 为什么使用缓存？
   - 减少和数据库的交互次数，减少系统开销，提高系统性能
3. 什么样的数据库能使用缓存？
   - 经常查询并且不经常改变的数据

## 13.2、Mybatis缓存

- MyBatis包含一个非常强大的查询缓存特性 ，它可以非常方便地定制和配置缓存。缓存可以极大的提升查询效率
- MyBatis系统中默认定义 了两级缓存：**一级缓存****和**二级缓存**
  - 默认情况下，只有一级缓存开启（SqlSession级别的缓存，也称为本地缓存）
  - 二级缓存需要手动开启和配置，他是基于namespace级别的缓存
  - 为了提高扩展性，MyBatis定义 了缓存接口Cache。我们可以通过实现Cache接口来定义二级缓存 

## 13.3、一级缓存

- 一级缓存也叫本地缓存：SqlSession
  - 与数据库同一次会话期间查询到的数据会放在本地缓存中
  - 以后如果需要获取相同的数据，直接从缓存中拿，没必要再去查询数据库了

测试步骤：

1. 开启日志
2. 测试在一个Session中查询两次相同记录
3. 查看日志输出![image-20231011195025369](C:\Users\LRX\AppData\Roaming\Typora\typora-user-images\image-20231011195025369.png)

**缓存失效的情况：**

1. 查询不同的东西
2. 增删改操作，可能会改变原来的数据，所以必定会刷新缓存
   ![image-20231011200030727](C:\Users\LRX\AppData\Roaming\Typora\typora-user-images\image-20231011200030727.png)
3. 查询不同的Mapper.xml
4. 手动清理缓存
   ![image-20231011200201556](C:\Users\LRX\AppData\Roaming\Typora\typora-user-images\image-20231011200201556.png)

小结：一级缓存默认是开启的，只在一次SqlSession中有效，也就是拿到连接到关闭连接这个区间段

一级缓存相当于一个Map

## 13.4、二级缓存

- 二级缓存也叫全局缓存，一级缓存作用域太低了，所以诞生了二级缓存
- 基于namespace级别的缓存，一个名称空间，对应一个二级缓存；
- 工作机制
  - 一个会话查询一条语句，这个数据就会被放在当前会话的一级缓存中
  - 如果当前回话关闭了，这个回话对应的一级缓存也就没了；但是我们想要的是，缓存关闭了，一级缓存中的数据被保存到二级缓存中
  - 新的回话查询信息，就可以从二级缓存中获取内容
  - 不同的mapper查出的数据会放在自己对应的缓存中

**步骤：**

1. 开启全局缓存

   ```xml
   <!--显示地开启全局缓存-->
   <setting name="cacheEnabled" value="true"/>
   ```

2. 在要使用二级缓存的Mapper中开启

   ```xml
   <mapper>
       <cache/>
       .....
   </mapper>
   ```

   也可以自定义参数

   ```xml
   <mapper>
       <cache eviction="FIFO"
                  flushInterval="60000"
                  size="512"
                  readOnly="true"/>
       .....
   </mapper>
   ```

3. 测试

   1. 问题：我们需要将实体类序列化，否则就会报错,如果`<cache readOnly="true"/>`就不需要序列化

      ```
      Caused by: java.io.NoSerializableException: com.xuan.pojo.User
      ```

小结：

- 只要开启了二级缓存，在同一个Mapper下就有效
- 所有的数据都会先放在一级缓存中
- 只有当会话提交，或者关闭的时候才会提交到二级缓存中

## 13.5、缓存原理

![image-20231011203855841](C:\Users\LRX\AppData\Roaming\Typora\typora-user-images\image-20231011203855841.png)



## 13.6、自定义缓存-ehcache

```
Ehcach是一种广泛使用的开源Java分布式缓存。主要面向通用缓存
```

先导包

```xml
 <dependency>
            <groupId>org.mybatis.caches</groupId>
            <artifactId>mybatis-ehcache</artifactId>
            <version>1.2.1</version>
        </dependency>
```



ehcache.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<ehcache xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:noNamespaceSchemaLocation="http://ehcache.org/ehcache.xsd"
         updateCheck="false">

    <diskStore path="./tmpdir/Tmp_EhCache"/>

    <defaultCache
            eternal="false"
            maxElementsInMemory="10000"
            overflowToDisk="false"
            diskPersistent="false"
            timeToIdleSeconds="1800"
            timeToLiveSeconds="259200"
            memoryStoreEvictionPolicy="LRU"/>

    <cache
            name="cloud_user"
            eternal="false"
            maxElementsInMemory="5000"
            overflowToDisk="false"
            diskPersistent="false"
            timeToIdleSeconds="1800"
            timeToLiveSeconds="1800"
            memoryStoreEvictionPolicy="LRU"/>
</ehcache>
```

Redis数据库来做缓存
