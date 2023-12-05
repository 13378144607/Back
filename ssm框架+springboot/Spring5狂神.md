# Spring框架

# 1.Spring

## 1.1、简介

- 2002，首次推出了Spring框架的雏形：interface21框架
- Spring框架以interface21框架为基础，2004-3-24，发布了1.0版本
- Rod Johnson是Spring Framework的创始人，悉尼大学博士，但是主页不是计算机而是音乐
- spring理念：使现有的技术更加容易使用，本身是一个大杂烩，整合了现有的技术框架
- SSH：Struct2 + Spring + Hibernate
- SSM：SpringMVC + Spring + Mybatis

官方下载地址：http://repo.spring.io/release/org/springframework/spring

```xml
<!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-webmvc</artifactId>
    <version>5.3.18</version>
</dependency>

<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-jdbc</artifactId>
    <version>5.3.18</version>
</dependency>


```

## 1.2、优点

- spring是一个开元的免费的容器
- spring是一个轻量级的、非入侵式的框架
- **控制反转（IOC），面向切面编程（AOP）**
- 支持事务的处理，对框架整合的支持

==总结：spring就是一个轻量级的控制反转（IOC）和面向切面编程（AOP）的框架==

## 1.3、组成

![image-20231012105859286](C:\Users\LRX\AppData\Roaming\Typora\typora-user-images\image-20231012105859286.png)

## 1.4、扩展

在spring官网有这个介绍：现代化的Java开发，说白了就是基于spring的开发

![image-20231012110032106](C:\Users\LRX\AppData\Roaming\Typora\typora-user-images\image-20231012110032106.png)

- Spring Boot
  - 一个 快速开发的脚手架
  - 基于SpringBoot可以快速开发单个微服务
  - 约定大于配置
- Spring Cloud
  - Spring Cloud是基于Spring Boot实现的

现在大多数公司都在使用Spring Boot进行快速开发，学习SpringBoot前提是完全掌握Spring和SpringMVC【承上启下】

**弊端：发展了太久之后，违背了原来的发展理念，配置十分繁琐，**

# 2.IOC理论推导

早期

![image-20231012113738101](C:\Users\LRX\AppData\Roaming\Typora\typora-user-images\image-20231012113738101.png)

1. UserDao 接口
2. UserDaoImpl 实现类
3. UserService 业务接口
4. UserServiceImpl 业务实现类

在我们之前的业务中，用户的需求可能会影响我们原来的代码，我们需要根据用户的需求去修改原来代码，如果代码量十分大，那么修改代码的代价十分昂贵

我们使用一个set接口实现

```java
private UserDao userDao;

    //利用set进行动态实现值的注入
    public void setUserDao(UserDao userDao) {
        this.userDao = userDao;
    }
```

- 之前的程序是主动创建对象，控制权在程序员手上
- 使用了set注入后，程序不再具有主动性，而是变成了被动的接受对象

这种思想，从本质上解决了问题，我们程序员再也不用去管理对象的创建了。系统耦合性大大降低，可以更加专注在业务实现上了。这是IOC的原型

![image-20231012113754140](C:\Users\LRX\AppData\Roaming\Typora\typora-user-images\image-20231012113754140.png)

## IOC本质

**控制反转IOC（Inversion of Control），是一种设计思想，DI（依赖注入）是实现IOC的一种方式，**也有人认为DI只是IOC的另一种说法，没有ioc程序中，我们使用面向对象编程，对象的创建与对象间的依赖关系完全硬编码在程序中，对象的创建由程序自己控制，控制反转后将对象的创建转移给第三方，个人认为所谓控制反转就是：获得依赖对象的方式反转了

**控制反转是一种通过描述（XML或者注解）并通过第三方去生产或者获取特定对象的方式，在Spring中实现控制反转的是ioc容器，其实现方法是依赖注入**

# 3.helloSpring

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">
<!--使用Spring来创建对象，在spring这些都称为Bean

    类型 变量名 = new 类型();
    Hello hello = new Hello();

    id = 变量名
    class = new 的对象;
    property 相当于给对象中的属性设置一个值
-->
    <bean id="hello" class="com.xuan.pojo.Hello">
        <property name="str" value="Spring"/>
    </bean>
</beans>
```



```java
public class MyTest {
    public static void main(String[] args) {
        //获取spring的上下文对象
        ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
        //我们的对象现在都在spring中管理了，我们要使用直接就取出来就可以了
        Hello hello = (Hello) context.getBean("hello");
        System.out.println(hello.toString());
    }
}
```



- 控制：谁来控制对象的创建，传统应用程序的对象是由程序本身控制创建的，使用spring后，对象是由spring来创建的
- 反转：程序本身不创建对象，而变成被动的接受对象
- 依赖注入：就是利用set方法来进行注入的
- IOC是一种编程思想，由主动的编程编程被动的接收
- 可以通过newClassPathXmlApplicationContext去浏览一下底层源码

# 4.IOC创建对象的方式

1. 使用无参构造创建对象，默认！

2. 假设我们要使用有参构造创建对象

   1. 下标赋值

      ```xml
      <!--  第一种方式：有参构造注入方法：下标赋值  -->
          <bean id="user" class="com.xuan.pojo.User">
              <!--    下标赋值    -->
              <constructor-arg index="0" value="xuange"/>
          </bean>
      ```

   2. 类型赋值

      ```xml
      <!--  第二种方式：通过类型创建：不建议使用!!!!，因为如果实体类中的属性类型多个都是同一个比如String的话就会出问题  -->
          <bean id="user" class="com.xuan.pojo.User">
              <constructor-arg type="java.lang.String" value="qingjiang"/>
          </bean>
      ```

   3. 通过参数名设置（推荐）

      ```xml
      <!--  第三种：直接通过参数名来设置  -->
          <bean id="user" class="com.xuan.pojo.User">
              <constructor-arg name="name" value="qingjiag"/>
          </bean>
      ```

      

总结：在配置文件加载的时候，容器中管理的对象就已经初始化了

# 5.Spring配置

## 5.1、别名（不推荐这样写别名）

```xml
<!--  别名，如果添加了别名，我们也可以使用别名获取到这个对象  -->
    <alias name="user" alias="bieming"/>
