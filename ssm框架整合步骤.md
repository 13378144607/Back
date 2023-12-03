# 							SSM整合步骤

# 1.导入依赖

```xml
 <dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-context</artifactId>
    <version>${spring.version}</version>
</dependency>
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-aspects</artifactId>
    <version>${spring.version}</version>
</dependency>
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-aop</artifactId>
    <version>${spring.version}</version>
</dependency>
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-tx</artifactId>
    <version>${spring.version}</version>
</dependency>
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-jdbc</artifactId>
    <version>${spring.version}</version>
</dependency>

<!--springmvc的-->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-webmvc</artifactId>
    <version>${spring.version}</version>
</dependency>
<dependency>
    <groupId>jakarta.platform</groupId>
    <artifactId>jakarta.jakartaee-web-api</artifactId>
    <version>9.1.0</version>
</dependency>

<!--jsp需要的-->
<dependency>
    <groupId>jakarta.servlet.jsp.jstl</groupId>
    <artifactId>jakarta.servlet.jsp.jstl-api</artifactId>
    <version>3.0.0</version>
</dependency>
<!--json的-->
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.15.0</version>
</dependency>

<!--mybatis&数据库的-->
<dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis</artifactId>
    <version>3.5.13</version>
</dependency>
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>8.0.33</version>
</dependency>

<!--整合操作的-->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-web</artifactId>
    <version>${spring.version}</version>
</dependency>
<dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis-spring</artifactId>
    <version>3.0.2</version>
</dependency>
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid</artifactId>
    <version>1.2.20</version>
</dependency>
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <version>1.18.30</version>
</dependency>

<!-- SLF4J 依赖 -->
<dependency>
    <groupId>org.slf4j</groupId>
    <artifactId>slf4j-api</artifactId>
    <version>1.7.32</version> <!-- 版本号可能有更新，可以根据实际情况调整 -->
</dependency>

<!-- Logback 日志实现，也可以选择其他 SLF4J 的实现，比如 log4j2 -->
<dependency>
    <groupId>ch.qos.logback</groupId>
    <artifactId>logback-classic</artifactId>
    <version>1.2.6</version> <!-- 版本号可能有更新，可以根据实际情况调整 -->
</dependency>
```



# 2.配置spring-config.xml文件

- 配置mapper、service包的扫描
- 配置数据源（druid）
- 配置SqlSessionFactoryBeanName
    - 引入数据源
    - 引入mybatis-config.xml文件
    - 配置mapper.xml文件的扫描
- 配置mapper接口的扫描MapperScannerConfigure

# 3.配置springmvc-config.xml文件

- 配置controller层的扫描
- 配置静态资源过滤问题
- 配置注解驱动支持
    - 配置json字符串乱码问题解决
- 配置视图解析器

# 4.配置web.xml文件

- 配置启动web容器时查找spring-config.xml文件的初始化<context-init></context-init>
- 配置启动监听器
- 配置DispatcherServlet的Servlet，并且通过<init-param>引入springmvc-config.xml文件
- 可以配置一个encoding:utf-8的过滤器

# 5.配置mybatis-config.xml配置文件

由于spring-config.xml中已经配置了数据源和mapper的扫描所以mybatis中只需要配置以下内容：

- settings：一般是驼峰自动转换、日志工厂
- typeAlias：别名