```



## 5.2、Bean的配置

```xml
<!--
       id:bean的唯一标识符，也就是相当于我们学过的对象名
       class:bean对象对应的权限定名
       name:也是别名，而且name可以同时取多个别名
      -->
    <bean id="userT" class="com.xuan.pojo.UserT" name="user2,u2 u3;u4">
        <property name="name" value="learn-spring"/>
    </bean>
```



## 5.3、import

import一般用于团队开发使用，可以将多个配置文件导入合并为一个

假设，现在项目中有多个人开发，这三个人负责不同的类开发，不同的类需要注册在不同的类bean中，我们可以利用`<import/>`将所有人的beans合并为一个总的

- 张三

- 李四

- 王五

- applicationContext.xml

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://www.springframework.org/schema/beans
          http://www.springframework.org/schema/beans/spring-beans.xsd">
      <import resource="beans.xml"/>
      <import resource="beans2.xml"/>
      <import resource="beans3.xml"/>
  
  </beans>
  ```

使用的时候，直接使用总的配置就行了

# 6.依赖注入（DI）

## 6.1、构造器注入

前面已经说过了

## 6.2、set方式注入【重点】

- 依赖注入：set注入
  - 依赖：bean对象的创建依赖于容器
  - 注入：bean对象中的所有属性，由容器来进行注入

【环境搭建】

1. 复杂类型

   ```java
   public class Address {
       private String address;
   
       public String getAddress() {
           return address;
       }
   
       public void setAddress(String address) {
           this.address = address;
       }
   }
   ```

   

2. 真实测试对象

   ```java
   public class Student {
       private String name;
       private Address address;
       private String[] books;
   
       private List<String> hobbys;
       private Map<String,String> card;
       private Set<String> games;
       private String wife;
   
       private Properties info;
   }
   ```

3. beans.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
           http://www.springframework.org/schema/beans/spring-beans.xsd">
   
           <bean id="student" class="com.xuan.pojo.Student" name="stu">
               <!--第一种：普通注入-->
               <property name="name" value="李睿轩"/>
           </bean>
   </beans>
   ```

4. 测试类

   ```java
   public class MyTest {
       @Test
       public void test(){
           ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
   
           Student student = (Student) context.getBean("stu");
   
           System.out.println(student.getName());
   
       }
   }
   
   ```

完善注入信息

```xml
<bean id="address" class="com.xuan.pojo.Address" name="adr">
    <property name="address" value="西安"/>
</bean>

<bean id="student" class="com.xuan.pojo.Student" name="stu">
    <!--第一种：普通属性注入-->
    <property name="name" value="李睿轩"/>
    <!--第二种：bean注入-->
    <property name="address" ref="address"/>

    <!--数组注入-->
    <property name="books">
        <array>
            <value>红楼梦</value>
            <value>西游记</value>
            <value>水浒传</value>
            <value>三国演义</value>
        </array>
    </property>

    <!--集合注入-->
    <property name="hobbys">
        <list>
            <value>听歌</value>
            <value>吃饭</value>
            <value>睡觉</value>
            <value>打豆豆</value>
        </list>
        <!--表示将对象引入到集合中-->
        <list>
        	<ref bean="xxx"></ref>
            <ref bean="xxx"></ref>
            <ref bean="xxx"></ref>
        </list>
    </property>

    <!--map注入-->
    <property name="card">
        <map>
            <entry key="身份证" value="123456789454664464464"/>
            <entry key="银行卡" value="12345678932145"/>
        </map>
    </property>

    <!--set-->
    <property name="games">
        <set>
            <value>王者荣耀</value>
        </set>
    </property>
    <!--null注入-->
<!--        <property name="wife" value=""/>-->
    <property name="wife">
        <null/>
    </property>
    <!--properties注入-->
    <property name="info">
        <props>
            <prop key="学号">0102</prop>
        </props>
    </property>
</bean>
```



## 6.3、其他方式

 我们可以使用p命名空间（property）和c命名空间（constructor-arg）进行注入，需要引入相应的名称空间

官方解释：

![image-20231012143354255](C:\Users\LRX\AppData\Roaming\Typora\typora-user-images\image-20231012143354255.png)



使用：

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xmlns:c="http://www.springframework.org/schema/c"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!--p命名空间注入，可以直接注入属性值-->
    <bean id="user" class="com.xuan.pojo.User" p:name="李睿轩" p:age="18"/>

    <!--c命名空间注入，可以通过构造器注入constructor-args-->
    <bean id="user2" class="com.xuan.pojo.User" c:age="18" c:name="张三"/>

</beans>
```

测试：

```java
@Test
    public void testUser(){
        ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext("userbeans.xml");

        User user = context.getBean("user2", User.class);

        System.out.println(user);

    }
}
```

注意：p命名和c命名空间不能直接使用，需要导入xml约束

```
xmlns:c="http://www.springframework.org/schema/c"
xmlns:p="http://www.springframework.org/schema/p"
```

## 6.4、bean的作用域

![image-20231012144209988](C:\Users\LRX\AppData\Roaming\Typora\typora-user-images\image-20231012144209988.png)

1. 单例模式（Spring默认机制）

   ```xml
       <bean id="user2" class="com.xuan.pojo.User" c:age="18" c:name="张三" scope="singleton"/>
   ```

2. 原型模式：每次从容器中get的时候都会产生一个新对象

   ```xml
       <bean id="user2" class="com.xuan.pojo.User" c:age="18" c:name="张三" scope="prototype"/>
   ```

3. 其余的Request、Session、application，这些只能在web开发中使用到

# 7.bean的自动装配

- 自动装配是Spring满足bean依赖一种方式
- Spring会在上下文中自动寻找，并自动给bean装配属性

在Spring中有三种装配的方式

1. 在xml中显示的配置
2. 在java中显示配置
3. 隐式的自动装配【重要】

## 7.1、测试

1. 环境搭建
   - 一个人有两个宠物

## 7.2、ByName自动装配

```xml
<bean id="cat" class="com.xuan.pojo.Cat"/>
<bean id="dog" class="com.xuan.pojo.Dog"/>
<!--
      byName:会自动在容器上下文中查找，和自己对象set方法后面的值对应的bean-id
      -->
<bean id="people" class="com.xuan.pojo.People" autowire="byName">
    <property name="name" value="小飞侠"/>
</bean>


```

==小结：==

当一个bean节点带有byName，的属性时：

1. 将查找其类中所有的set方法名，例如setCat，获得将set去掉并且首字母小写的字符串，即cat。
2. 去spring容器中寻找是否有此字符串名称id的对象。
3. 如果有，就取出注入；如果没有，就报空指针异常。

## 7.3、byType自动装配

使用autowire byType首先需要保证：同一类型的对象，在spring容器中唯一。如果不唯一，会报不唯一的异常。

```xml
<!--
byType:会自动在容器上下文中查找，和自己对象属性类型相同的bean-id
      -->
    <bean id="people" class="com.xuan.pojo.People" autowire="byType">
        <property name="name" value="小飞侠"/>
    </bean>
```

小结：

- byname的时候，需要保证所有的bean的id唯一，并且这个bean需要和自动注入的属性的set方法的小写值一致
- bytype的时候，需要保证所有bean的class唯一 ，并且这个bean需要和自动注入的属性的类型一致

## 7.4、使用注解自动装配

jdk1.5支持的注解，spring2.5就支持注解了

要使用注解须知：

1. 导入约束。context约束

2. ==配置注解的支持 `<context:annotation-config/>`==

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
   	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
   	xmlns:context="http://www.springframework.org/schema/context"
   	xsi:schemaLocation="http://www.springframework.org/schema/beans
   		https://www.springframework.org/schema/beans/spring-beans.xsd
   		http://www.springframework.org/schema/context
   		https://www.springframework.org/schema/context/spring-context.xsd">
   
   	<context:annotation-config/>
   
   </beans>
   ```

   **@Autowired**

   直接在实体类属性上用即可

   使用Autowired我们可以不用编写set方法了，前提是你这个自动装配的属性在IOC容器中存在且符合名字bytype

   java类中

   ```java
   public class User {
      @Autowired
      private Cat cat;
      @Autowired
      private Dog dog;
      private String str;
   
      public Cat getCat() {
          return cat;
     }
      public Dog getDog() {
          return dog;
     }
      public String getStr() {
          return str;
     }
   }
   
   ```

   配置文件

   ```xml
   <context:annotation-config/>
   
   <bean id="dog" class="com.kuang.pojo.Dog"/>
   <bean id="cat" class="com.kuang.pojo.Cat"/>
   <bean id="user" class="com.kuang.pojo.User"/>
   ```

   **科普：**

   **@Autowired(required=false)**

   ```xml
   ①默认情况下，@Autowired注解要求依赖项必须存在，如果找不到匹配的依赖项，则会抛出异常。
   ②使用@Autowired(required=false)时，如果找不到匹配的依赖项，Spring将不会抛出异常，而是将依赖项设置为null。这样可以避免因为某些依赖项不可用而导致程序出错。
   ③总结起来，@Autowired(required=false)的作用就是告诉Spring在注入依赖时，如果找不到匹配的依赖项可以不抛出异常，而是将依赖项设置为null。这样可以提高程序的健壮性和灵活性。
   ```

   源码
   
   ```java
   public @interface Autowired{
   	boolean required() default true;
   }
   ```

   测试代码
   
   ```java
   public class People{
       //如果显示定义了Autowired的required属性为false，说明如果这个对象没有被找到则不会报错，而是会赋值为null
       @Autowired(required = false)
       private Cat cat;
       @Autowired
       private Dog dog;
       private String name;
   }
   ```

   **@Qualifier(value = "xxx")**

   如果`@Autowired`自动装配的环境比较复杂，自动装配无法通过一个注解【@Autowired】完成的时候，我们可以使用你`@Qualifier(value = "xxx")`进行强制配合`@Autowired`使用，指定唯一的一个bean对象注入
   
   ```
   @Qualifier 是Spring框架中注解的一种，用于解决依赖注入时的歧义性问题。当存在多个匹配的依赖项时，@Qualifier 可以指定具体要注入的依赖项。
   
   在使用 @Autowired 注解进行依赖注入时，如果存在多个匹配的依赖项，Spring 会默认按类型进行注入。但是，当存在多个类型相同的依赖项时，就会出现歧义性问题。这时就可以使用 @Qualifier 注解来指定具体要注入的依赖项。
   
   @Qualifier 注解可以标记在字段、构造函数、setter 方法或方法参数上，用于给依赖项取一个特定的名称或标识。在进行依赖注入时，配合 @Autowired 注解使用，通过指定 @Qualifier 的值来告诉 Spring 要注入的是哪个具体的依赖项。
   
   举个例子，假设有两个实现了同一个接口的类：ClassA 和 ClassB。在某个目标类中需要注入一个 ClassA 的实例，而不是 ClassB 的实例。这时，可以在目标类的字段或构造函数上使用 @Autowired 和 @Qualifier(“classA”) 注解来指定注入 ClassA 的实例。
   
   总结起来，@Qualifier 注解的作用是为依赖项指定一个特定的名称或标识，用于解决依赖注入时的歧义性问题。通过和 @Autowired 注解配合使用，可以精确地选择要注入的依赖项。
   ```
   
   

   ```java
   @Autowired
       @Qualifier(value = "cat222")
       private Cat cat;
       @Autowired
       @Qualifier(value = "dog222")
       private Dog dog;
   ```
   
   ```xml
   <bean id="dog1" class="com.kuang.pojo.Dog"/>
   <bean id="dog2" class="com.kuang.pojo.Dog"/>
   <bean id="cat1" class="com.kuang.pojo.Cat"/>
   <bean id="cat2" class="com.kuang.pojo.Cat"/>
   ```
   
   **@Resource**
   
   ```
   @Resource 是Java EE提供的注解，也被Spring框架支持，用于进行依赖注入。它的作用类似于@Autowired注解，都可以自动注入依赖项。但是，@Resource 使用的是名称（name）来匹配依赖项，而不是类型。
   
   @Resource 注解可以标记在字段、方法、构造函数或方法参数上，用于指定要注入的依赖项的名称。当 Spring 容器进行依赖注入时，会根据名称来寻找匹配的依赖项，并将其注入到目标对象中。
   
   如果没有指定 name 值，默认情况下，@Resource 会根据字段或方法的名称来匹配依赖项。如果没有找到匹配的依赖项，会抛出异常。如果指定了 name 值，则会根据指定的名称来匹配依赖项。
   
   与 @Qualifier 注解相比，@Resource 更加通用，因为它不仅支持Spring框架，也支持Java EE平台。但是需要注意的是，@Resource 在注入依赖项时，是按照名称来匹配的，因此要确保依赖项的名称唯一且正确。
   
   总结一下，@Resource 注解用于进行依赖注入，按照名称来匹配依赖项。它是Java EE提供的注解，也被Spring框架支持，可以用于指定要注入的依赖项的名称。
   ```
   
   - @Resource如有指定的name属性，先按该属性进行byName方式查找装配；
   - 其次再进行默认的byName方式进行装配；
   - 如果以上都不成功，则按byType的方式自动装配。
   - 都不成功，则报异常。

实体类

```java
public class User {

   @Resource(name = "cat2")
   private Cat cat;
   @Resource
   private Dog dog;
   private String str;
}
```

beans.xml

```xml
<bean id="dog" class="com.kuang.pojo.Dog"/>
<bean id="cat1" class="com.kuang.pojo.Cat"/>
<bean id="cat2" class="com.kuang.pojo.Cat"/>

<bean id="user" class="com.kuang.pojo.User"/>

```

第二种情况，byname找不到，再bytype找

```java
@Resource
private Cat cat;
@Resource
private Dog dog;

```

```xml
<bean id="dog" class="com.kuang.pojo.Dog"/>
<bean id="cat1" class="com.kuang.pojo.Cat"/>
```

小结：

@Resource和@Autowired的区别：

- 都是用来自动装配的，都可以放在属性字段上
- @Autowired通过bytype的方式实现
- @Resource默认通过byname的方式实现，如果找不到名字则通过bytype实现
- 执行顺序不同：@Autowired通过bytype的方式实现，如果bytype不唯一再根据byname进行查找。@Resource默认通过byname方式实现

# 8.注解开发

在spring4之后，要使用注解开发，必须要保证aop的包导入了

![image-20231012190728514](C:\Users\LRX\AppData\Roaming\Typora\typora-user-images\image-20231012190728514.png)

使用注解需要导入context约束，增加注解的支持

## 1.bean

1. 配置扫描哪些包下的注解

   ```xml
   <!--指定注解扫描包-->
   <context:component-scan base-package="com.kuang.pojo"/>
   
   ```

2. 在指定包下编写类增加注解

   ```java
   @Component("user")
   // 相当于配置文件中 <bean id="user" class="当前注解的类"/>
   public class User {
      public String name = "秦疆";
   }
   
   ```

3. 测试

   ```java
   @Test
   public void test(){
      ApplicationContext applicationContext =
          new ClassPathXmlApplicationContext("beans.xml");
      User user = (User) applicationContext.getBean("user");
      System.out.println(user.name);
   }
   
   ```

   

## 2.属性如何注入

- 可以不用提供set方法，直接在直接名上添加@value(“值”)


   ```java
   @Component("user")
   // 相当于配置文件中 <bean id="user" class="当前注解的类"/>
   public class User {
      @Value("秦疆")
      // 相当于配置文件中 <property name="name" value="秦疆"/>
      public String name;
   }
   
   ```

   - 如果提供了set方法，在set方法上添加@value(“值”);


```java
@Component("user")
public class User {

   public String name;

   @Value("秦疆")
   public void setName(String name) {
       this.name = name;
  }
}
```



## 3.衍生的注解

`@Component`有几个衍生注解，我们在web开发中，会按照mvc三层架构分层 

- dao【@Repository】

- service【@Service】

- controller【@Controller】

  这四个注解功能一样都是代表将某个类注册到spring容器中，装配bean

## 4.自动装配

```
-@Autowired：自动装配通过类型。名字，默认通过bytype形式查找装配
	如果Autowired不能唯一自动装配上属性，则需要通过@Qualifier(value = "xxx")
-@Nullable	字段标记饿这个注解，说明这个字段可以为null
-@Resource	：自动装配通过名字。类型。默认通过byname形式查找装配，没有找到则通过bytype形式查找装配
```

## 5.作用域

```java
@Component
@Scope("prototype")
public class User {
    public String name;

    @Value("liruixuan")//spring的注解
    public void setName(String name) {
        this.name = name;
    }
}
```

## 6.小结

xml与注解：

- xml更加万能，适用于任何场合
- 注解不是自己的类使用不了

xml与注解最佳实践：

- xml用来管理bean

- 注解只负责完成属性的注入

- 我们在使用的过程中，只需要注意一个问题：必须让注解生效，就需要开启注解的支持

  ```xml
  <!--指定要扫描的包，这个包下的注解才会生效-->
  <context:component-scan base-package="com.xuan"/>
  <context:annotation-config/>
  ```

  

# 9.完全使用java形式配置spring

![image-20231013091624700](C:\Users\LRX\AppData\Roaming\Typora\typora-user-images\image-20231013091624700.png)

实体类

```java
//这个注解的意思就是说明这个类被spring接管了，注册到了容器中自动装配
@Component
public class User {
    private String name;

    public String getName() {
        return name;
    }

    @Value("小明")
    public void setName(String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return "User{" +
                "name='" + name + '\'' +
                '}';
    }
}
```

配置类

```java
//也会被spring容器托管，注册到容器中，本身就是一个@component
@Configuration//代表是一个配置类，和beans.xml是一样的
@ComponentScan("com.xuan.pojo")//扫描包
@Import(MyConfig2.class)//相当于配置文件中的<import/>
public class MyConfig {
    //注册一个bean就相当于之前写的一个bean标签
    //这个方法的名字就相当于bean标签中的id属性
    //这个方法的返回值就相当于bean标签中的class属性
    @Bean
    public User getUser(){
        return new User();//就是返回要注入到bean的对象
    }
}
```

测试类

```java
public class MyTest {
    @Test
    public void test(){
        //如果完全使用了配置类的方式去做，我们就只能通过AnnotationConfig上下文来获取容器，通过配置类的class对象加载
        ApplicationContext context = new AnnotationConfigApplicationContext(MyConfig.class);

        User user = context.getBean("getUser", User.class);

        System.out.println(user.getName());
    }
```

# 10.代理模式

为什么要 学习代理模式？因为这是AOP的底层

代理模式的分类：

- 静态代理
- 动态代理

## 10.1、静态代理

角色分析：

- 抽象角色：一般会使用接口或者抽象类来解决 
- 真是角色：被代理的角色
- 代理角色：代理真实角色，代理真实角色后，一般会做一些附属操作
- 客户：访问代理对象的人

代码步骤：

1. 接口

   ```java
   //租房
   public interface Rent {
       public void rent();
   }
   ```

   

2. 真实角色

   ```java
   //房东
   public class Host implements Rent{
       @Override
       public void rent() {
           System.out.println("房东要出租房子");
       }
   }
   ```

   

3. 代理角色

   ```java
   public class Proxy implements Rent{
   
       private Host host;
   
       public Proxy(){}
       public Proxy(Host host){
           this.host = host;
       }
   
       @Override
       public void rent() {
           seeHouse();
           host.rent();
           contract();
           fare();
       }
   
       //看房
       public void seeHouse(){
           System.out.println("中介带你看房");
       }
       //收中介费
       public void fare(){
           System.out.println("收中介费");
       }
       //签合同
       public void contract(){
           System.out.println("签合同");
       }
   }
   ```

   

4. 客户端访问代理角色

   ```java
   public class Client {
       public static void main(String[] args) {
           //房东要出租房子
           Host host = new Host();
           //代理，中介帮房东租房子，但是一般有一些附加操作
           Proxy proxy = new Proxy(host);
           //你不用面对房东，直接找中介即可
           proxy.rent();
       }
   }
   ```

   

代理模式好处：

- 可以使真实角色的操作更加纯粹，不用去关注一些公共的业务
- 公共业务就交给了代理角色，实现了业务的分工
- 公共业务发生扩展的时候，方便集中管理

缺点：

- 一个真实角色就会产生一个代理角色，代码量会翻倍，开发效率变低

## 10.2、加深理解

代码对应的08-demo02

关于AOP

![image-20231013111836025](C:\Users\LRX\AppData\Roaming\Typora\typora-user-images\image-20231013111836025.png)

## 10.3、动态代理

- 动态代理和静态代理角色一样
- 动态代理类是动态生成的，不是我们直接写好的
- 动态代理分为两大类：基于接口的动态代理---JDK【我们在这里使用】、基于类的动态代理---cglib
  - java字节码实现：javasist

了解两个类：Proxy：代理、InvocationHandler：调用处理程序

动态代理的好处：

- 静态代理的好处都有
- 一个动态代理类代理的就是一个接口，一般就是对应的一类业务
- 一个动态代理类可以代理多个类，只要实现了同一个接口即可

**接口**

```java
package com.xuan.demo02;

public interface UserService {
    public void add();
    public void delete();
    public void update();
    public void query();
}

```

**真实角色**

```java
package com.xuan.demo02;

//真实对象
public class UserServiceImpl implements UserService{
    @Override
    public void add() {
        System.out.println("增加了一个用户");
    }

    @Override
    public void delete() {
        System.out.println("删除了一个用户");
    }

    @Override
    public void update() {
        System.out.println("修改了一个用户");
    }

    @Override
    public void query() {
        System.out.println("查询了用户");
    }
}

```

**代理类**

```java
package com.xuan.demo04;

import com.xuan.demo03.Rent;

import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;

//我们会用这个类，自动生成代理类
public class ProInvocationHandler implements InvocationHandler {
    //被代理的接口
    private Object target;

    public ProInvocationHandler(Object target) {
        this.target = target;
    }

    //生成得到代理类——死代码
    public Object getProxy(){

        return Proxy.newProxyInstance(target.getClass().getClassLoader(),
                target.getClass().getInterfaces(), this);//这里的this就是invoke方法
    }

    //处理代理实例，并返回结果
    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        //动态代理 的本质就是使用反射机制实现
        log(method.getName());
        Object result = method.invoke(target, args);
        return result;
    }

    public void log(String msg){
        System.out.println("[debug]执行了"+msg+"方法");
    }
}

```

**测试**

```java
package com.xuan.demo04;

import com.xuan.demo02.UserService;
import com.xuan.demo02.UserServiceImpl;

public class Client {
    public static void main(String[] args) {
        //真实角色---必须有
        UserServiceImpl userService = new UserServiceImpl();

        //代理角色：不存在，创建动态代理
        ProInvocationHandler pih = new ProInvocationHandler(userService);//设置要代理的对象
        //动态生成代理类
        UserService proxy = (UserService) pih.getProxy();
        proxy.add();

    }
}

```



# 11.AOP

## 11.1、什么是AOP

![image-20231013133101448](C:\Users\LRX\AppData\Roaming\Typora\typora-user-images\image-20231013133101448.png)

![image-20231013133343510](C:\Users\LRX\AppData\Roaming\Typora\typora-user-images\image-20231013133343510.png)

## 11.2、Aop在spring中的作用

==提供声明式事务；允许用户自定义切面==

- 横切关注点：跨越应用程序多个模块的方法或者功能。即是，与我们业务逻辑无关的，但是我们需要关注的部分，就是横切关注点。如日志，安全，缓存，事务等等
- 切面（ASPECT）：横切关注点被模块化的特殊对象。即，它是一个类
- 通知（Advice）：切面必须要完成的工作。即，它是类中的一个方法
- 目标（Target）：被通知对象
- 代理（Proxy）：向目标对象应用通知之后创建的对象
- 切入点（PointCut）：切面通知执行的“地点”的定义
- 连接点（JoinPoint）：与切入点匹配的执行点

![image-20231013143502429](C:\Users\LRX\AppData\Roaming\Typora\typora-user-images\image-20231013143502429.png)

SpringAOP中，通过Advice定义横切偶记，Spring中支持5种类型的Advice

![image-20231013134133883](C:\Users\LRX\AppData\Roaming\Typora\typora-user-images\image-20231013134133883.png)

## 11.3、使用Spring实现AOP

【重点】使用AOP植入，需要导入一个依赖包

```xml
<!-- https://mvnrepository.com/artifact/org.aspectj/aspectjweaver -->
<dependency>
    <groupId>org.aspectj</groupId>
    <artifactId>aspectjweaver</artifactId>
    <version>1.9.6</version>
</dependency>
```

![image-20231013142811965](C:\Users\LRX\AppData\Roaming\Typora\typora-user-images\image-20231013142811965.png)

方式一：使用spring的API接口【主要SpringAPI接口实现】

方式二：使用自定义类实现AOP【主要是切面类定义】

方式三：使用注解实现

## 11.4完全使用注解开发aop

- 关键1：必须有配置类MyConfig里面必须有@Configuration注解和**@EnableAspectJAutoProxy**
- 关键2：aop类中必须有@Aspect注解表示这是个切面
- 关键3：然后使用切面的类必须有@Component注解表示是一个@bean
- 关键4：必须在MyConfig类中通过@Bean的形式写aop类才行

# 12.整合Mybatis

步骤：

1. 导入相关jar包
   - junit
   
       ```
       <dependency>
          <groupId>junit</groupId>
          <artifactId>junit</artifactId>
          <version>4.12</version>
       </dependency>
       
       ```
   
   - mybatis
   
       ```
       <dependency>
          <groupId>org.mybatis</groupId>
          <artifactId>mybatis</artifactId>
          <version>3.5.2</version>
       </dependency>
       
       ```
   
   - mysql数据库
   
       ```
       <dependency>
          <groupId>mysql</groupId>
          <artifactId>mysql-connector-java</artifactId>
          <version>5.1.47</version>
       </dependency>
       
       ```
   
   - spring相关的
   
       ```
       <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-webmvc</artifactId>
          <version>5.1.10.RELEASE</version>
       </dependency>
       <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-jdbc</artifactId>
          <version>5.1.10.RELEASE</version>
       </dependency>
       
       ```
   
   - aspectJaop织入器
   
       ```
       <!-- https://mvnrepository.com/artifact/org.aspectj/aspectjweaver -->
       <dependency>
          <groupId>org.aspectj</groupId>
          <artifactId>aspectjweaver</artifactId>
          <version>1.9.4</version>
       </dependency>
       
       ```
   
   - mybatis-spring【new】
   
       ```
       <dependency>
          <groupId>org.mybatis</groupId>
          <artifactId>mybatis-spring</artifactId>
          <version>2.0.2</version>
       </dependency>
       
       ```
   
2. 编写配置文件
3. 测试

## 12.1、回忆mybatis

1. 编写实体类

    ```java
    package com.kuang.pojo;
    
    public class User {
       private int id;  //id
       private String name;   //姓名
       private String pwd;   //密码
    }
    
    ```

2. 编写核心配置文件

    ```xml
    <?xml version="1.0" encoding="UTF-8" ?>
    <!DOCTYPE configuration
           PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
           "http://mybatis.org/dtd/mybatis-3-config.dtd">
    <configuration>
    
       <typeAliases>
           <package name="com.kuang.pojo"/>
       </typeAliases>
    
       <environments default="development">
           <environment id="development">
               <transactionManager type="JDBC"/>
               <dataSource type="POOLED">
                   <property name="driver" value="com.mysql.jdbc.Driver"/>
                   <property name="url" value="jdbc:mysql://localhost:3306/mybatis?useSSL=true&amp;useUnicode=true&amp;characterEncoding=utf8"/>
                   <property name="username" value="root"/>
                   <property name="password" value="123456"/>
               </dataSource>
           </environment>
       </environments>
    
       <mappers>
           <package name="com.kuang.dao"/>
       </mappers>
    </configuration>
    
    ```

3. 编写接口

    ```java
    public interface UserMapper {
       public List<User> selectUser();
    }
    
    ```

4. 编写Mapper.xml

    ```xml
    <?xml version="1.0" encoding="UTF-8" ?>
    <!DOCTYPE mapper
           PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
           "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
    <mapper namespace="com.kuang.dao.UserMapper">
    
       <select id="selectUser" resultType="User">
        select * from user
       </select>
    
    </mapper>
    
    ```

5. 测试

    ```java
    @Test
    public void selectUser() throws IOException {
    
       String resource = "mybatis-config.xml";
       InputStream inputStream = Resources.getResourceAsStream(resource);
       SqlSessionFactory sqlSessionFactory = newSqlSessionFactoryBuilder().build(inputStream);
       SqlSession sqlSession = sqlSessionFactory.openSession();
    
       UserMapper mapper = sqlSession.getMapper(UserMapper.class);
    
       List<User> userList = mapper.selectUser();
       for (User user: userList){
           System.out.println(user);
      }
    
       sqlSession.close();
    }
    
    ```

    

## 12.2、MYbatis-spring

MyBatis-Spring需要以下版本：

![image-20231017101031608](C:\Users\LRX\AppData\Roaming\Typora\typora-user-images\image-20231017101031608.png)



1. 编写数据源

   ```xml
   <bean id="datasource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
           <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
           <property name="url"
                     value="jdbc:mysql://localhost:3306/mybatis?useSSL=false&amp;useUnicode=true&amp;characterEncoding=utf8"/>
           <property name="username" value="root"/>
           <property name="password" value="040615"/>
       </bean>
   ```

2. sqlSessionFactory

   ```xml
   <!--sqlSessionFactory-->
       <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
           <property name="dataSource" ref="datasource"/>
           <!--绑定MyBatis配置文件-->
           <property name="configLocation" value="classpath:mybatis-config.xml"/>
           <property name="mapperLocations" value="classpath:com/xuan/mapper/UserMapper.xml"/>
       </bean>
   ```

3. sqlSessionTemplate（还有第二种方法代替它事件接口的类继承SqlSessionDaoSupport类使用getSqlSession就能直接获得sqlSession）

   ```xml
   <!--org.mybatis.spring.SqlSessionTemplate就是我们使用的sqlSession-->
       <bean id="sqlSession" class="org.mybatis.spring.SqlSessionTemplate">
           <!--只能使用构造器注入sqlSessionFactory，因为它没有set方法-->
           <constructor-arg index="0" ref="sqlSessionFactory"/>
       </bean>
   ```

4. 需要给接口加实现类

   ```java
   public class UserMapperImpl implements UserMapper {
       //在原来我们的所有操作都是用sqlSession执行，现在都是用sqlSessionTemplate；
       private SqlSessionTemplate sqlsession;
   
       public void setSessionTemplate(SqlSessionTemplate sqlsession) {
           this.sqlsession = sqlsession;
       }
   
       @Override
       public List<User> selectUser() {
           UserMapper mapper = sqlsession.getMapper(UserMapper.class);
           List<User> userList = mapper.selectUser();
           return  userList;
       }
   }
   ```

5. 将自己写的实现类注入到spring中

   ```xml
   <bean id="userMapperImpl" class="com.xuan.mapper.UserMapperImpl">
           <property name="sessionTemplate" ref="sqlSession"/>
       </bean>
   ```

6. 测试使用

# 13.声明式事务

## 1、回顾事务

- 要么都成功，要么都失败
- 事务在项目开发中十分重要，涉及到数据的一致性问题
- 确保完整性和一致性

事务的ACID原则

- 原子性
- 一致性
- 隔离性
  - 多个业务可能操作同一个资源，防止数据损坏
- 持久性
  - 事务一旦提交，无论系统发生什么事情，结果都不会再被影响，被持久化的写到存储器中

## 2、spring中的事务管理

Spring在不同的事务管理API之上定义了一个抽象层，使得开发人员不必了解底层的事务管理API就可以使用Spring的事务管理机制。Spring支持编程式事务管理和声明式的事务管理

- 声明式事务：AOP【重点】
  - 一般情况下比编程式事务好用
  - 将事务管理代码从业务方法中分离出来，以声明的方式来实现事务管理。
  - 将事务管理作为横切关注点，通过aop方法模块化。Spring中通过Spring AOP框架支持声明式事务管理。
- 编程式事务：需要在代码中进行事务的管理【会改变原有代码，不推荐】
  - 将事务管理代码嵌到业务方法中来控制事务的提交和回滚
  - 缺点：必须在每个事务操作业务逻辑中包含额外的事务管理代码

**使用Spring管理事务，注意头文件的约束导入 : tx**

```xml
xmlns:tx="http://www.springframework.org/schema/tx"

http://www.springframework.org/schema/tx
http://www.springframework.org/schema/tx/spring-tx.xsd">

```

**事务管理器**

- 无论使用Spring的哪种事务管理策略（编程式或者声明式）事务管理器都是必须的。
- 就是 Spring的核心事务管理抽象，管理封装了一组独立于技术的方法。

**JDBC事务**

```xml
<bean id="transactionManager"class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
       <property name="dataSource" ref="dataSource" />
</bean>

```

**配置好事务管理器后我们需要去配置事务的通知**

```xml
<!--配置事务通知-->
<tx:advice id="txAdvice" transaction-manager="transactionManager">
   <tx:attributes>
       <!--配置哪些方法使用什么样的事务,配置事务的传播特性-->
       <tx:method name="add" propagation="REQUIRED"/>
       <tx:method name="delete" propagation="REQUIRED"/>
       <tx:method name="update" propagation="REQUIRED"/>
       <tx:method name="search*" propagation="REQUIRED"/>
       <tx:method name="get" read-only="true"/>
       <tx:method name="*" propagation="REQUIRED"/>
   </tx:attributes>
</tx:advice>

```

**spring事务传播特性：**

事务传播行为就是多个事务方法相互调用时，事务如何在这些方法间传播。spring支持7种事务传播行为：

- propagation_requierd：如果当前没有事务，就新建一个事务，如果已存在一个事务中，加入到这个事务中，这是最常见的选择。
- propagation_supports：支持当前事务，如果没有当前事务，就以非事务方法执行。
- propagation_mandatory：使用当前事务，如果没有当前事务，就抛出异常。
- propagation_required_new：新建事务，如果当前存在事务，把当前事务挂起。
- propagation_not_supported：以非事务方式执行操作，如果当前存在事务，就把当前事务挂起。
- propagation_never：以非事务方式执行操作，如果当前事务存在则抛出异常。
- propagation_nested：如果当前存在事务，则在嵌套事务内执行。如果当前没有事务，则执行与propagation_required类似的操作

Spring 默认的事务传播行为是 PROPAGATION_REQUIRED，它适合于绝大多数的情况。

**配置AOP**

```xml
<!--配置aop织入事务-->
<aop:config>
   <aop:pointcut id="txPointcut" expression="execution(* com.kuang.dao.*.*(..))"/>
   <aop:advisor advice-ref="txAdvice" pointcut-ref="txPointcut"/>
</aop:config>

```

**进行测试**

```java
@Test
public void test2(){
   ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
   UserMapper mapper = (UserMapper) context.getBean("userDao");
   List<User> user = mapper.selectUser();
   System.out.println(user);
}
```

思考：

为什么需要事务？

- 如果不配置事务可能存在数据提交不一致的情况下；

  
